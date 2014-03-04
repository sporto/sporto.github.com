---
layout: post
title: "The Intuitive Proto Object in JavaScript"
date: 2011-11-06 09:25
comments: true
categories: javascript
---

Javascript is meant to be a prototypical language. But unfortunately it is a half done prototypical language. Internally it works like this but externally it tries to looks like something else. To understand this let's look at the __proto__ property.

Take the following code

```javascript
    Animal = {
        alive:true,
        legs:4
    }
    
    person = {
        legs:2
    }
    person.__proto__ = Animal;
    
    console.log(person.alive);
    //true
```

It is extremely easy to see what is happening here. We have just created a prototype inheritance chain between 'person' and 'Animal'. If you ask for a property or method in 'person' it will be looked up in that object first, if it is not found there it then will be looked up in the prototype.

__proto__ looks scary because of all the underscores around the name, but in reality it is quite easy and straightforward to understand and use. Unfortunately the __proto__ property is an internal properties only exposed by some browsers (e.g. Chrome, Firefox). So it cannot be used safely. 

So sadly we are left with this:

```javascript
	Animal = {
		alive:true,
		legs:4
	}
	  
	Person = function(){
		this.legs = 2;
	}
	Person.prototype = Animal;
	person = new Person();
	  
	console.log(person.alive); //true
	console.log(person.legs); //2
```

Note what is happening here:

- We need to create a constructor function
- Then we assign 'Animal' as the prototype of that constructor
- Then we run the function to get the instance we want

This is an extremely convoluted process to get to what we want, which is to have an object that inherits from another.

Object.create
------------

There is another alternative which is using Object.create:

```javascript
	Animal = {
	    alive:true,
	    legs:4
	}
	
	person = Object.create(Animal);
	person.legs = 2;
	
	console.log(person.alive); //true
	console.log(person.legs); //2
```

Object.create makes a new object with the prototype set to the given object. This is heaps better than the function prototype work around. But still using __proto__ is a lot more intuitive.

