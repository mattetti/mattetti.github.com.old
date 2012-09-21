---
layout: post
title: "Ruby: the differences between dup &amp; clone"
date: 2012-07-28 12:20
legacy_url: http://coderwall.com/p/1zflyg
comments: true
categories: 
- ruby
---

Have you ever wondered what the differences are between **#dup** and **#clone** in Ruby?

They both create a shallow copy of an object (meaning that they don't copy the objects that might be referenced within the copied object). However, **#clone** does two things that **#dup** doesn't:

* copy the singleton class of the copied object
* maintain the frozen status of the copied object

Examples of the singleton methods not being copied.

dup:

```ruby
a = Object.new
def a.foo; :foo end
p a.foo
# => :foo
b = a.dup
p b.foo
# => undefined method `foo' for #<Object:0x007f8bc395ff00> (NoMethodError)
```

vs clone:

```ruby
a = Object.new
def a.foo; :foo end
p a.foo
# => :foo
b = a.clone
p b.foo
# => :foo
```

Frozen state:

```ruby
a = Object.new
a.freeze
p a.frozen?
# => true
b = a.dup
p b.frozen?
# => false
c = a.clone
p c.frozen?
# => true
```

Looking at the [Rubinius source code](https://github.com/rubinius/rubinius/blob/master/kernel/alpha.rb#L230) makes the difference extremely obvious.

Because of the extra steps, **clone** is a bit slower than **dup** but that's probably **not** what will make your app too slow.

Just a quick note about shallow copies (true for **clone** and **dupe**). Notice how the array referenced by the bar attribute doesn't get copied but shared between the original and the copied instances:

```ruby
class Foo
  attr_accessor :bar
  def initialize
    self.bar = [1,2,3]
  end
end

a = Foo.new
b = a.clone
p a.bar
# => [1, 2, 3]
p b.bar
# => [1, 2, 3]
a.bar.clear # clearing the array #bar points to
p a.bar
# => []
p b.bar
# => []
```

Both objects, **a** and **b** share the same reference to the array instance created when **a** is initiated. There are a few ways to do a deep copy of an object, but that's a different matter.

