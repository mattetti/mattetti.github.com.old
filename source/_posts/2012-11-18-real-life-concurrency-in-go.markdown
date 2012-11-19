---
layout: post
title: "Real life concurrency in Go"
date: 2012-11-18 17:18
comments: true
categories: 
- Go
- Golang
---

Programming languages reflect the challenges and solutions language
designers decided to address. Each designer coming with his/her own background
decides to tackle some specific issues in a novel way and/or instead
decides to borrow existing paradigms from other languages.
This is why one should refrain to judge languages without understanding
what problem the language designer was trying to address.

Today we are going to look at [Google's Go language](http://golang.org/).
Go approaches concurrency from an interesting view point. But instead of digging
into the history and reasoning which led to this approach, I'd like to
show you the language constructs by actually writing real life code.

## Fetching web resources concurrently

The following example is taken from a recent presentation I gave called
[Ruby vs. the World](/posts/2012/11/02/rubyconf-2012-ruby-vs-the-world/) where I explored a few programming languages and
showed how they drove me to write Ruby code differently.

To show how [Go](http://golang.org/) addresses concurrency, I decided to build a simple
program which would concurrently fetch various web resources, wait for all of
them to be fetched and then process them all at once. In other
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

The code above does a few things, first, it names our package then
imports a few standard libraries that we are going to need and finally
defines an array/slice of strings representing the urls we are going to
fetch.

We then define a type we will use a bit later:

```go
type HttpResponse struct {
	url      string
	response *http.Response
	err      error
}
```

You can think of a struct type as a simple representation of a class. Technically, we are defining a structure with some typed attributes. We will later on, create instances of this defined type.

Go implements OOP [slightly differently](http://golang.org/doc/go_faq.html#Is_Go_an_object-oriented_language) than other languages.
Functions are defined on an interface, not a class or subclass.
This is call [structural typing](http://en.wikipedia.org/wiki/Structural_type_system),
follow the link to read more about what it means. 

So here is the code:


```go
func asyncHttpGets(urls []string) []*HttpResponse {
	ch := make(chan *HttpResponse, len(urls)) // buffered
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
		default:
			fmt.Printf(".")
			time.Sleep(5e7)
		}
	}
	return responses
}
```

This is the meat of our application. And there is quite a lot of going
on in just a few lines. Because I know some of you aren't familiar
with Go, I'll walk everyone though the code.
The function is called `asyncHttpGets`, it takes an argument named
`urls` which is an "array" of strings (I used quotes around the word
array because it's technically what Go calls a slice). The function is defined to return an
"array" of `HttpResponse` pointers.
That's it for the function signature, let's move on to the body.
We start by creating an instance of a `channel` and assigning it to the
`ch` variable name. Our channel is typed and buffered, but that's not
quite important right now. 

Think of a channel as a pipe, actually a little bit like a unix pipe. 
We can write to and read from that channel.
We then create an empty instance of a slice containing pointers to
`HttpResponse` objects.
Using the `for range` language construct, we iterate through our urls
and allocate one of the `urls` string to scoped variable named `url`
that we process within the block/lambda/closure marked by the curly braces.

Now this is where the async construct is being used. Using the `go`
keyword, we define an anonymous function taking a string representing a
url, we print this string and then use the standard library `net/http`
library to fetch the web resource. We use the returned data to create an
instance of our `HttpResponse` type and send it to the channel.
What might be slightly confusing (partly also because I named two
differently variables using the same name: `url`) is that we call this
anonymous function right away passing it the `url` variable that we set
in our loop.

You might wonder why we bother to create an anonymous function and
calling it right away instead of just calling the code directly.
What I didn't explain is that the `go` keyword executes the code that is
being passed in a "goroutine" which is well explained [here](http://golang.org/doc/effective_go.html#goroutines)

> A goroutine is a function executing concurrently with other goroutines in the same address space. It is lightweight, costing little more than the allocation of stack space. And the stacks start small, so they are cheap, and grow by allocating (and freeing) heap storage as required.

In other words, you start a goroutine and you let the "system" handle
how it wants to deal with the low level details. Technically, goroutines 
run on one or multiple threads but are designed so we don't need to worry about that.
So what's going on is that we trigger each http fetching in a separate goroutine
and then each response is being pushed down the channel.

We then start another `for` loop with a switch/case statement.
The case statement is pretty straight forward, we check if something is
in the channel. If there is something, we allocate the data to the `r`
variable, print the resource's url and append the resource to the slice
we created at the beginning of the function. If the length of the array
is the same as the length of all urls we want to fetch, we are done
fetching all our resources and can return.
The default case statement just prints a dot and sleep a little bit so
the loop isn't too tight.

Now that we wrote all the code we need, we can use it in our main
function:

```go
func main() {
	results := asyncHttpGets(urls)
	for _, result := range results {
		fmt.Printf("%s status: %s\n", result.url,
                 result.response.Status)
	}
}
```

If we then compile our code and run it, this is what we get:

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
sequential way, but the responses come in different orders. 

## Conclusion

What I like with this example is that it shows very clearly how the
language was designed to make concurrency easy for developers.
Go is a very well documented language and you will find [on this page
a lot of information about its concurrency philosophy](http://golang.org/doc/effective_go.html#concurrency), details about each available constructs works etc...
What I like is that the language is very simple and the constructs
explicit. If you want to write concurrent code, Go pushes you to do it
a certain way and I find that way clear and simple so it works great for
me. My code stay simple, I don't have to go all callback crazy, and the
conventions make it simple for everyone else to understand my code.

You might or might not like the language, but you have to admit that its
authors stay close to one of their goals: develop to a 21st century C
with a special focus on concurrency while keeping the unix approach.
