---
date: '2009-05-17 21:31:14'
layout: post
legacy_url: http://merbist.com/2009/05/17/couchdb-with-couchrest-in-5-minutes/
slug: couchdb-with-couchrest-in-5-minutes
source: merbist.com
status: publish
title: CouchDB with CouchRest in 5 minutes
wordpress_id: '492'
categories:
- ruby
- merbist.com
- blog-post
tags:
- couchdb
- couchrest
- ruby
---

The other night, during our monthly [SDRuby meetup](http://sdruby.com), lots of people were very interested in learning more about CouchDB and Ruby. I tried to show what Couch was all about but I didn't have time to show how to use CouchDB with Ruby.
Here is me trying to do that in 10 minutes or less. I'll assume you don't have CouchDB installed.

Install CouchDB, if you are on MacOSX, you are in luck, download and unzip the standalone package called [CouchDBX](http://janl.github.com/couchdbx/).
That's it you have couch ready to go, press play and play with the web interface.

Next, let's write a quick script. Let's say we want to write a script that manages your contacts.

First, let's install CouchRest:

`
$ sudo gem install couchrest
`

Now, let's open a new file and write our script.

In line 4 and 5 we are just setting up the server(by default, localhost is being used). If the database doesn't exist, it will get created.

` SERVER    = CouchRest.new `

`DB           = SERVER.database!('contact-manager') `

Then, we define your 'model', we set the default database to use and define a list of properties. Properties are not required, but they generate getters and setters for you. They are also used to set default values and validate your model.  Line 11 shows how to use an alias that will provider a getter and a setter for the property name and the alias name:

`property :last_name, :alias => :family_name`

Line 14 does something that might seem strange at first. We are casting the address property as an instance of the Address class.  Here is what the implementation of the Address class could look like: 

Address is just an instance of Hash with some extra methods provided by the CouchRest::CastedModel module. (If you wonder why it's called CastedModel instead of the more grammatically correct CastModel, the answer is simple: I suck at English grammar :p )

So here is a quick example of how to use a 'CastedModel':


That's part of what's great with CouchDB, you don't need to worry too much about storage. Just define your properties, cast to models if needed and save everything as a document.

For more examples checkout the [CouchRest spec fixtures](http://github.com/mattetti/couchrest/tree/a4e6713aeb04721604553bb03475b11912a6e1ff/spec/fixtures/more) and the [examples](http://github.com/mattetti/couchrest/tree/85079a54d98ea90ecbab31cba319f0971904e9a6/examples).

To learn more about couchdb, read the (free) online draft of the [CouchDB book](http://books.couchdb.org/relax/) and of course you probably should read the [CouchRest source on GitHub](http://github.com/mattetti/couchrest).
