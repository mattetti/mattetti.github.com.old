---
layout: post
title: "Using Go vs Ruby for web APIs"
date: 2013-06-23 09:50
comments: true
categories: 
- ruby
- golang
- apis
- performance 
---

A few days ago, I was wondering if using [Go](http://golang.org/) would be worth it when developing new web APIs.
I obviously knew that Go would be faster than Ruby, but I wasn't sure
how much faster. I also wondering about the amount of work required to
write get a full API implemented.

I therefore wrote the same web API in Ruby (using Rails) and in Go (at
first using Revel and then rewriting it without a framework since Go's
std lib have everything one might need).
The API spec was simple:
* extract an authorization token contained in the request header
* use the token to query a MySQL database
* respond by sending back the MySQL row in json format
* return 401 if the token isn't value

I didn't try to optimize the Ruby code, nor the Go code. The idea wasn't
to get precise benchmark results, the goal was to get an idea of how
much faster Go was in a real life situation. The other goal was to
evaluate the amount of work needed to write web APIs in Go for someone
who already knows the language.

At the end of the day the API implemented in Go is more than 50x faster than
the Ruby version. Interesting enough, writing the code and tests for the
Go API was pretty close to the Ruby experience (more on that later).
50X performance gain, including high concurrency support might be a very
good argument to start using some Go when it makes sense.

I documented my experiment on [Google+](https://plus.google.com/101114877505962271216/posts/PeZk8FY3PWY), click the following screenshot to read more.

[![Matt Aimonetti's Go vs Ruby post on Google+](/images/matt_aimonetti-golang_vs_ruby_api_exp.png)](https://plus.google.com/101114877505962271216/posts/PeZk8FY3PWY)

