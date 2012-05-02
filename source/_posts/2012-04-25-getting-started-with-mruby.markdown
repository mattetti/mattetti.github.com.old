---
layout: post
title: "Getting started with mruby"
description: "Learn to embed Ruby in a C program using mruby."
date: 2012-04-25 10:59
comments: true
categories: 
- mruby
- Ruby
---

mruby is the latest Ruby implementation in an already quite long list:

* [MRI](http://en.wikipedia.org/wiki/Ruby_MRI)
* [REE](http://www.rubyenterpriseedition.com/)
* [JRuby](http://jruby.org/)
* [Rubinius](http://rubini.us/)
* [MacRuby](http://macruby.org)
* [Maglev](http://maglev.github.com/)
* [IronRuby](http://www.ironruby.net/)

And many other less known implementations.

This time, the main man behind the project is the Ruby creator
himself: [Yukihiro 'Matz' Matsumoto](http://en.wikipedia.org/wiki/Yukihiro_Matsumoto).
I already covered the [announcement, you can read more about it there](http://matt.aimonetti.net/posts/2012/04/20/mruby-and-mobiruby/).

## Why mruby?

Following my previous [article on mruby](http://matt.aimonetti.net/posts/2012/04/20/mruby-and-mobiruby/), some people seemed to be confused by the "raison d'être"/purpose of the project and the difference between [MRI](http://en.wikipedia.org/wiki/Ruby_MRI) and mruby.

mruby is designed to be modular and to be embedded, which means that
it's expected to live inside other software applications. In other words
mruby's goal is to make Ruby an embedded language. mruby is library that you link in other applications.

When talking about embedded languages, [Lua](http://www.lua.org/) is
probably the most well known interpreted language.
[Lua](http://www.lua.org/) is usually used for the purpose of offering a
higher level language (scripting language) inside a software written in a low level language
(usually C/C++). This is why [Lua](http://www.lua.org/) is very popular
in the game industry to allow a scriptable interface and have parts of
the code interpreted instead of requiring to recompile the code base
against the target platform. According to [this survey from 2009](http://www.satori.org/2009/03/the-engine-survey-general-results/),
Lua was by far the most popular scripting language for game developers.

![Lua vs other scripting languages](http://www.satori.org/images/GE1/9Scripting.gif)

Of course, Lua is also popular outside of the game industry and the
reasons are simple to understand:

* very easily embedded (simple and well documented API)
* small footprint
* portable (runs almost everywhere)

That gives you an idea of what mruby is trying to be. As a matter of
fact, this definition of Lua applies very well to mruby:

> "Lua is dynamically typed, runs by interpreting bytecode for a register-based virtual machine, and has automatic memory management with incremental garbage collection, making it ideal for configuration, scripting, and rapid prototyping."

## Why not Lua?

This is a matter of taste and requirements. It should eventually boil
down to the difference in the language designs.

<img src='/images/lua_logo.gif' alt='Lua logo, Matt Aimonetti site' style='margin:auto;display:block;width:100px' class='no-caption'/>

> Lua combines simple procedural syntax with powerful data description constructs based on associative arrays and extensible semantics. 

</br>
 <img src='/images/ruby_logo.png' alt='Ruby logo, Matt Aimonetti site' style='margin:auto;display:block;width:100px'class='no-caption' />

> Ruby is a dynamic, open source programming language with a focus on simplicity and productivity. It has an elegant syntax that is natural to read and easy to write.

<br style="clear:both">


So on one hand you have a very simple language with a focus on
functional programming and on the other, a [richer language](http://www.ruby-lang.org/en/about/) (albeit more
complex) with a
main focus on Object Oriented programming.

If you don't know Lua, it's quite close to JavaScript in the sense that
it's a prototype based language and you can therefore write code that is
Object Oriented.

Ruby being a full OO and richer language, you can organize your code
differently and write more of your complex logic in a scriptable language.
One could even potentially leverage the language to create its own
Domain Specific Language in pure Ruby and create a "Rails like" experience
within its application.


To be honest, I don't think that Ruby via mruby will just replace Lua,
but I do believe it will become a very interesting alternative for some
use cases. Especially if mruby manages to perform as well as Lua with the
same type of footprint. There is also the fact that mruby being
a sponsored project from the Japanese government, I wouldn't be
surprised to see big Japanese companies experiment with embedding Ruby
in their devices. After all, software is being added in every object we
own, rapid development and being first to market is more critical than
ever which makes mruby is a very attractive solution.

## Getting started

I wrote a simple hello world example to give you a place to start:

```c
#include <stdlib.h>
#include <stdio.h>

/* Include the mruby headers */
#include <mruby.h>
#include <mruby/proc.h>
#include <mruby/data.h>
// eventually to be replaced by #include <mruby/compile.h>
#include <compile.h>

int main(void)
{
  struct mrb_parser_state *p;
  mrb_state *mrb = mrb_open();
  char code[] = "p 'hello world!'";
  printf("Executing code with mruby!\n");

  p = mrb_parse_string(mrb, code);
  int n;
  n = mrb_generate_code(mrb, p->tree);
  mrb_run(mrb, mrb_proc_new(mrb, mrb->irep[n]), mrb_top_self(mrb));
  if (mrb->exc) {
   mrb_p(mrb, mrb_obj_value(mrb->exc));
  }
  return 0;
}
```
This [sample code](https://github.com/mattetti/mruby/tree/samples/samples/helloWorld) is available on my [GitHub repo](https://github.com/mattetti) and can be compiled by using `make` (see the [Makefile here](https://github.com/mattetti/mruby/blob/samples/samples/helloWorld/Makefile)).

To get started, you just need the [mruby source code](https://github.com/mruby/mruby) and a compiler. (I haven't tried to compile mruby or my sample on Windows for this code, but I assume it would work just fine with Visual C++).

The above example is very trivial and takes a line of Ruby code:
```ruby
p 'hello world!'
```
that the linked mruby lib interprets.
You can take the compiled file and use it on other machines using the
same platform and the code will run fine, just like normal C code.

For a more complete example, take a look at the [mruby's standalone
interpreter](https://github.com/mruby/mruby/blob/master/tools/mruby/mruby.c)

{% gist 2489553 %}


## Future

It's a bit too early to know if mruby will be succesful or not. There are few things I will keep my
eyes on.

### Performance

To be a true alternative to Lua, mruby will have to interpret Ruby code
much faster than what MRI is able to do right now. It will also need to
keep the memory footprint really tiny. Ruby being a more complicated
language than Lua, it might be tricky, but we'll see.

### Documentation

While the idea of using Ruby has a macro language might get a lot of
people excited, if documentation lacks or if the process is painful, the very same people might
fallback to another language.
Historically speaking, the MRI team has had issues with documentation
and communication due to various factors. I hope that thanks to Matz
experience and to the strong Ruby community, mruby will become a easy
and efficient alternative to Lua or people wanting to embed one of the
greatest interpreted languages.

### Ruby outside of Rails

While [Ruby on Rails](http://rubyonrails.org) really made Ruby popular,
I'm always a bit dissapointed when I see how many popular projects the
Ruby community has outside of Rails. Think about it, we have some
awesome implementation such as JRuby and MacRuby which allow you to use
an amazing amount of libraries to do the craziest thing one could
imagine using the language they like the most. But yet, Rails is still
by far the #1 Ruby project. Of course, there are many non-Rails related
projects out there, but they don't benefit from Rails' aura.

What I hope with mruby is that it will allow developers to leverage the
beauty of Ruby and to create other niches for people to have fun. 
Some people already started:

####MobiRuby
[Yuichiro MASUI](https://github.com/masuidrive) 
is working on having Ruby available on iOS and Android.

####Ruby for Node.js
[Yasuhiro Matsumoto](http://mattn.kaoriya.net/) is working on [mruby-uv](https://github.com/mattn/mruby-uv) an interface for [libuv](https://github.com/joyent/libuv) Node.js' platform layer.

####mod_mruby

[MATSUMOTO Ryosuke](http://blog.matsumoto-r.jp/) is working on an Apache
module for mruby called [mod_mruby](https://github.com/matsumoto-r/mod_mruby) which would 
be comparable to [mod_lua](http://httpd.apache.org/docs/2.3/mod/mod_lua.html)

####mruby REPL

[Frank Celler]() started working and blogging about writing a REPL for
mruby, go check out [his excellent blog posts](http://www.avocadodb.org/category/mruby) on the shell he's working on and other stuff he's doing with mruby.

But there is plenty more to do, [redis](http://antirez.com/post/scripting-branch-released.html) for instance now has/is about to be scriptabled via Lua, what about trying to use mruby to also support Ruby? 
What about scripting game logic and even full games in Ruby?
What about mruby on a [Raspberry pi](http://www.raspberrypi.org/)?
Ruby on my TV, fridge, car, AC, solar panel controller...

I will certainly be looking forward to people trying to reproduce the success
of Rails in a different domain thanks to the Ruby language.
