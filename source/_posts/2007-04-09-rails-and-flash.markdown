---
date: '2007-04-09 07:41:00'
layout: post
legacy_url: http://railsontherun.com/2007/04/09/rails-and-flash/
slug: rails-and-flash
source: railsontherun.com
status: publish
title: Restful Rails and Flash
wordpress_id: '7'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- actionScript
- adobe
- BDD
- bundle
- failure
- flash
- flex
- rails
- REST
- RSpec
- scaffhold_resource
- spec
- textmate
---

With the recent Buzz around [Adobe Apollo](http://labs.adobe.com/wiki/index.php/Apollo) I figured out that since I recently switched to Mac and that I didn’t try the latest Flex upgrade, I should try [Flex 2.01](http://www.adobe.com/products/flex/) for Mac. 





I really like what Adobe did with Flex, Unit testing, better accessibility, etc… but one thing I regret, it’s getting closer and closer to Java and [AS3 syntax](http://flexblog.faratasystems.com/?p=115) is a pain to use when you got used to Ruby. 





Anyway, what I really wanted to do was to have Flash quickly access my Restful Rails app. The adobe guys came up with a [RoR Ria SDK](http://code.google.com/p/rubyonrails-ria-sdk-by-adobe/) but well….   it only covers Flex and requires FlashPlayer 9.
I also found some great [tutorials](http://blog.vixiom.com/2006/08/23/flash-remoting-for-rails-tutorial/) on how to use the efficient  [AMF messaging protocol](http://osflash.org/documentation/amf) with Rails using the [WebOrb for Rails plugins](http://www.themidnightcoders.com/weborb/rubyonrails/index.htm)





All that was really nice and I had fun, but it was an overkill for what I wanted to do. Let me show you how in less than 5 minutes how you can access you Rails Model from Flash and add some new item directly from Flash.





Create your new Rails app and use the script/generate scaffold_resource command to generate your Event Model.




    
    <code>script/generate scaffold_resource Event
    </code>





It should create all that for you:




    
    <code>exists  app/models/
      exists  app/controllers/
      exists  app/helpers/
      create  app/views/events
      exists  test/functional/
      exists  test/unit/
      create  app/views/events/index.rhtml
      create  app/views/events/show.rhtml
      create  app/views/events/new.rhtml
      create  app/views/events/edit.rhtml
      create  app/views/layouts/events.rhtml
      identical  public/stylesheets/scaffold.css
      create  app/models/event.rb
      create  app/controllers/events_controller.rb
      create  test/functional/events_controller_test.rb
      create  app/helpers/events_helper.rb
      create  test/unit/event_test.rb
      create  test/fixtures/events.yml
      exists  db/migrate
      create  db/migrate/001_create_events.rb
      route  map.resources :events
    </code>





let’s edit the migration file:
db/migrate/001_create_events.rb




    
    <code>class CreateEvents < ActiveRecord::Migration
      def self.up
        create_table :events do |t|
          t.column :title, :string
          t.column :description, :string
          t.column :location, :string
            t.column :starts_at, :datetime
            t.column :ends_at, :datetime
        end
      end
    
      def self.down
        drop_table :events
      end
    end
    </code>





And let's add some fixtures:
test/fixtures/events.yml




    
    <code>meeting:
      id: 1
      title: Meeting
      description: Boring meeting with the whole staff
      location: conference room
      starts_at: 2007-11-02 09:00:00
      ends_at: 2007-11-02 10:30:00
    Joe_bday:
      id: 2
      title: Joe Bday Party
      description: Come and celebrate Joe's Birthday
      location: Lapin Agile Pub
      starts_at: 2007-09-07 20:00:00
      ends_at: 2007-09-07 23:30:00
    </code>





Ok, now simply migrate your database,load the fixtures and start the webrick:




    
    <code>rake db:migrate
    rake db:fixtures:load
    script/server
    </code>





Great, we are done with Rails.  





Let's launch Flash





Create a new Flash document and create a new .As fie in TextMate (or your favorite editor). We'll write a quick ActionScript class to access Rails.




    
    <code>class Restfulflash{
        public var gateway:String;
    
        function Resftfulflash(gateway:String){
            this.set_gateway(gateway);
        }
        public function set_gateway(gateway:String){
            this.gateway = gateway;
            trace("gateway set to:"+gateway);
        }
    
        public function get(model, callback){
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
            railsRequest.contentType="application/xml";
            railsRequest.sendAndLoad(this.gateway+model, railsReply);
            delete railsRequest;
        }
    
        public function create(model, newItem){
            railsRequest.onLoad = function(success){
                    trace("Item creation success: " + success);
                    trace(this);
            };
            var railsRequest:XML = new XML();
            railsRequest.parseXML(newItem);
            railsRequest.contentType="application/xml";
            railsRequest.sendAndLoad(this.gateway+model+'/create/', railsRequest,'POST');
            delete railsRequest;
        }
    }
    </code>





Save this file in the same directory as your .fla file  





In your fla file add:




    
    <code>// Create a XML object to hold the events from our Rails app
    rails_events = new XML();
    
    // Prepare the connection to Rails (it would be nicer to do that in 1 step, but to make things clearer i decided to do it in 2)
    var rails:Restfulflash = new Restfulflash();
    rails.set_gateway("http://localhost:3000/");
    
    // Get the events from rails and load the result in the rails_event XML object.
    rails.get('events', rails_events);
    trace(rails_events);
    
    // Let's create a new event
    newEvent = new XML('<event><description>Spend some time with Grandma before its too late</description><ends-at type="datetime">2007-11-02T18:30:00-07:00</ends-at><id type="integer">1</id><location>Paris, France</location><starts-at type="datetime">2007-11-02T16:00:00-07:00</starts-at><title>Visit Grandma</title></event>&#8217;)
    rails.create(&#8216;events&#8217;, newEvent);
    
    // Verify  that the event was added
    rails.get(&#8216;events&#8217;, rails_events);
    trace(rails_events);
    </code>





There you go, you have all the events provided to you by Rails nicely prepared in an easy to parse XML object. You can bind the results to a Datagrid or display the info the way you want it. Ohh and by the way, we just added a new Event to the database… easy, isn’t it?  The code is a bit dirty but it’s still a good example why you need to use REST and how easy it is to get Flash to talk with Rails. (I strongly encourage that you also look at the very good WebOrb plugin for Rails)
