---
layout: post
title: "Asynchronous JS: Callbacks, Listeners, Control Flow Libs and Promises"
date: 2012-12-09 16:59
comments: true
categories: 
---

When it comes to dealing with **asynchronous** development in JavaScript there are many tool you can use. This post explains four of these tools and what their advantages are. These are Callbacks, Listeners, Control Flow Libraries and Promises.

### Example Scenario

To illustrate the use of these four tools, let's create a simple example scenario.

Let's say that we want to find some records, then process them and finally return the processed results. Both operations (find and process) are asynchronous.

<div style="height: 180px; overflow:hidden;">
	<img src="http://photos.foter.com/29/why-didnt-you-call-me_l.jpg" style="display: block; width: 100%; margin-top:-120px;">
</div>
Photo credit: bitzcelt / Foter / CC BY-NC-ND

Callbacks
---------

Let's start with callback pattern, this is the most basic and the best know pattern to deal with async programming.

A callback looks like this:

```javascript
finder([1, 2], function(results) {
	..do something
});
````

In the callback pattern we call a function that will do the asynchronous operation. As one of the parameters we pass a function that will be called when the operation is done.

### Setup

In order to illustrate how they work we need a couple of functions that will find and process the records. In the real world this functions will make an AJAX request and return the results, but for now let's just use timeouts.

```javascript
function finder(records, cb) {
    setTimeout(function () {
        records.push(3, 4);
        cb(records);
    }, 1000);
}
function processor(records, cb) {
    setTimeout(function () {
        records.push(5, 6);
        cb(records);
    }, 1000);
}
```
### Using the callbacks

The code that consumes these functions looks like this:

```javascript
finder([1, 2], function (records) {
    processor(records, function(records) {
    	console.log(records);       
    });
});
```

We call the first function, passing a callback. Inside this callback we call the second function passing another callback.

These nested callbacks can be written more clear but passing references to a function.

```javascript
function onProcessorDone(records){
	console.log(records);   
}

function onFinderDone(records) {
    processor(records, onProcessorDone);
}

finder([1, 2], onFinderDone);
```

In both case the console log above with log [1,2,3,4,5,6]

Working example here:
{% jsfiddle jjzbr  js,result %}

### Pros

- They are very well know pattern, so they are easy to understand.
- Very easy to implement in your own libraries / functions.

### Cons

- Nested callbacks will form the infamous pyramid of doom as shown above, which can get hard to read when you have many levels deep. But this is quite easy to fix by splitting the functions also as shown above.
- You can only pass one callback for a given event, this can be a big limitation sometimes.

<div style="height: 150px; overflow:hidden;">
	<img src="http://photos.foter.com/66/summer-sound-large-view_l.jpg" style="display: block; width: 100%;">	
</div>
Photo credit: Brandon Christopher Warren / Foter / CC BY-NC

Listeners
---------

Listeners are also a well known pattern, mostly made popular by jQuery and other DOM libraries. A Listener might look like this:

```javascript
finder.on('done', function (event, records) {
	..do something
});
```

We call a function on an object that adds a listener. In that function we usually pass the name of the event we want to listen to and a callback function. 'on' is one of many common way of adding listeners, other common function names you will come across are 'bind', 'listen', 'addEventListener', 'observe'.

### Setup

Let's do some setup for a listener demonstration. Unfortunately the setup needed is a bit more involving than the callbacks example. 

First we need a couple of objects that will do the work of finding and processing the records.

```javascript
var finder = {
    run: function (records) {
        var self = this;
        setTimeout(function () {
            records.push(3, 4);
           self.trigger('done', [records]);            
        }, 1000);
    }
}
var processor = {
    run: function (records) {
        var self = this;
        setTimeout(function () {
            records.push(5, 6);
            self.trigger('done', [records]);            
        }, 1000);
    }
 }
```

Note that they are calling a method trigger when the work is done, I will add this method to these objects using a mix-in. Again 'trigger' is one of the names you will come across, others are 'fire', 'publish'.

We need a mix-in object that has the listener behaviour, in this case I will just lean on jQuery for this:

```javascript
var eventable = {
    on: function(event, cb) {
        $(this).on(event, cb);
    },
    trigger: function (event, args) {
        $(this).trigger(event, args);
    }
}
```

Then apply the behaviour to our finder and processor objects:

```javascript
 $.extend(finder, eventable);
 $.extend(processor, eventable);
```

Excellent, now our objects can add listeners and trigger events.

### Using the listeners

The code that consumes the listeners is simple:

```javascript
finder.on('done', function (event, records) {
	processor.run(records);  
});
processor.on('done', function (event, records) {
    console.log(records);
});
finder.run([1,2]);
```

Again the console run will output [1,2,3,4,5,6]

Working example here:
{% jsfiddle FYBjc  js,result %}

### Pros

- This is another well understood pattern.
- The big advantage is that you are not limited to one listener per object, you can add as many listeners as you want. E.g.

```javascript
finder
	.on('done', function (event, records) {
    	.. do something
	})
	.on('done', function (event, records) {
    	.. do something else
	});
```

### Cons

- A bit more difficult to setup than callbacks in your own code, you will probably want to use a library e.g. jQuery, bean.

<div style="height: 150px; overflow:hidden;">
	<img src="http://foter.com/image/display/1036313/495x371/" style="display: block; width: 100%; margin-top: -50px;">
</div>
Photo credit: Nod Young / Foter / CC BY-NC-SA

A Flow Control Library
---------------------

Flow control libraries are also a very nice way to deal with asynchronous code. One I particularly like is [Async.js](https://github.com/caolan/async).

Code using Async looks like this:

```javascript
async.series([
    function(){ ... },
    function(){ ... }
]);
```

#### Setup (Example 1)

Again we need a couple of functions that will do the work, as in the other examples these functions in the real world will probably make an AjAX request and return the results. For now let's just use timeouts.

```javascript
function finder(records, cb) {
    setTimeout(function () {
        records.push(3, 4);
        cb(null, records);
    }, 1000);
}
function processor(records, cb) {
    setTimeout(function () {
        records.push(5, 6);
        cb(null, records);
    }, 1000);
}
```

#### The Node Continuation Passing Style
Note the style used in the callbacks inside these functions. 

```javascript
	cb(null, records);
```

The first argument in the callback is null if no error occurs; or the error if one occurs. This is a common pattern in Node.js libraries and Async.js uses this pattern. By using this style the flow between Async.js and the callbacks becomes super simple.

### Using Async

The code that will consume these functions looks like this:

```javascript
async.waterfall([
    function(cb){
        finder([1, 2], cb);
    },
    processor,
    function(records, cb) {
        alert(records);
    }
]);
```

Async.js takes care of calling each function in order after the previous one has finished. Note how we can just past the 'processor' function, this is because we are using the Node continuation style. As you can see this code is quite minimal and easy to understand.

Working example here:
{% jsfiddle GuxRF  js,result %}

### Another setup (Example 2)
Now, when doing front-end development it is unlikely that you will have a library that follows the callback(null, results) signature. So a more realistic example will look like this:

```javascript
function finder(records, cb) {
    setTimeout(function () {
        records.push(3, 4);
        cb(records);
    }, 500);
}
function processor(records, cb) {
    setTimeout(function () {
        records.push(5, 6);
        cb(records);
    }, 500);
}

// using the finder and the processor
async.waterfall([
    function(cb){
        finder([1, 2], function(records) {
            cb(null, records)
        });
    },
    function(records, cb){
        processor(records, function(records) {
            cb(null, records);
        });
    },
    function(records, cb) {
        alert(records);
    }
]);
```
​
It becomes a lot more convoluted but style you can see the flow going from top to bottom.

Working example here:
{% jsfiddle x63BS  js,result %}

### Pros

- Usually code using a control flow library is easier to understand because it follows a natural order (from top to bottom). This is not true with callbacks and listeners.

### Cons

- If the signatures don't match as in the second example then you can argue that the flow control library offers little in terms of readability.

<div style="height: 150px; overflow:hidden;">
	<img src="http://photos.foter.com/90/time-heals-nothing_4.jpeg" style="display: block; width: 100%; margin-top:-70px;">
</div>
Photo credit: Helmut Kaczmarek / Foter / CC BY-NC-SA

Promises
---------

Finally we get to our final destination. Promises are a very powerful tool, but they are the least understood.


Code using promises may look like this:

```javascript
finder([1,2])
    .then(function(records) {
    	.. do something
    });
```

This will vary widely depending on the promises library you use, in this case I am using [when.js](https://github.com/cujojs/when).

### Setup

Out finder and processor functions look like this:

```javascript
function finder(records){
    var deferred = when.defer(); 
    setTimeout(function () {
        records.push(3, 4);
        deferred.resolve(records);
    }, 500);
    return deferred.promise;
}
function processor(records) {
     var deferred = when.defer();
    setTimeout(function () {
        records.push(5, 6);
        deferred.resolve(records);
    }, 500);
    return deferred.promise;
}
```

Each function creates a deferred object and returns a promise. Then it resolves the deferred when the results arrive.

### Using the promises

The code that consumes these functions looks like this:

```javascript
finder([1,2])
    .then(processor)
    .then(function(records) {
            alert(records);
    });
```

As you can see it is quite minimal and easy to understand. When used like this promises bring a lot of clarity to your code as they follow a natural flow. Note how in the first callback we can simply pass the 'processor' function. This is because this function returns a promise itself so everything will just flow nicely.

Working example here:
{% jsfiddle Rhjbn  js,result %}

There is a lot to promises, they can be passed around and aggregated into bigger promises, resolved or rejected.

### The big benefit of promises

Now if you think that this is all there is to promises you are missing the what I consider the biggest advantage. Promises have a neat trick that neither callbacks, listeners or control flows can do. You can add listener to promises even when they have already resolved and those listener will trigger immediately, meaning that you don't have to worry if the event has already happened. Let me show you an example of this last point:

{% jsfiddle 8zjGq  js,result %}

This is a huge feature for dealing with user interaction in the browser. In complex applications you may not now the order of actions that the user will take, so promises are an excellent tool for dealing with this. See this other [post](http://sporto.github.com/blog/2012/09/22/embracing-async-with-deferreds/).

### Pros

- Really powerful, you can aggregate promises, pass the around, or add listeners when already resolved.

### Cons
- Least understood of all of these tools.
- They can get difficult to track when you have lots of aggregated promises with added listeners along the way.

Conclusion
---------

These are in my opinion the four main tools for dealing with asynchronous code. Hopefully I have help you to understand these tools better.

