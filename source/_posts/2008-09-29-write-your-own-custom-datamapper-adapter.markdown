---
date: '2008-09-29 00:38:46'
layout: post
slug: write-your-own-custom-datamapper-adapter
status: publish
title: Write your own custom DataMapper adapter
wordpress_id: '91'
categories:
- News
- Tutorial
tags:
- adapter
- datamapper
- google video
- Tutorial
---

If you read this blog, you probably know that [Merb](http://merbivore.com)'s best [ORM](http://en.wikipedia.org/wiki/Object-relational_mapping) friend is [DataMapper](http://datamapper.org/).

[caption id="" align="alignleft" width="240" caption="compounds in basil oil have potent antioxidant and is used for supplementary treatment of stress"][![compounds in basil oil have potent antioxidant and is used for supplementary treatment of stress](http://farm1.static.flickr.com/55/190457943_8b93fda9e6_m.jpg)](http://flickr.com/photos/darn/190457943/)[/caption]

Merb works very well with ActiveRecord and Sequel but most of the Merbivores get excited about [DataMapper](http://datamapper.org/).

DataMapper has a lot of cool stuff going for it. I'm planning on writingi few articles about what I particularily like with DM and some of the misconceptions.

I'm going to give a talk about DataMapper during [MerbCamp](http://merbcamp.com) and something I want to cover is the fact that you can write DM adapters for virtually anything. From an adapter for couchdb (available in dm-more) to an [adapter for SalesForce API](http://github.com/wycats/dm-adapters/tree/master/salesforce). That's the kind of stuff that gets me excited, a bit like what [Ambition](http://ambition.rubyforge.org/) does but built-in in DM.

So, I decided to take some advise from [Yehuda](http://yehudakatz.com/) and dkubb and wrote my own adapter for [Google Video](http://video.google.com). I had just finished a [gem to retrieve google videos](http://github.com/mattetti/gvideo) for a given google user and thought it would be a perfect exercise to mix a [http-scraper](http://en.wikipedia.org/wiki/Screen_scraping) with a DM adapter.



Based on the advise I received, I started by defining the API I want to use:



I matched the API calls to the underlying methods I would need to make. I then modified my original gem to support conditional calls.

Once that was done I implemented the required methods for my models to support the Model.first and Model.all calls with conditions.

A custom adapter inherits from [AbstractAdapter](http://github.com/sam/dm-core/tree/master/lib/dm-core/adapters/abstract_adapter.rb) and can define the default adapter methods:

    
    
    <div id="LC9" class="line">Â Â Â Â Â Â <span class="k">def</span> <span class="nf">create</span><span class="p">(</span><span class="n">resources</span><span class="p">)</span></div>
    <div id="LC10" class="line">Â Â Â Â Â Â Â Â <span class="k">raise</span> <span class="no">NotImplementedError</span></div>
    <div id="LC11" class="line">Â Â Â Â Â Â <span class="k">end</span></div>
    <div id="LC13" class="line">Â Â Â Â Â Â <span class="k">def</span> <span class="nf">read_many</span><span class="p">(</span><span class="n">query</span><span class="p">)</span></div>
    <div id="LC14" class="line">Â Â Â Â Â Â Â Â <span class="k">raise</span> <span class="no">NotImplementedError</span></div>
    <div id="LC15" class="line">Â Â Â Â Â Â <span class="k">end</span></div>
    <div id="LC17" class="line">Â Â Â Â Â Â <span class="k">def</span> <span class="nf">read_one</span><span class="p">(</span><span class="n">query</span><span class="p">)</span></div>
    <div id="LC18" class="line">Â Â Â Â Â Â Â Â <span class="k">raise</span> <span class="no">NotImplementedError</span></div>
    <div id="LC19" class="line">Â Â Â Â Â Â <span class="k">end</span></div>
    <div id="LC21" class="line">Â Â Â Â Â Â <span class="k">def</span> <span class="nf">update</span><span class="p">(</span><span class="n">attributes</span><span class="p">,</span> <span class="n">query</span><span class="p">)</span></div>
    <div id="LC22" class="line">Â Â Â Â Â Â Â Â <span class="k">raise</span> <span class="no">NotImplementedError</span></div>
    <div id="LC23" class="line">Â Â Â Â Â Â <span class="k">end</span></div>
    <div id="LC25" class="line">Â Â Â Â Â Â <span class="k">def</span> <span class="nf">delete</span><span class="p">(</span><span class="n">query</span><span class="p">)</span></div>
    <div id="LC26" class="line">Â Â Â Â Â Â Â Â <span class="k">raise</span> <span class="no">NotImplementedError</span></div>
    <div id="LC27" class="line">Â Â Â Â Â Â <span class="k">end
    </span></div>


Since my adapter only needs to read data, I just had to implement #read_one and #read_many. I implemented a #read private method accessed by #read_one and #read_many as you can see [here](http://github.com/mattetti/dm-gvideo-adapter/tree/master/lib/dm-gvideo-adapter.rb#L29-60).

Because DM offers a clean and consistent API, things were pretty easy and you can check the [specs](http://github.com/mattetti/dm-gvideo-adapter/tree/master/spec/dm-gvideo-adapter_spec.rb) to have a better understanding of how things are expected to work.

As you can see with less than 80LOC, I implemented an adapter that I can use and reuse cleanly in my apps. And on top of that, the adapter uses a standard API known by everyone using DM.

**Even though, this adapter has a very limited scope, I hope this example will inspire you and at your turn, will write some more cool adapters to share with the rest of us.**


## UPDATE:


Sam Smoot, also known as Mr DataMapper made a very good comment. I should explain a bit more how you create a collection of objects to return when a user does a .all or .first call.

The whole collection creation is a bit strange at first.

In my read method, "set" is a DataMapper::Collection instance that is passed by the #read_one or #read_many method.

The collection needs to be loaded with an array of ordered params.

For instance in this case, to create a Video collection I need to load the collection with an ordered array of params like docid, title etc..Â  However, some params might be lazy loaded and in some instance, the query might be in the form of Video.all(:fields => :title)

To figure out what fields/params we need, I used:

    
    <span class="n">properties</span> <span class="o">=</span> <span class="n">query</span><span class="o">.</span><span class="n">fields
    </span>


Which retrieves the required fields. Once you have the fields you need to return the structured data and that's when I used the #result_values method which basically loop through the fields and retrieves the data by sending the param as a method to the result:

    
    <span class="n">properties</span><span class="o">.</span><span class="n">map</span> <span class="p">{</span> <span class="o">|</span><span class="nb">p</span><span class="o">|</span> <span class="n">result</span><span class="o">.</span><span class="n">send</span><span class="p">(</span><span class="nb">p</span><span class="o">.</span><span class="n">field</span><span class="p">(</span><span class="n">repository_name</span><span class="p">))</span> <span class="p">}</span>


The values once retrieved get loaded, but here is a trick I took from wycat's saleforce API:

    
    <span class="n">arr</span> <span class="p">?</span> <span class="n">set</span><span class="o">.</span><span class="n">load</span><span class="p">(</span><span class="n">values</span><span class="p">)</span> <span class="p">:</span> <span class="p">(</span><span class="k">break</span> <span class="n">set</span><span class="o">.</span><span class="n">load</span><span class="p">(</span><span class="n">values</span><span class="p">,</span> <span class="n">query</span><span class="p">))
    </span>


This snippet is quite simple if we set arr as true, that means we want to return an array so we will keep on looping (used by read_more when we want to return an array of objects). Otherwise we use the break operator which will stop the loop but also return a value. (yes, Ruby is awesome). By returning a single object we do exactly what's expected and don't have to call #first on a returned array. Pretty slick
