---
layout: post
title: "Embracing Async with Deferreds and Promises"
date: 2012-09-22
comments: true
external-url:
categories:
---

Deferred and promises are a very powerful tool for handling asynchronous events. In this blog post I will explain what they are and when to use them.

Let's create gretting cards
--------------

As an example let’s say that we are building a UI for a greeting cards making application. Our UI may look something like this:

{% img /images/deferreds/image01.png %}

The user can select an animation, select the music and then click next. Our event flow will look like this:

{% img /images/deferreds/image02.png %}
 
In this first case we know everything we need from the beginning. When the user clicks 'next' we know which animation and which music to load. After we are done loading these assets we will show the greeting card. To do this is code we have several options:

### We can count the assets already loaded:

```javascript

var assetsToLoad = 2;

loader.on('load', function(){
	assetsToLoad --;
		if(assetsToLoad===0) show();
});

loader.load(anim);
loader.load(music);

```

This code simply keeps a count of assets loaded and when all are loaded it shows the card.

### We can use a library like [Async](https://github.com/caolan/async):

```javascript

async.parallel([
	loadAnim,
	loadMusic,
], show);

```

We simply tell the library that we want to load these two assets in parallel and then call 'show' when done. The library takes care of all the details for us. A library like this is great but we need to know everything that we need to load at the start.

Not knowing everything from the beginning
--------------

Now let’s imagine that we don’t want a ‘Next’ button anymore in our UI:

{% img /images/deferreds/image04.png %}
 
Here we just want to show the greeting card automatically after the user has selected the animation and the music. Maybe not the best user experience but it works for our example. We don't know the order in which the user will select the assets.

If we want to stick with the previous way of doing thing (knowing everything at the start). Our event flow will looks something like this:

{% img /images/deferreds/image05.png %}
 
In the above flow we are waiting idle while the user is busy selecting the music. We don't want this, we want to take advantage of this time to load the assets the user has already chosen. So our event flow should look more like this:

{% img /images/deferreds/image06.png %}
 
In this flow we start loading the animation as soon as the user has selected it. While the user is busy selecting the music the animation is loading in the background. As soon as the user select the music we start loading it too in paralell.

A library like as Async is not useful in this case anymore. We can however still count like before or we could use conditional like:

```javascript

function onVideoLoaded(){
		checkIfAllLoaded();
}

function onMusicLoaded(){
		checkIfAllLoaded();
}

function checkIfAllLoaded(){
		if(videoLoaded && musicLoaded && … ) show();
}

```

This works but it is not very elegant and becomes hard to maintain quickly. 

Deferreds to the rescue
-----------------------

Here is where Deferreds shine. But let me explain what they are first. In a nutshell a Deferred is contract for an event that will happen in the future. Easier to explain this with some code:

```javascript

// we create a Deferred
var def = $.Deferred();

// we add a listener to the Deferred
// when the Deferred is done then do something
def.done(function(val){
		//… do something
});

//… later
// we mark the Deferred as done
// this will trigger the listener added above
def.resolve(val);

```

We create an Deferred object that accepts listeners like ‘done’. At some point in our application we set this deferreds as done (‘resolve’). This will trigger all the listeners. 

There are many Deferred implementations like jQuery (1.5+), [underscore deferreds](https://github.com/wookiehangover/underscore.deferred), [promised-IO](https://github.com/kriszyp/promised-io). My examples are using jQuery but the concepts are pretty much the same for all of them.

Aggregation
----------

A deferred can also be aggregated (I will explain promises later):

```javascript

// We create two deferreds
var def1 = $.Deferred();
var def2 = $.Deferred();

// We combine them together using the ‘when’ function. 
// This creates a new object (Promise) that is the aggregation of the two Deferreds. 
// We add a listener ‘done’ to the aggregated Promise.
$.when(def1, def2).done(function(one, two){
		//… do something with one and two;
});

//… later
def1.resolve(val);

//… even later
def2.resolve(val);

```

In this case when def1 and def2 are resolved the listener in the combined Promise will trigger. 

So going back to our greeting cards example. To do this:

{% img /images/deferreds/image06.png %}

We can simply code it like this:

```javascript

var animDef = $.Deferred();
var musicDef = $.Deferred();

$.when(animDef, musicDef).done(function(){
		show();	
});

//when the music is loaded
musicDef.resolve();

//when the animation is loaded
animDef.resolve();

```

No conditions, no counting. Quite elegant if you ask me.

What if it is already resolved?
-----------------

Deferreds have another neat trick. Let's say that the user selects the music first and it completely loads before we even start loading the animation.

{% img /images/deferreds/image07.png %}

By the time we add our aggregated listener the Deferred for the music has already been resolved:

```javascript

var animDef = $.Deferred();
var musicDef = $.Deferred();

//…later
musicDef.resolve();

//…even later
$.when(animDef, musicDef).done(function(){
		show();	
});

```

No problems! The aggregated listener will still triggers, it knows that the Deferred is already resolved and acts as expected. This is something you cannot with common event listeners!

Fail and reject
----

Deferred can also be rejected as well:

```javascript

var def = $.Deferred();

def
	.done(function(result){
		//do something
	})
	.fail(function(){
		//fallback
	});

//…later, something bad happened
def.reject();

```

This gives us a way of handling errors and providing fallbacks.

Promises
----------------------

A promise is mostly like a Deferred but it doesn’t provide the methods to resolve and reject it. This is useful when you want to give a reference to the Deferred to another object so it can add listeners but you don't want to give that object the power to resolve the Deferred.

Let's say you have a caller object with code like this:

```javascript

// create a loader object
var loader = new Loader();

// ask the loader to load something
// it receives a promise back
var promise = loader.load(…);

// add listeners to the promise
promise.done(function () {
		...do something
});

```

This caller receives a Promise from the loader object, it can add listeners to the Promise or aggregate it with other Promises. But it cannot resolve or reject them. Trying to do something like:

	promise.resolve();

will fail.

The code in the loader object will look something like this:

```javascript

function load() {
	def = $.Deferred();
	return def.promise(); // it returns the promise
}

//..later
function onLoad() {
		def.resolve();
}

```

Note the def.promise() method which creates the promise. The jQuery ajax methods does exactly this, it gives you a promise back when called.

You can combine promises to you heart content:

```javascript

var promise1 = $.when(animDef, musDef);
var promise2 = $.when(msgDef, bgDef);

$.when(promise1, promise2).done(function(){
		//… do something with anim, music message and background
});

```

Using Deferreds you can easily code something like this. Where you have many actions happening at the same time, each without a clear start and ending and depending on each other.

{% img /images/deferreds/image08.png %}
 
Conclusion
----------

In conclusion Deferreds are best suited in situations where:

-	You have several actions happening at the same time e.g. loaders
-	You don’t know where the action starts and when it finishes e.g. user interaction
-	You have other actions that depend on the completion of multiple other actions








