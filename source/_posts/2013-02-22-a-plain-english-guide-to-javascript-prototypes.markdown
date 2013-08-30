---
layout: post
title: "A plain english guide to JavaScript prototypes"
date: 2013-02-22 10:21
comments: true
categories: 
---

When I first started learning about JavaScript object model my reaction was of horror and disbelief. I was totally puzzled by its prototype nature as it was my first encounter with a prototype based language. I didn't help that JavaScript has a unique take on prototypes as it adds the concept of __function constructors__. I bet that many of you have had similar experience.

But as I used JavaScript more I didn't just learn to understand its object model but also started love parts of it. Thanks to JavaScript I have find out the elegance and flexibility of prototypes languages. I am now quite fond of prototype languages because they have a simpler and more flexible object model than class based languages.

Prototypes in Javascript
------------
Most guides / tutorials start explaining JavaScript objects by going directly to ‘__constructor functions__’, I think this is a mistake, as they introduce a fairly complex concept early on making Javascript look difficult and confusing from the start. Let's leave this for later. First let's start with the basics of prototypes.

Prototype chains (aka prototype inheritance)
-------------

Every object in Javascript has a __prototype__. When a messages reaches an object, JavaScript will attempt to find a property in that object first, if it cannot find it then the message will be sent to the object's prototype and so on. This works just like single parent inheritance in a class based language.

<img src="https://docs.google.com/drawings/d/1NdiIkHd9Cg2j6W4QcJ1X3DEhGXD2gacMXRuURcoE5T4/pub?w=960&amp;h=720">

Prototype inheritance chains can go as long as you want. But in general it is not a good idea to make long chains as your code can get difficult to understand and maintain.

The \_\_proto\_\_ object
------------------

To understand prototype chains in JavaScript there is nothing as simple as the __\_\_proto\_\___ property. Unfortunatelly __\_\_proto\_\___ is not part of the standard interface of JavaScript, not at least until ES6. So you shouldn't use it in production code. But anyway it makes explaining prototypes easy.

```js
	// let's create an alien object
	var alien = {
		kind: 'alien'
	}

	// and a person object
	var person = {
		kind: 'person'
	}

	// and an object called 'zack'
	var zack = {};

	// assign alien as the prototype of zack
	zack.__proto__ = alien
	
	// zack is now linked to alien
	// it 'inherits' the properties of alien
	console.log(zack.kind); //=> ‘alien’

	// assign person as the prototype of zack
	zack.__proto__ = person
	
	// and now zack is linked to person
	console.log(zack.kind); //=> ‘person’
```

As you can see the __\_\_proto\_\___ property is very straightforward to understand and use. Even if we shouldn't use __\_\_proto\_\___ in production code, I think that these examples give the best foundation to understand the JavaScript object model.

You can check that one object is the prototype of another by doing:

```js
	console.log(alien.isPrototypeOf(zack))
	//=> true
```

### Prototype lookups are dynamic

You can add properties to the prototype of an object at any time, the prototype chain lookup will find the new  property as expected.

```js
	var person = {}
	
	var zack = {}
	zack.__proto__ = person
	
	// zack doesn't respond to kind at this point
	console.log(zack.kind); //=> undefined
	
	// let's add kind to person
	person.kind = 'person'
	
	// now zack responds to kind
	// because it finds 'kind' in person
	console.log(zack.kind); //=> 'person'
```

### New / updated properties are assigned to the object, not to the prototype

What happens if you update a property that already exists in the prototype? Let's see:

```js
	var person = {
		kind: 'person'
	}
	
	var zack = {}
	zack.__proto__ = person
	
	zack.kind = 'zack'
	
	console.log(zack.kind); //=> 'zack'
	// zack now has a 'kind' property
	
	console.log(person.kind); //=> 'person'
	// person has not being modified
```

Note that the property 'kind' now exists in both person and zack.

Object.create
---------------

As explained before __\_\_proto\_\___ is not a well supported way of assigning prototypes to objects. So the next simplest way is using __Object.create()__. This is available in ES5, but old browsers / engines can be shimmed using this [es5-shim](https://github.com/kriskowal/es5-shim).

```js
	var person = {
		kind: 'person'
	}

	// creates a new object which prototype is person
	var zack = Object.create(person);
		
	console.log(zack.kind); // => ‘person’
```

You can pass an object to Object.create to add specific properties for the new object

```js
	var zack = Object.create(person, {age: {value:  13} });
	console.log(zack.age); // => ‘13’
```

Yes, the object you need to pass is a bit convoluted, but that is the way it is. See the docs [here](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Object/create).

### Object.getPrototype

You can get the prototype of an object using Object.getPrototypeOf

```js
	var zack = Object.create(person);
	Object.getPrototypeOf(zack); //=> person
```

There is no such thing as Object.setPrototype.

## Constructor Functions

__Constructor functions__ are the most used way in JavaScript to construct prototype chains. The popularity of __constructor functions__ comes from the fact that this was the only original way for constructing types. It is also an important consideration the fact that many engines are highly optimized for constructor functions.

Unfortunatelly they can get confusing, they are in my opinion one of the main reasons why new comers find JavaScript puzzling, but they are a big part of the language and we need to understand them well.

### Functions as constructors

In JavaScript you create an instance of a function like this:

```js
	function Foo(){}

	var foo = new Foo();

	//foo is now an instance of Foo
	console.log(foo instanceof Foo ) //=> true
```

In essence functions when used with the keyword __new__ behave like factories, meaning that they create new objects. The new object they create is linked to the function by its prototype, more on this later. So in JavaScript we call this an __instance__ of the function.
	
### 'this' is assigned implicitly

When we use '__new__', JavaScript injects an implicit reference to the new object being created in the form of the ‘__this__’ keyword. It also returns this reference implicitly at the end of the function. 

When we do this:

```js
	function Foo() {
		this.kind = ‘foo’
	}
	
	var foo = new Foo();	
	foo.kind //=> ‘foo’
```

Behind the scenes it is like doing something like this:

```js
	function Foo() {
		var this = {}; // this is not valid, just for illustration
		this.__proto__ = Foo.prototype;
		
		this.kind = ‘foo’
		
		return this;
	}
```

But keep in mind that the implicit '__this__' is only assigned to a new object when using '__new__'. If you forget '__new__' keyword then '__this__' will be the global object. Of course forgetting __new__ is a cause of multiple bugs, so don't forget __new__. 

One convention that I like is capitalizing the first letter of a function when it is intented to be used as a function constructor, so you now straightaway to you are missing the __new__ keyword.

### The 'function prototype'

Every function in JavaScript has a special property called ‘__prototype__’.

```js
	function Foo(){
	}

	Foo.prototype
```

As confusing as it may sound, this ‘__prototype__’ property is not the real prototype (__\_\_proto\_\___) of the function. 

```js
	Foo.__proto__ === Foo.prototype //=> false
```

This of course generates a lot of confusion as people use the term '__prototype__' to refer to different things. I think that a good clarification is to always refer to the special '__prototype__' property of functions as '__the function prototype__', never just '__prototype__'.

The ‘__prototype__’ property points to the object that will be asigned as the prototype of instances created with that function when using '__new__'. Confusing? This is easier to explain with an example:

```js
	function Person(name) {
		this.name = name;
	}

	// the function person has a prototype property
	// we can add properties to this function prototype
	Person.prototype.kind = ‘person’

	// when we create a new object using new
	var zack = new Person(‘Zack’);
	
	// the prototype of the new object points to person.prototype
	zack.__proto__ == Person.prototype //=> true

	// in the new object we have access to properties defined in Person.prototype
	zack.kind //=> person
```

That is mostly everything there is to know about the JavaScript object model. Understanding how __\_\_proto\_\___ and __function.prototype__ are related will give you countless hours of joy and satisfaction, or maybe not.

Mistakes, confusing? Let me know.




