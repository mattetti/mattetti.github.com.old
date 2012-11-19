---
layout: post
title: "What is Scala's pattern matching"
date: 2012-09-20 21:18
comments: true
categories: 
- scala
---

[Scala](http://www.scala-lang.org/) is a very interesting programming
language. It has for goal to provide
both [Object Oriented](http://en.wikipedia.org/wiki/Object-oriented_programming) and [Functional Programming](http://en.wikipedia.org/wiki/Functional_programming) paradigms. 
Now [Scala](http://www.scala-lang.org/) isn't the only recent programming language out there mixing the two paradigms.
[Ruby](http://www.ruby-lang.org/), [JavaScript](http://en.wikipedia.org/wiki/JavaScript) and [Clojure](http://clojure.org/) are other examples of popular
languages implementing both functional and OO programming patterns. Of
course, they each have a different take on the problem and that is what
is interesting.

Instead of arguing the pros and cons of OOP vs FP and how each of the
previously mentioned languages handle being OOP and FP, I'd like to introduce
a very powerful [Scala](http://www.scala-lang.org/) idiom: [pattern
matching](http://en.wikipedia.org/wiki/Pattern_matching). Note that
pattern matching isn't something Scala invented nor that it only exists in
Scala. Pattern matching can be achieved many different ways. However,
the majority of the popular languages don't put this concept at the
center of their language. A few languages way before Scala rested
heavily on pattern matching such as [Erlang](http://www.erlang.org/doc/reference_manual/patterns.html), 
[Haskell](http://www.haskell.org/haskellwiki/Haskell) but that's a different story.
How does Scala offers Pattern Matching, what is it and finally why is it
valuable?

## Scala pattern matching by examples

As its name indicates, pattern matching is used to detect patterns.
Here is an example that covers a few interesting cases:

```scala
def listAnalysis(list: List[Any]) = list match {
   case Nil => "empty"
   case 'a' :: tail => "starting by 'a'"
   case (head:Int) :: _ if head > 3 => "starting by an int greater than 3"
   case (head:Int) :: _ => "starting by an int"
   case _ => "whatever"
}
```

If you've never seen any Scala that probably looks like gibberish to
you. Let me break it down:

```
def listAnalysis(list: List[Any]) = list match {}
```

I define a new function called `listAnalysis` which takes an argument
named `list` which is of type `List` (this list could contain any kind
of elements).
The implementation of this function is a pattern match on the list
argument.
The body of this 'pattern match' looks like a classical switch statement.
But it's actually much more than a simple switch statement. Surely it
could be used like one, but as we will see, it can do much more.

Note that you can apply a pattern match against more than one object at
once as shown a bit later.

Let's look at the statements inside the function.

```scala
case Nil => "empty"
```

In this case we are checking that the list is empty or nil. If that's
the case, the statement on the other side of the "fat arrow" is
executed. In this case, we return a string but we could have called
another function or so whatever.

Now the second statement is much more complex and much more powerful:

```scala
case 'a' :: tail => "starting by 'a'"
```

Remember that we are doing pattern matching against our list object.
What we are doing here is use the `::` operator (aka cons operator) to 
extract the head and the rest of the list and then we match the head
against the `'a'` character.

This statement could have been written different ways:

```scala
case 'a' :: rest => "starting by 'a'"
```

In that case we named the `tail` of the list `rest`, but really we don't
care how it's called or its value, so the sensitive thing to do is to
rewrite that statement like that:

```scala
case 'a' :: _ => "starting by 'a'"
```

This basically says we are looking for a list that starts by `'a'` (and we
don't care about the rest).

Another statement:

```scala
case (head:Int) :: _ => "starting by an int"
```

In this case we type match the first element of the list and check that
we have an integer. Note that using the cons operator in the match cases
doesn't seem to affect performance. It would seem that at compilation,
the statement are rewritten to avoid creating uneeded objects (List also implements structural sharing of the tail list). 
I'm not a Scala expert so someone with more experience might be able to
confirm/clarify.

Now let's look at a variant of this statement:

```scala
case (head:Int) :: _ if head > 3 => "starting by an int greater than 3"
```

This is the same statement as above, but we are adding an extra
condition after the match. This is quite useful when simple matching
doesn't cut it.

Finally we have a fallback statement:

```scala
case _ => "whatever"
```

For more information about the cons operator, [read about the extractor objects](http://www.scala-lang.org/node/112) and what they can do.

Here is the result of calling our function with different lists:

```scala
listAnalysis(List())                             //> java.lang.String = empty
listAnalysis("This is a test".toList)            //> java.lang.String = whatever
listAnalysis("abcde".toList)                     //> java.lang.String = starting by 'a'
listAnalysis(List(1,2,3))                        //> java.lang.String = starting by an int
listAnalysis(List(42,24,36))                     //> java.lang.String = starting by an int greater than 3
listAnalysis("a".toList)                         //> java.lang.String = starting by 'a'
```

Here is another example using 2 items for the match:

```scala
def doubleMatch(foo: Any, bar: Any) = (foo, bar) match {
  case ('a', 'b') => "a and b"
  case (1, 'b') => "1 and b"
  case (1, _) => "1 and "+ bar
  case (a:Float, _) => "foo float"
  case _ => "unknown case"
}

doubleMatch(1, "test")                           //> java.lang.String = 1 and test
doubleMatch(1, 'b')                              //> java.lang.String = 1 and b
doubleMatch(42, Nil)                             //> java.lang.String = unknown case
doubleMatch('a', 'b')                            //> java.lang.String = a and b
doubleMatch(4.2f, 42)                            //> java.lang.String = foo float
```

# Why is pattern matching valuable?

In short, pattern matching allows the developer to deconstruct a structure to find specific
elements, in other words the pattern, needed to then constuct an
object/structure or trigger a function.

It's the opposite process of calling a method on an object. Here we
start from a structure (instead of the instance of an object), this structure is just a basic struct and 
based on a found pattern, we then trigger a function (with access to the data if we need it). 
When you have a stable and known data structure, it's often very interesting to
use the pattern matching approach because you can easily expand the
operations you can execute. However, if your operations are stable but the data changes, 
then the Object Oriented approach seems more adequate.

Besides that, pattern matching will often make your code clearer than
using if/else statements. Especially in a language like Scala where you
can define pattern matching function within a function and you can also
pass pattern matching functions around. Like eveything else, it needs to be used with caution so the intend of
the code is still understandable. That said it's a great tool to have handy and I've
had a lot of fun rewriting my newbie Scala code using a more idiomatic
approach based on pattern matching.

I hope you enjoyed this quick introduction. You can read more about pattern matching in Scala in the following articles:
 _(note: [Ikai](http://ikaisays.com)'s post on how he uses regexps with pattern matching is a fun read.)_

* [http://www.scala-lang.org/node/120](http://www.scala-lang.org/node/120)
* [http://pragprog.com/magazines/2012-03/scala-for-the-intrigued](http://pragprog.com/magazines/2012-03/scala-for-the-intrigued)
* [http://kerflyn.wordpress.com/2011/02/14/playing-with-scalas-pattern-matching/](http://kerflyn.wordpress.com/2011/02/14/playing-with-scalas-pattern-matching/)
* [http://ikaisays.com/2009/04/04/using-pattern-matching-with-regular-expressions-in-scala/](http://ikaisays.com/2009/04/04/using-pattern-matching-with-regular-expressions-in-scala/)
* [http://www.artima.com/scalazine/articles/pattern_matching.html](http://www.artima.com/scalazine/articles/pattern_matching.html)

