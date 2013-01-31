---
layout: post
title: "Real life concurrency in Go"
date: 2012-11-27 10:08
comments: true
categories: 
- Go
- Golang
---

The structure of a programming language reflects the challenges and solutions the
designers decided to address. Each designer coming with his/her own background
decides to tackle some specific issues in a novel way and/or often
decides to borrow existing paradigms from other languages.
We can't, then, fairly judge a language without understanding
what problem the language designer was trying to address.

Today we are going to look at [Google's Go language](http://golang.org/).
Go approaches concurrency from an interesting view point. But instead of digging
into the history and reasoning which led to this approach, I'd like to
show you the language constructs by actually writing real life code.

## Fetching web resources concurrently

The following example is taken from my recent presentation [Ruby vs. the World](/posts/2012/11/02/rubyconf-2012-ruby-vs-the-world/). I explored a few programming languages and
showed how they changed my Ruby.

To show how [Go](http://golang.org/) addresses concurrency, I decided to build a 
program which would concurrently fetch various web resources, wait for all of
them to be fetched, then process them all at once. In other
programming languages, we could have used [threads](http://en.wikipedia.org/wiki/Thread_(computing)) and a [semaphore](http://en.wikipedia.org/wiki/Semaphore_(programming)), [actors](http://en.wikipedia.org/wiki/Actor_model) or
[callbacks](http://en.wikipedia.org/wiki/Callbacks). Go's approach is [slightly different](http://en.wikipedia.org/wiki/Communicating_sequential_processes), let's walk through the
code together.

The first part of our code gets us setup:

```go
package main

import (
	"fmt"
	"net/http"
	"time"
)

var urls = []string{
	"http://www.rubyconf.com/",
	"http://golang.org/",
	"http://matt.aimonetti.net/",
}
```

The code above names our package then imports a few standard libraries that we are going to need. It then defines an array/slice of strings representing the urls we are going to fetch.

Next we define a type we will use a bit later:

```go
type HttpResponse struct {
	url      string
	response *http.Response
	err      error
}
```

You can think of a struct type as a simple representation of a class. Technically, we are defining a structure with some typed attributes. We will later on, create instances of this defined type.

Go implements OOP [slightly differently](http://golang.org/doc/go_faq.html#Is_Go_an_object-oriented_language) than other languages.

> Methods in Go are more general than in C++, Java: they can be defined for any sort of data, even built-in types such as plain, “unboxed” integers. They are not restricted to structs (classes).

We can therefore define methods/functions for any type of data,
including "any/all" types.
This approach to types is called [structural typing](http://en.wikipedia.org/wiki/Structural_type_system). 

Here is the code:

```go
func asyncHttpGets(urls []string) []*HttpResponse {
	ch := make(chan *HttpResponse)
	responses := []*HttpResponse{}
	for _, url := range urls {
		go func(url string) {
			fmt.Printf("Fetching %s \n", url)
			resp, err := http.Get(url)
			ch <- &HttpResponse{url, resp, err}
		}(url)
	}

	for {
		select {
		case r := <-ch:
			fmt.Printf("%s was fetched\n", r.url)
			responses = append(responses, r)
			if len(responses) == len(urls) {
				return responses
			}
		case <-time.After(50 * time.Millisecond):
			fmt.Printf(".")
		}
	}
	return responses
}
```

This is the meat of our application. And there is quite a lot of going
on in just a few lines. Assuming you aren't familiar
with Go, I'll walk though the code.

Let's start with the signature:

* the function is named `asyncHttpGets`
* it takes an argument named`urls` which is an "array" of strings (I used quotes around the word
array because it's technically what Go calls a slice)
* it returns an "array" of `HttpResponse` pointers

Then in the function body:

We start by creating an instance of a `channel` and assigning it to the
`ch` variable name. Think of a channel as a pipe like in unix.  We can write to and read from that channel.

In the next line we create an empty instance of a slice containing pointers to
`HttpResponse` objects.

Then, using the `for range` language construct, we iterate through our `urls`, storing the current value being used into the scoped variable `url`. The `url` is then available within the block/lambda/closure marked by the curly braces.

Now this is where the async construct comes in. Using the `go`
keyword, we define an anonymous function that takes a string argument representing a
url.

The function prints this string, then uses the `net/http`
library to fetch the web resource. We use the returned data to create an
instance of our `HttpResponse` type and send it to the channel.

This part gets a bit confusing because I reused the name `url`. We call this
anonymous function right away passing it the `url` variable set
by the loop.

You might wonder why we bother to create an anonymous function and
call it right away instead of just executing the code directly.
The `go` keyword executes the code that is passed in as a *goroutine* which is well explained [here](http://golang.org/doc/effective_go.html#goroutines)

> A goroutine is a function executing concurrently with other goroutines in the same address space. It is lightweight, costing little more than the allocation of stack space. And the stacks start small, so they are cheap, and grow by allocating (and freeing) heap storage as required.

In other words, you start a *goroutine* and you let the "system" handle
how it wants to deal with the low level details. Technically, goroutines 
might run in one or multiple threads, but you don't need to know.
We trigger each http fetch in a separate goroutine
and then each response is pushed down the channel.

The second block of code begins with another `for` loop containing a switch/case statement.
The case statement checks if something is
in the channel. If there is something, we...

* allocate the data to the `r` variable
* print the resource's url 
* append the resource to the slice we created at the beginning of the function. 

If the length of the array is the same as the length of all urls we want to fetch, we are done
fetching all our resources and can return.
While still waiting for responses, we print a dot every 50ms.

**Update:**
In the first version of this blog post I had used a 'default' case
statement to print the dot and sleep for 50ms so the loop wouldn't be
too tight and the concurrency effect was more obvious. But some
[HN comments](http://news.ycombinator.com/item?id=4837919) pointed out that it wasn't needed and I shouldn't block.
For reference here is what I had before (don't use this code):

```go
	for {
		select {
		case r := <-ch:
			fmt.Printf("%s was fetched\n", r.url)
			responses = append(responses, r)
			if len(responses) == len(urls) {
				return responses
			}
		default:
			fmt.Printf(".")
			time.Sleep(50 * time.Millisecond)
		}
	}
``` 
Thank you HackerNews.



With that code constructed, our `main` can make use of it like this:

```go
func main() {
	results := asyncHttpGets(urls)
	for _, result := range results {
		fmt.Printf("%s status: %s\n", result.url,
                 result.response.Status)
	}
}
```

Compiling and running the code look like this:

```bash
$ go build concurrency_example.go && ./a.out 
.Fetching http://www.rubyconf.com/
Fetching http://golang.org/
Fetching http://matt.aimonetti.net/ 
.....http://golang.org/ was fetched 
.......http://www.rubyconf.com/ was fetched 
.http://matt.aimonetti.net/ was fetched 
http://golang.org/ status: 200 OK 
http://www.rubyconf.com/ status: 200 OK 
http://matt.aimonetti.net/ status: 200 OK
```

As you can see from the print statements, the 3 urls are triggered in a
sequential way, but the responses come back in different orders due to different server latencies and response transfer time. 

## Conclusion

Go was designed to make concurrency easy for developers.
It is a very well documented language and you will find [on this page
a lot of information about its concurrency philosophy](http://golang.org/doc/effective_go.html#concurrency) and details about each available constructs works.

I like that the language is very simple and the constructs
explicit. If you want to write concurrent code, Go pushes you to do it
in a specific style. That style is clear and comfortable for me. My code stays simple, I don't go crazy with callbacks, and the
conventions make it simple for everyone else to understand my code.

Whether or not Go appeals to you stylistically, but clearly the designers
stayed close to the goal of developing to a 21st century C
with a special focus on concurrency with the unix approach.
