---
layout: post
title: "Safe testing in Rails"
date: 2013-06-27 12:41
comments: true
categories: 
---

There is a typical progression when developing an application. First we start with something simple, with a few models, things are simple to test. At this stage we can afford to build all the objects we need in our test without too much concern for speed and complexity.

```ruby
	describe User do
		let(:user) { User.new(name: ‘John’) }
		it ‘has a name’ do
			expect(user.name).to eq(‘John’)
		end
	end
```

But soon things get complicated, we add more and more models to our application that depend on each other. Tests reflect this as well, we have to create a lot of objects in our tests to test simple things.

```ruby
	describe User do
		let(:english) { Language.new(name: ‘English’) }
		let(:australia) { Location.new(name: ‘Australia’, language: ‘english’) }
		let(:melbourne) { Location.new(name: ‘Melbourne’, parent: australia) }
		let(:office) { Workspace.new(name: ‘Melbourne Office’, location: melbourne) }
		let(:user) { User.new(name: ‘John’, workspace: office) }
		
		it ‘has a language ’ do
			expect(user.default_language).to eq(english)
		end
		
	end
```

The example above creates all the objects that are needed to test `user.default_language`. We need to know a lot about our application just to run a test. Soon our test become a burden to write and very slow to run.
	
## Mocks, Stubs!
	
It is evident that we are just doing too much in our test, we should just be creating the object we care about and mock the rest. Let's rewrite this using doubles and stubs:

```ruby
	describe User do
		let(:english) { double.as_null_object}
		let(:office) { double.as_null_object } 
		let(:user) { User.new(name: ‘John’, workspace: office) }
		
		before do
			office(:language).and_return(:english)
		end
		
		it ‘has a language ’ do
			expect(user.default_language).to eq(english)
		end
		
	end 
```
	 
This is faster and simpler, we are just creating the object under test (user) and doubles for the other objects. We don’t need know about all the relationships in our application, just knowing the interface we care about in the objects directly related is enough. In this case just knowing that office responds to language is all we need.
 
But there is of course a problem. What happens in office doesn’t respond to language anymore? Say that someone changes `office.language` to `office.default_language`. Our test will still pass! __We have created an alterative universe in our test.__

## Integration test

Many experience developers will tell us that the solution to this conodrum is to have good integration test. That is partially true, integration tests are likely to fail when the interface between the objects is changed.

But guess what will happen? We will go and fix the integration test and then we will back to green. But wait, we forgot to fix the unit tests but everything passes! Unfortunatelly we cannot rely on due dilligence of people fixing all that should be fixed. 

Now we are even in a worst situation, our tests are all green but our app is broken. __So integration tests are not a reliable solution.__

# Safe mocking / stubbing

We still want the benefits of lean, fast tests but without the dangers of alternative realities. So what is the solution? 

I found that safe testing libraries are the best solution out there. This library will take care of checking that the methods you call actually exist in the mocked object. This frees us to still use doubles and stubs in our test without worring about the possibility of getting out of sync with the real application. 

From all the libraries I have tried, (Bogus)[https://github.com/psyho/bogus] is the most complete one. A test using Bogus will look like this:

```ruby
	describe User do
		fake(:english) { Language}
		fake(:office) { Workspace } 
		let(:user) { User.new(name: ‘John’, workspace: office) }
		
		before do
			stub(:office).language { english }
		end
		
		it ‘has a language ’ do
			expect(user.default_language).to eq(english)
		end
		
	 end 
```

Bogus will check that `office.language` is an actual method and that it takes the number of parameters we send. In this way we can have the benefits of stubbing and mocking without the drawbacks. I highly recommend that you try it in your project.
