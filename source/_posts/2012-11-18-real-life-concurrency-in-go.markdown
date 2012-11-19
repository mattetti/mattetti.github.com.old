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
[Ruby vs. the World]()
