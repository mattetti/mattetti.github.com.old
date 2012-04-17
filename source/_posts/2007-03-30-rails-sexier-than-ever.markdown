---
date: '2007-03-30 15:43:00'
layout: post
legacy_url: http://railsontherun.com/2007/03/30/rails-sexier-than-ever/
slug: rails-sexier-than-ever
source: railsontherun.com
status: publish
title: Rails, sexier than ever.
wordpress_id: '158'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- migration
- plugin
- rails
---

3 days ago, [Edge was patched](http://dev.rubyonrails.org/ticket/7932) to add a nicer syntax to our dear migrations.





Instead of doing:  

    create_table :people do |t|
      t.column :name, :string
      t.column :age,  :integer
    end
You can now do:
    create_table :people do |t|
      t.name :string
      t.age  :integer
    end





While this is great, I found out that [someone](http://err.lighthouseapp.com/projects/466-plugins) came up with an even nicer way of creating your migration, actually, a sexier way of doing migrations, and we all like sexy, don’t we? Otherwise we would not use Rails, would we?





Anyway thanks to this new plugin, your old Migration:




    
    <code>class UpdateYourFamily < ActiveRecord::Migration
      create_table :updates do |t|
        t.column :user_id,  :integer
        t.column :group_id, :integer
        t.column :body,     :text
        t.column :type,     :string
    
        t.column :created_at, :datetime
        t.column :updated_at, :datetime
      end
    
      def self.down
        drop_table :updates
      end
    end
    </code>





can look sexier than ever:
    class UpdateYourFamily < ActiveRecord::Migration
      create_table :updates do
        foreign_key :user
        foreign_key :group




    
    <code>    text   :body
        string :type
    
        timestamps!
      end
    
      def self.down
        drop_table :updates
      end
    end
    </code>





Note that I didn’t test the code above and the self.up method seems to be missing, for more information, check the [official post](http://errtheblog.com/post/2381) to give it a try:
    ./script/plugin install svn://errtheblog.com/svn/plugins/sexy_migrations
