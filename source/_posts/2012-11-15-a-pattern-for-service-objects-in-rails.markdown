---
layout: post
title: "A Pattern for Service Objects in Rails"
date: 2012-11-15 21:59
comments: true
categories: ruby rails
---

Service Objects in Rails allows to neatly separate the business logic of your application in reausable components. This post describes our implementation of this pattern.

We went through the usual story, we started by putting some business logic in controllers and some in models, then decided that all this logic should go in the models and ended up with very fat models tightly coupled to each other.

So when looking for alternatives patterns for organising the business logic I came across the idea of having separated objects to handle this business logic. Somewhere I saw this pattern labeled as 'Service objects' (SO). This was way before this very interesting post <a href="http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/">7 Patterns to Refactor Fat ActiveRecord Models</a>

The discussions often involved the Single Responsibility Principle (SLP), so most of the examples shown a class with only one public method on it. At first I totally dismissed this as a kind of functional pattern that didn't fit into the OOP world of Ruby.

But my pain with the fat models made me look into this again. So I decided to give it a try. Since then I have grown very fond of this approach because of the following:


- As this objects have little code they are easy to reason about
- They are very easy to compose (use one inside the other)
- They encapsulate the business logic neatly, so you never has to repeat the same logic in different places
- They use dependency injection (DI) heavily so they are loosely couple with the rest of the application
- Using DI makes it very easy to swap the dependencies in testing
- But still they have sensible dependency defaults, I don't see the point in injecting dependencies all the time when in 90% of the cases you just need the defaults


Let me show the pattern we are using:

```ruby

class FindInvoicesForClientService

  def call(client, args={})
      ...
      invoices = find_invoices_service.(some_args)
      ...
  end

  def find_invoices_service
      @find_invoices_service ||= FindInvoicesService.new
  end

  def find_invoices_service=(obj)
      @find_invoices_service = obj
  end
end

service = FindInvoicesForClientService.new
service.(client, args)

# in test we just swap the dependencies
collaborator = double.as_null_object
collaborator.stub(:call).and_return(invoices)
service.find_invoices_service = collaborator

```

The key points in our version are:


- the class has only one public method (call)
- dependencies are only passed if needed, the class has some sensible defaults that will be used 90% of the time
- each dependency injector has its own method instead of a attr_accessor, this is so you can prepare the dependencies if needed

This has been a great pattern for us, we have hundreds of these objects that can be easily composed as needed. This pattern has made our code seriously easier to work with.


