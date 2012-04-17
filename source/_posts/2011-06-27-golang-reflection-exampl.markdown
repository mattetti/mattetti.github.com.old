---
date: '2011-06-27 12:12:32'
layout: post
slug: golang-reflection-exampl
status: publish
title: Go's reflection example
wordpress_id: '1084'
categories:
- Misc
- blog-post
- Tutorial
---

The [Go Programming language](http://golang.org/) is really cool language by Google. According to the sales pitch, it's a **_"fast, statically typed, compiled language that feels like a dynamically typed, interpreted language"_**. Well, if you are like me, you don't trust sales pitches because you know that people writing them dont' care about you, they care about their product. However cynical you are, you still have to check the facts. So here is a quick demonstration showing how to use Go's reflection feature.

Installing Go is actually really straight forward on a Mac, and slightly harder on Linux, check [this guide ](http://golang.org/doc/install.html)to see how to build Go in a few minutes.

Once all setup, you might want to read the documentation to see how to code in Go. Go is actually a kind of nice version of C with a[ simplified syntax](http://golang.org/doc/go_spec.html), no header files, really fast compilation time, a garbage collector and a [simple way to approach object inheritance](http://golang.org/doc/effective_go.html?#interfaces_and_types) without turning in the complicated mess C++ is. The language is designed around the concept of [goroutines, a very nice way to handle concurrency](http://golang.org/doc/effective_go.html?h=goroutines#concurrency). It also has some features that Rubyists, Pythonistas and Javascripters wouldn't want to live without such as closures and some they probably wish they had such as [defer](http://golang.org/doc/effective_go.html?#defer). But of the things we are used to with dynamic languages is the concept of reflection. In a nutshell, at runtime, your code can reflect on the type of a given object and let the developer act accordingly. Depending on your programming background that might be obvious or you might not see the value. To be honest, that's not the question here. What I'm interested in showing you is how it works.

For the sake of this demo, let's pretend we want to have a "Dish" data model, each instance of the "Dish" type will have a few attributes, an id, a name, an origin and a custom query which really is a function that we store as an attribute. Here is how we would represent that model in Go:

    
    // Data Model
    type Dish struct {
      Id  int
      Name string
      Origin string
      Query func()
    }


This is more or less the equivalent of the following Ruby code:

    
    class Dish
      attr_accessor :id, :name, :origin, :query
    end


Ruby works slightly differently in the sense that defining attribute accessors create getters and setter methods but doesn't technically create instance variables until they are used. Here is what I mean:

    
     
    shabushabu = Dish.new
    shabushabu.instance_variables # => []
    shabushabu.name = "Shabu-Shabu"
    shabushabu.instance_variables # => ["@name"]
    shabushabu.origin = "Japan"
    shabushabu.instance_variables # => ["@name", "@origin"]


Another way of checking on the accessors is to check the methods defined on the object:

    
     
    shabushabu.methods - Object.new.methods
    => ["name", "name=", "origin", "origin=", "id=", "query", "query="]


But anyway, this post isn't about Ruby, it's about Go and what we would like is to reflect on an object of "Dish" type and see its attributes. The good news is that the Go language ships with a [package to do just that](http://golang.org/pkg/reflect/). Here is the full implementation:

    
     
    package main
    
    import(
      "fmt"
      "reflect"
    )
    
    func main(){
      // iterate through the attributes of a Data Model instance
      for name, mtype := range attributes(&Dish;{}) {
        fmt.Printf("Name: %s, Type %s\n", name, mtype.Name())
      }
    }
    
    // Data Model
    type Dish struct {
      Id  int
      Name string
      Origin string
      Query func()
    }
    
    // Example of how to use Go's reflection
    // Print the attributes of a Data Model
    func attributes(m interface{}) (map[string]reflect.Type) {
      typ := reflect.TypeOf(m)
      // if a pointer to a struct is passed, get the type of the dereferenced object
      if typ.Kind() == reflect.Ptr{
        typ = typ.Elem()
      }
    
      // create an attribute data structure as a map of types keyed by a string.
      attrs := make(map[string]reflect.Type)
      // Only structs are supported so return an empty result if the passed object
      // isn't a struct
      if typ.Kind() != reflect.Struct {
        fmt.Printf("%v type can't have attributes inspected\n", typ.Kind())
        return attrs
      }
    
      // loop through the struct's fields and set the map
      for i := 0; i < typ.NumField(); i++ {
        p := typ.Field(i)
          if !p.Anonymous {
            attrs[p.Name] = p.Type
          }
         }
    
      return attrs
    }


Unfortunately, my code highlighter doesn't support the Go syntax, but GitHub does, so here is a [pretty version](https://gist.github.com/1009629).

There are ways of running Go source code like Ruby or Python scripts but in this case, we'll use the compiler & linker provided with Go. I named my source file "example.go", and here is how I compiled, linked and run it:


    
     
    $ 6g example.go && 6l example.6 && ./6.out
    Name: Origin, Type string
    Name: Id, Type int
    Name: Query, Type 
    Name: Name, Type string
    



As you can see each attribute is printed out with its name and type. The code might seem a bit odd if you never looked at Go before. 
Here is a quick rundown of the code:

In our main function, we create a new instance of type Dish on which we call attributes on. The call returns a map on which we iterate through and print the attribute name (key) and type (value).
The attributes function is defined a bit below and and it takes any type of objects (empty interface) and returns a map, which is like a Hash or a Dictionary. The map has keys of String type and values of "Type" type. The "Type" type is defined in the reflect package. Inside the function, 23 then use the previously mentioned reflect package to check on the type and the name of each attribute and assign it to a map object. (note that I'm explicitly returning the map, but I could have done it in a more implicit way)

So there you go, that's how you use reflection in Go. Pretty nifty and simple.


