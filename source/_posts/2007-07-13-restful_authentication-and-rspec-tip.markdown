---
date: '2007-07-13 05:55:00'
layout: post
legacy_url: http://railsontherun.com/2007/07/13/restful_authentication-and-rspec-tip/
slug: restful_authentication-and-rspec-tip
source: railsontherun.com
status: publish
title: restful_authentication and RSpec tip
wordpress_id: '53'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- authentication
- BDD
- helper
- plugin
- restful
- restful_authentication
- RSpec
- tests
- tip
---

[resftul_authentication](http://svn.techno-weenie.net/projects/plugins/restful_authentication/) is a great plugin, but not always resftful… especially if like me you dropped unit test for RSpec.





![](http://myskitch.com/matt_a/p-restful_24x40.jpg__jpeg_image__2224x1475_pixels__-_scaled__43__-20070712-224940.jpg)





I have a set of RSpec examples I use all the time on all my projects requiring restful_authentication, obviously I always end up tweaking them because each application has its specific needs.





However, I was recently asked _what’s the way of dealing with controllers specs when a controller has a limited access ?_





Let’s look at the following controller for instance:




    
    <code>class AssetController < ApplicationController
     before_filter :login_required, :except => [ :index, :show ]
    </code>





my assets_controller_spec.rb file will have the following code:




    
    <code>describe AssetController, "handling GET /assets/new" do
    
      before do
        @asset = mock_model(Asset)
        Asset.stub!(:new).and_return(@asset)
      end
    
      def do_get
        get :new
      end
    
      it "should be successful" do
        do_get
        response.should be_success
      end
    
    end
    </code>





and guess what… it will fail! Why? simply because we are trying to access an action requiring login. Right, so what’s the best way of dealing with this issue and get our test to pass?





The best solution I found was to use a simple helper that I put in my spec_helper.rb file




    
    <code>def login_as(user)
      case user
        when :admin
          @current_user = mock_model(User)
          User.should_receive(:find_by_id).any_number_of_times.and_return(@current_user)
          request.session[:user] = @current_user
        else
          request.session[:user] = nil
      end
    end
    </code>





You might wonder why I use when :user, well, in a lot of the application I’m working on, I have different levels of access and  I want to tests different accounts so I the case conditional loop.





Anyway, let’s implement our helper in our previous RSpec example:




    
    <code>describe AssetController, "handling GET /assets/new" do
    
      before do
        login_as :admin
        @asset = mock_model(Asset)
        Asset.stub!(:new).and_return(@asset)
      end
    
      def do_get
        get :new
      end
    
      it "should be successful" do
        do_get
        response.should be_success
      end
    
    end
    </code>





and there you go, now your example passes properly ;)





Have fun
