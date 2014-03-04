---
layout: post
title: "Simple Dependency Injection in Ruby"
date: 2013-09-25 21:12
comments: true
categories: ruby
---

Some time ago I read the book [Practical Object-Oriented Design in Ruby](http://www.amazon.com/Practical-Object-Oriented-Design-Ruby-Addison-Wesley/dp/0321721330) by Sandi Metz, while an excellent book there is one thing that bothered me while reading it. 

That is the pattern for dependecy injection proposed on the book. It goes something like this:

```ruby
  class Car
    def initialise(engine)
      @engine = engine
    end

    def start
      engine.start
    end
  end

  car = Car.new(engine)
```

In this example the class Car takes an `engine` dependecy, each time you want to create a car you have to pass an engine to it. This can easily become a cumbersome way of instantiating objects because you need to know a lot about your object just to create them. In this case I need to know that Car takes an engine and specifically which engine depending on the type of car I want to create.

To deal with this the suggested solution is to use an __injector__. See the definition in [wikipedia](http://en.wikipedia.org/wiki/Dependency_injection). The injector is in charge of creating the instances you need and injecting the right dependecies.

Although this is a powerful pattern I believe it mostly an unnecessary complication, specially in dynamic languages like ruby. Let me explain why.

What do we want to achieve with DI?
---------------------------

The main reason for using DI is to be able to swap the dependencies of an object at run time, this is mostly useful for testing. To achieve this there is a dead simple way to do it without having to deal with the complexity of injectors.

The simple way to do it
-----------------------

Instead of passing the dependecy when instantiating an object, we can simply let the object have default dependecies.

```ruby
  class Car

    attr_writer :engine

    def start
      engine.start
    end

    def engine
      @engine ||= Engine.new
    end
  end
  
  car = Car.new
```

If we just create a car we get a default engine for it, so we don't need to bother with injectors.

What do we gain:

- External code doesn't need to know about the internals of our objects
- We have sane default dependecies
- We can easily swap the dependecies at run time

In your test you can simply do this:

```ruby
  car = Car.new
  car.engine = FakeEngine

  car.start
```

So that's it, __DI doesn't have to be complicated at all in ruby__.
