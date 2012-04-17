---
date: '2010-02-22 23:25:09'
layout: post
legacy_url: http://railsontherun.com/2010/02/22/speed-up-your-rails-xml-responses/
slug: speed-up-your-rails-xml-responses
source: railsontherun.com
status: publish
title: Speed up your Rails XML responses
wordpress_id: '1734'
categories:
- rails
- Ruby
- railsontherun.com
- blog-post
tags:
- benchmarks
- nokogiri
- xml
---

At [work](http://www.us.playstation.com/), we have an XML API that gets quite a lot of traffic. Last week I looked into improving its performance since we are expecting more traffic soon and want to make sure our response time is optimized.

My first thought was to make sure we had  an optimized [ActiveSupport's xmlmini backend](http://api.rubyonrails.org/classes/ActiveSupport/XmlMini.html). Rails 2.3.5 fixed some issues when using [Nokogiri](http://nokogiri.org/) as a xmlmini so I switched to my favorite Ruby XML lib:

    
    ActiveSupport::XmlMini.backend = 'Nokogiri'


I run some benchmarks using ab, httperf and jmeter but the results were not that great. I was so sure that switching from [rexml](http://ruby-doc.org/stdlib/libdoc/rexml/rdoc/index.html) to [nokogiri](http://nokogiri.org/) would give me awesome results that I was very disappointed.

I was about to call [Aaron Patterson](http://tenderlovemaking.com/) (Nokogiri's author) to insult him, blame him for _why's disappearance and tell him that all his pro bono efforts were useless since my own app was not running much faster when switched to his library. As I was about to dial his number on my iPhone I had a crazy thought... maybe it was not Aaron's fault, maybe it was mine.

So I took a break went to play some fuzzball and as I was being ridiculed by Italian coworker, Emanuele, I realized that most of our API calls were just simple HTTP requests with no XML payload, just some query params. However, we were generating a lot of XML to send back to the client and AS::XmlMini only takes care of the XML parsing, not the rendering.

The XML rendering is done by [Jim Weirich](http://onestepback.org/)'s pure Ruby [builder library](http://builder.rubyforge.org/) which is vendored in Rails. Builder does a good job, but I thought that maybe a C based library might improve the speed. A coworker of mine (James Bunch) recommended to look into [faster-builder](http://github.com/codahale/faster-builder), a drop-in Builder replacement based on libxml. Unfortunately, the project doesn't seem to be maintained and I decided to look into using [Nokogiri](http://nokogiri.org/) XML builder instead. (Also, faster-builder's author doesn't like me very much while Aaron knows he's one of my Ruby heroes so asking for help could be easier)

Some people reported having tried using [Nokogiri](http://nokogiri.org/) as a XML builder but didn't see much speed improvement. Because of the amount of magic required to render a rxml template, I was not really surprised but I decided to contact Aaron and ask him if he tried using his lib instead of builder in a Rails app. [Aaron](http://www.flickr.com/photos/aaronp/57241193/) told me he gave it a try a while back and he helped me get my Rails app setup to render xml templates using [Nokogiri](http://nokogiri.org/).

The next step was simple, create a [benchmark app](http://github.com/mattetti/noko-vs-builder-benchmark) and benchmark Builder vs Nokogiri using various templates. Here are the results I got using Ruby 1.9.1 (the Ruby version we use in production) and two sets of templates:

**Builder** small template, **time per request: 15.507 [ms]** (mean)

    
    $ ab -c 1 -n 200 http://127.0.0.1:3000/benchmarks/builder_small
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/
    
    Benchmarking 127.0.0.1 (be patient)
    Completed 100 requests
    Completed 200 requests
    Finished 200 requests
    
    Server Software:        nginx/0.7.65
    Server Hostname:        127.0.0.1
    Server Port:            3000
    
    Document Path:          /benchmarks/builder_small
    Document Length:        216 bytes
    
    Concurrency Level:      1
    Time taken for tests:   3.101 seconds
    Complete requests:      200
    Failed requests:        0
    Write errors:           0
    Total transferred:      114326 bytes
    HTML transferred:       43200 bytes
    Requests per second:    64.49 [#/sec] (mean)
    Time per request:       15.507 [ms] (mean)
    Time per request:       15.507 [ms] (mean, across all concurrent requests)
    Transfer rate:          36.00 [Kbytes/sec] received
    
    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0    0   0.0      0       0
    Processing:    11   15   8.8     12      47
    Waiting:        3   15   8.9     12      47
    Total:         11   15   8.8     12      47
    
    Percentage of the requests served within a certain time (ms)
      50%     12
      66%     12
      75%     13
      80%     13
      90%     35
      95%     36
      98%     38
      99%     41
     100%     47 (longest request)


**Nokogiri** small template, **time per request: 15.354 [ms] (mean)**

    
    $ ab -c 1 -n 200 http://127.0.0.1:3000/benchmarks/noko_small
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/
    
    Benchmarking 127.0.0.1 (be patient)
    Completed 100 requests
    Completed 200 requests
    Finished 200 requests
    
    Server Software:        nginx/0.7.65
    Server Hostname:        127.0.0.1
    Server Port:            3000
    
    Document Path:          /benchmarks/noko_small
    Document Length:        238 bytes
    
    Concurrency Level:      1
    Time taken for tests:   3.071 seconds
    Complete requests:      200
    Failed requests:        0
    Write errors:           0
    Total transferred:      118717 bytes
    HTML transferred:       47600 bytes
    Requests per second:    65.13 [#/sec] (mean)
    Time per request:       15.354 [ms] (mean)
    Time per request:       15.354 [ms] (mean, across all concurrent requests)
    Transfer rate:          37.75 [Kbytes/sec] received
    
    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0    0   0.0      0       0
    Processing:    11   15   8.6     12      39
    Waiting:       11   15   8.6     12      39
    Total:         11   15   8.6     12      39
    
    Percentage of the requests served within a certain time (ms)
      50%     12
      66%     12
      75%     12
      80%     13
      90%     35
      95%     36
      98%     37
      99%     38
     100%     39 (longest request)


Running the benchmarks many times showed that Nokogiri and Builder were taking more or less the same amount of time to builder a small template.

I then decided to try a bigger template, closer to what we have in production, here are the results:

**Nokogiri** longer template, **time per request: 31.252 [ms] (mean)**

    
    $ ab -c 1 -n 200 http://127.0.0.1:3000/benchmarks/noko
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/
    
    Benchmarking 127.0.0.1 (be patient)
    Completed 100 requests
    Completed 200 requests
    Finished 200 requests
    
    Server Software:        nginx/0.7.65
    Server Hostname:        127.0.0.1
    Server Port:            3000
    
    Document Path:          /benchmarks/noko
    Document Length:        54398 bytes
    
    Concurrency Level:      1
    Time taken for tests:   6.250 seconds
    Complete requests:      200
    Failed requests:        0
    Write errors:           0
    Total transferred:      10951200 bytes
    HTML transferred:       10879600 bytes
    Requests per second:    32.00 [#/sec] (mean)
    Time per request:       31.252 [ms] (mean)
    Time per request:       31.252 [ms] (mean, across all concurrent requests)
    Transfer rate:          1711.00 [Kbytes/sec] received
    
    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0    0   0.0      0       0
    Processing:    24   31  11.3     26      62
    Waiting:       23   30  11.3     24      61
    Total:         24   31  11.3     26      62
    
    Percentage of the requests served within a certain time (ms)
      50%     26
      66%     27
      75%     27
      80%     29
      90%     54
      95%     55
      98%     58
      99%     59
     100%     62 (longest request)


**Builder**, longer template, **Time per request: 140.725 [ms] (mean)**

    
    $ ab -c 1 -n 200 http://127.0.0.1:3000/benchmarks/builder
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/
    
    Benchmarking 127.0.0.1 (be patient)
    Completed 100 requests
    Completed 200 requests
    Finished 200 requests
    
    Server Software:        nginx/0.7.65
    Server Hostname:        127.0.0.1
    Server Port:            3000
    
    Document Path:          /benchmarks/builder
    Document Length:        54376 bytes
    
    Concurrency Level:      1
    Time taken for tests:   28.145 seconds
    Complete requests:      200
    Failed requests:        0
    Write errors:           0
    Total transferred:      10947000 bytes
    HTML transferred:       10875200 bytes
    Requests per second:    7.11 [#/sec] (mean)
    Time per request:       140.725 [ms] (mean)
    Time per request:       140.725 [ms] (mean, across all concurrent requests)
    Transfer rate:          379.83 [Kbytes/sec] received
    
    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0    0   0.1      0       1
    Processing:   127  141  24.6    132     331
    Waiting:      126  139  23.6    130     328
    Total:        127  141  24.6    132     331
    
    Percentage of the requests served within a certain time (ms)
      50%    132
      66%    138
      75%    147
      80%    149
      90%    156
      95%    169
      98%    193
      99%    311
     100%    331 (longest request)


Wow, [@tenderlove](http://twitter.com/tenderlove)'s Nokogori just brought us a** 4.5X speed improvement for this specific template**.  100ms per request is probably not a big deal for most people and I have to say that Jim did a great job with Builder. However in my specific case, 100ms on a request being called thousands of times per hour is quite important.

(The [benchmark app is available on github](http://github.com/mattetti/noko-vs-builder-benchmark), feel free to fork it and benchmark your own templates)

Who would have thought that a man like this could save the day?!

[caption id="attachment_1737" align="alignleft" width="150" caption="Aaron 'Tenderlove' Patterson"][![](http://railsontherun.com/wp-content/uploads/2010/02/aaron-150x150.jpg)](http://railsontherun.com/wp-content/uploads/2010/02/aaron.jpg)[/caption]

[![](http://farm3.static.flickr.com/2470/3824959062_fb0755e665_m_d.jpg)](http://www.flickr.com/photos/aaronp/3824959062/)
  

[![](http://farm1.static.flickr.com/29/57241193_8137f2a4af_m_d.jpg)](http://www.flickr.com/photos/aaronp/57241193/)

[![](http://farm4.static.flickr.com/3289/3132124227_3ace4ec7ae_m_d.jpg)](http://www.flickr.com/photos/aaronp/3132124227/)  


**_The moral of the story is that adding a bit of tenderlove to your Ruby code can make it perform much much better!_**


**Thank you Aaron 'Tenderlove' Patterson!**
