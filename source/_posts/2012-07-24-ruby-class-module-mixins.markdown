---
layout: post
title: "Ruby constructs: class, module and mixin"
date: 2012-07-30 14:46
comments: true
categories: 
- ruby
- language
---

When you first get started with the Ruby programming and you come from a different
language, the only tricky piece is often Ruby's approach to block/closure/anonymous functions.
Sure the metaprogramming seems a bit odd, but you don't have to use it.
That's why a lot of developers think that Ruby is a simple language.
Turns out that when you dig a bit further, you realize that Ruby is
actually quite a complex language. Ask any developer who worked on a
Ruby implementation, they'll all tell you the same thing: Ruby is full of
small little things that makes it complicated.

An example of something that might seem simple is inheritance. Ruby,
unlike C++, doesn't support multiple inheritance. What that means is
that a Ruby class can only have 1 parent class (superclass). However
multiple inheritance can be achieved via modules used as a mixins.
That's a very common pattern, people put some code in a module and then
mix it in/include it in a bunch of classes.
The problem I see though, is that people abuse these concepts and don't
respect the difference between a class, a module and a module used a
mixin.

## The Class

Object Oriented Programming 101: 
> "In object-oriented programming, a class is a construct that is used to create instances of itself – referred to as class instances, class objects, instance objects or simply objects. A class defines constituent members which enable its instances to have state and behavior."

So if you create a class and you don't create instances, you are using
the wrong construct. Here is an example of what I often see:

```ruby
class Settings

  @settings = {}
  def self.all
    @settings
  end

  def self.[](key)
    all[key]
  end

  def self.[]=(key, value)
    all[key] = value
  end

end
```

Which can be used as such:

```ruby
Settings[:secret] = 42 * Math::PI * Time.now.to_f
p Settings[:secret]
# => 177243152913.2707
```

_(granted this isn't a great example since we could have used a subclass
of `Hash` but just bear with me)_

Actually, the developer who wrote the code above would probably also use
some Ruby magic like `method_missing` to provide a more laxed API and allow for "nicer" getters
such as ```Settings.secret``` and ```Settings['secret']```. I have my
own thoughts on the topic but it's
an entirely different subject. 

Note also that the way class level methods are defined
can also be different depending on who wrote the code, you might see the
following variations (and other more esoteric ones):

```ruby
class Settings

  def Settings.all; end

  # or
  class << self
    def all; end
  end

end
```

The `Settings` code above works, the code is simple, yet I will argue one thing: **it's
an abuse of the class construct**. We're breaking the #1 rule of classes:
_"create instances of self"_.

**It's easy, whenever you don't create instances of a class,
please don't use a class.**

That's also true for slightly different examples such as:

```ruby
class API

  def fetch(id)
    HTTP.get('http://matt.aimonetti.net/article/:id', :id => id)
  end

end
```

```ruby
resource = API.new.fetch(42)
```

There is no need whatsoever to create an instance of `API`, using a class is
picking the wrong construct. Also, I don't care if you use the `Singleton`
module to only allow 1 instance of the class, you still shouldn't use a
class in the above example.

## The Module

In Ruby's object hierarchy, the Class object actually inherits from the
Module object.

```
ruby -v -e "p Class.ancestors"
ruby 1.9.3p194 (2012-04-20 revision 35410) [x86_64-darwin11.3.0]
[Class, Module, Object, Kernel, BasicObject]
```

As per Ruby's source code defintion: 

> "A Module is a collection of methods and constants."

I like to think of modules as namespaced methods and constants. Whenever
you want code that logically belongs together but that
won't require that you create instances of a 'model', then a module is
the right construct to use.

As a matter of fact, the two examples above are great cases where a
module should have been used.

The confusing bit is that modules can have module level methods but also
instance level methods. Here is an example:

```ruby
module API

  def self.fetch(id)
    HTTP.get('http://matt.aimonetti.net/article/:id', :id => id)
  end

end
```

This is a module level function, it could also be written like that:

```ruby
module API

  module_function

  def fetch(id)
    HTTP.get('http://matt.aimonetti.net/article/:id', :id => id)
  end

end
```

And be used like that:
```ruby
resource = API.fetch(42)
```

Until now, it makes sense. The weird thing is that even though, modules
unlike classes aren't meant to create instances, we have the possibility
to define module instance methods.

```ruby
module Settings

  DATA = {repo: 'http://github.com/mattetti'}

  def repository
    DATA[:repo]
  end

  def secret_key
    DATA[:key] ||= 42*Math::PI
  end

end
```

Great, but we can't actually use these methods since they are instance
methods and we don't create instances of modules. Well, that isn't quite
true, there is a way to access them and that's by using a module as a
mixin. What that means is that we inject/copy the module code inside a
class or an(other) object. Example in code:

```ruby
a = Object.new
a.extend(Settings)
a.repository
# => "http://github.com/mattetti"
```

Or we can add the code to a class so the instances of this class can
access our module instance methods:

```ruby
class Foo
  include Settings
end

Foo.new.repository
# => "http://github.com/mattetti" 
```

## The mixin modules

There a plenty of resources online about the many ways to use mixins in Ruby to achieve multiple inheritance and do cool stuff.
But the point of this article is to try to demonstrate that mixins
shouldn't be abused.

My problem with the above example is that by mixing in the `Settings`
module inside our `Foo` class, we created an uneeded, confusing extra
level of abstraction. Instances of `Foo` now have access to two
methods/objects: `repository` and `secret_key`. These methods or the
objects they refer to don't belong to the `Foo` class, but it seems
convenient to not have to type `Settings.repository` so we mixed things
in. Plus, a lot of Ruby developers seem to dislike adding class/module
level methods so they feel that this approach 'feels better'.

Here is the thing, the convenience of typing a few less characters isn't
worth it. Next time you or someone else will look at an instance of the
`Foo` class calling `repository`, finding where it is defined is going
to be a pain. That's especially true if you have many mixins in your
class. `Settings` will also probably grow and you will end up with a
bunch of methods that have nothing to do with your class instances.
In this case, I will call the use of a mixin, an abuse of construct.
Sure, Ruby allows you to do it, but that doesn't mean it's the right
thing to do. In Ruby, unlike in Python, there are 101 ways to do a
simple thing. It doesn't mean that the 101 ways are good, it just means
that Matz wasn't sure how people would use his programming language and
chose to give us more freedom to messup/doing it our own way.

### When to use mixins?

I have my own rule: use mixins whenever you need to *share behaviors* between
different classes.

In the above example, we weren't sharing behaviors, we were sharing
objects, there was no need to actually use a mixin.

That said, rules aren't rules without exceptions. A good example of this
exception would be the `Math` module from the standard library.
This module offers trigonometric and transcendental functions. You might
think that this module would be designed to be a mixin so you can get
`log`, `cos`, `exp` and friends available in your math related classes.
It turns out, all Math's methods are defined a module functions meaning
that they are meant to be called from the `Math` module directly.

However, Ruby allows you to mixin module functions, but these functions
become private. If you do include the module inside your class, your instance methods
will be able to call `hypot(x,y)` directly, but these methods won't be
available from the outside (`Foo.new.log(42)` would raise an
exception).

To conclude with mixins: mixins are great but don't abuse them or you
will endup with so much abstraction that your coworkers will secretely
call you [Kandinsky](http://en.wikipedia.org/wiki/Wassily_Kandinsky).
Stick to simple mixins allowing you to share behaviors between at least
a couple classes. See `DataMapper` for a great way to use mixins.


## Modules: your secret functional programming weapon

I have to say that I do like functional programming. The idea of having
functions not mutating the states of things around them pleases me. It
just seems clean, you feed data to a function and you get another piece
of data. No states were changed, maybe some temporary variables were
allocated to process the data, but the only thing that matters is the
input and the output. Easy to grasp, easy to follow, no magical states
being changed by some code fairies.

The good news is that Ruby allows us to write code like that. And this
is where modules are great. Very much like the `Math` module we
discussed above, there are many cases where you want to have a bunch of
functions that process an input and provide an output without keeping
any states. A good example of such a module would be a param
verification filter. The filter takes an input, takes some rules and
verifies that the input matches the rules.
Surely, we could create an instance for each verification, this would
allow to keep states in our class and do the usual OOP things. But we
could also simply use a module with a bunch of module (level) functions
that would pass to each other the input they need to not need to keep
states. The end result will be faster, nicer on the GC and easy to
follow.

Mixing OOP and functional programming isn't new, ask [Scala](http://www.scala-lang.org/) developers!
If done right, by adopting this approach we can simply our code base,
make it faster, easier to maintain and not losing the chance to also use
OOP paradigms when needed.

## Compromise

As shown earlier, modules and classes have pros and cons. Classes are
however much more natural to use for Object Oriented Programming. A
compromise about the `Settings` examples was suggested by Evan Phoenix.
The solution is elegant and simple. Use a class and an instance.
Here is an implementation based on his suggestion:

```ruby
class AppSettings < Hash

  def custom_method
  end

end

SETTINGS = AppSettings.new
```

The point here is not about the class implementation but that fact that we use
a class and associate an instance of this class to a constant so it can
be shared all over the place. Wow, a constant, this is so nasty you
might think. Classes are constants too and so are modules, here we just
allocate an instance of a class to a constant. Surpisingly simple and efficient.


