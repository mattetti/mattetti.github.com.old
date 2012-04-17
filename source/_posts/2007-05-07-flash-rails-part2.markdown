---
date: '2007-05-07 22:15:00'
layout: post
legacy_url: http://railsontherun.com/2007/05/07/flash-rails-part2/
slug: flash-rails-part2
source: railsontherun.com
status: publish
title: Rails/Flash, part 2
wordpress_id: '13'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- adobe
- flash
- rails
- respond_to
- REST
- restful
- xml
---

In [part I](http://www.railsontherun.com/2007/4/9/rails-and-flash) I explained how to access Rails data from Flash.





However Yves aka Kadoudal was wondering what I did with Rails to return the event record:





Rails.get('events', rails_events); how rails returns the event record as we don't call a controller/action ... ? I believe doing it RESTFul it's depending upon your route ? is it not?





We are actually calling a controller/action If you look at the Restfulflash class, you'll notice a function called get




    
    <code>public function get(controller, callback){
            var railsReply:XML = new XML();
            railsReply.ignoreWhite = true;
            railsReply.onLoad = function(success:Boolean){
                if (success) {
                        trace ('Rails responded: '+railsReply);
                        callback.text = railsReply;
                } else {
                        trace ('Error while waiting for Rails to reply');
                   callback.text = 'error';
                }
            }
            var railsRequest:XML = new XML();
            railsRequest.contentType='application/xml';
            railsRequest.sendAndLoad(this.gateway+controller, railsReply);
            delete railsRequest;
        }
    </code>





What's really interesting are the following 2 lines:





railsRequest.contentType="application/xml";
railsRequest.sendAndLoad(this.gateway+controller, railsReply);





I'm preparing a request that I will send to my gateway mentioning the controller name. In the previous example I was calling the 'events' controller: http://mysite.fr/events because I'm using REST and because my request is a 'get' Rails will call the index action.





for more info on [RESTful design](http://www.softiesonrails.com/2007/4/10/rest-101-part-3-just-call-me-the-repo-man) check the nice series available from the [softies on rails blog](http://www.softiesonrails.com)





Let's just look at my code and see what's up with rails magic :)





Here is my index action sending you back a different object based on the header of your request.




    
    <code>class EventsController < ApplicationController
      # GET /events
      # GET /events.xml
      def index
        @events = Event.find(:all)
    
        respond_to do |format|
          format.html # index.rhtml
          format.xml  { render :xml => @events.to_xml }
          format.json { render :json => @events.to_json }
        end
      end
    </code>





That's it, and it was automatically created for you by the script/generate scaffold_resource command :)





Since I'm answering Yves' questions, let's look at his final question:





problem : what if the xml returned is not a record, but it has to be prepared by Rails...
I explain, right now my rails view .. I have a javascript object passing all the info via JS
I would like to replace the datasource file by a direct call to rails from Flash, so this param will disappear... so (correct me if I am wrong)
    1- I need to pass a user_id value in JS to Flash in the script...
    2- Flash need to send a request to Rails (controller/action/id to prepare the xml... ) to get in return a correct xlm object to be displayed





1- I'm not sure why you need to pass the user_id from JS to Flash but I guess Flash doesn't know what user to query?? (anyway that would work)





2- If you are using REST, Flash just needs make a get call to /controller/id Make sure to set the request header type as xml, like I did in my function railsRequest.contentType="application/xml"; Otherwise I believe (but didn't try) that you can call /events/1.xml where 1 id the id of the event you want to retrieve.





By the way, it will call the show action from the events controller which looks like that:




    
    <code>def show
      @event = Event.find(params[:id])
    
      respond_to do |format|
        format.html # show.rhtml
        format.xml  { render :xml => @event.to_xml }
      end
    end
    </code>
