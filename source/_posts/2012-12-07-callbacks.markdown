---
layout: post
title: "Callbacks, Listeners, Control Flow, Promises, Oh My!"
date: 2012-12-07 16:59
comments: true
categories: 
---

When doing JavaScript development you will find that there are lots of tools at your disposal for dealing with Asynchronous events. This post explains four of these tools and what their advantages are.

Callbacks
---------

Let's start with callbacks, these are the most basic and well known form of async pattern you will find.

A callback looks like this:

```javascript
finder([1, 2], function(results) {
	..do something
});
````

Now let's say that we want to find some records and then process then and finally return the result. Both operations (find and process) are async.

### Setup

We need some functions to call in order to play with the callbacks. In the real world this functions will make an AJAX request somewhere and return the results but for now let's just use timeouts.

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

The code that uses this functions will look like this:

```javascript
finder([1, 2], function (records) {
    processor(records, function(records) {
    	console.log(records);       
    });
});
```

This can also be done like this, which might be clearer:

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

See the working example here http://jsfiddle.net/sporto/jjzbr/

### Pros

- Callbacks are easier to understand, we are quite familiar with them
- Easy to implement in your own libraries / functions

### Cons

- The infamous pyramid of doom as shown above, but this is quite easy to fix by splitting the functions also as shown above
- There can only be one callback

Listeners
---------

Listeners are also a well known pattern, mostly popularizer by jQuery and other DOM libraries. A Listener looks like this:

```javascript
finder.on('done', function (event, records) {
	..do something
});
```

### Setup

Let's do some setup for a listener demonstration. Unfortunately this setup is a lot more involving than callbacks. 

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

Note that they are calling a method trigger when the work is done, I will add this method to these objects using a mix-in.

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
Cool, now our objects can add listeners and trigger events.

### Using the listeners

The code to use the listeners is simple:

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

See the working example here http://jsfiddle.net/sporto/FYBjc/

### Pros

- This is well understood pattern.
- The big advantage is that you are not limited to one listener per object, you add as many listeners as you want. E.g.

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

- A lot more difficult to setup than callbacks, you will probably want to use a library e.g. jQuery, bean.

A Flow Control Library
---------------------

Flow control libraries are also a very nice way to tackle your async needs. One I particularly like is [Async](https://github.com/caolan/async).

Code using Async looks like this:

```javascript
async.series([
    function(){ ... },
    function(){ ... }
]);
```

#### Setup (Example 1)

Again we need a couple of functions that will do the work, as in the other examples these functions in the real world will probably make an AjAX request and return the results. For now let's just use a timeout.

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
Note that I am triggering callbacks inside this functions. These callbacks will be passed by the control flow library. Also note that the first argument passed to the callbacks is 'null': 

```javascript
cb(null, records);
```

This a common pattern in Node.js that allows passing an error as the first argument if an error occurs, by using this signatures in my functions and let flow very nicely with Async.

### Using Async

So the code that will use these functions looks like this:

```javascript
async.waterfall([
    function(cb){
        finder([1, 2], cb);
    },
    function(records, cb){
        processor(records, cb);
    },
    function(records, cb) {
        alert(records);
    }
]);
```

Async takes care of calling each function in order after the previous one has finished. As you can see this code is quite minimal and easy to understand.

You can see the working example here http://jsfiddle.net/sporto/GuxRF/

### Another setup (Example 2)
Now, it is very likely that you will have a library that follows the callback(null, results) signature unless you are using Node.js. So a more real example will look like this:

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
Note the nested functions inside the waterfall call. You can see this example here http://jsfiddle.net/sporto/x63BS/

### Pros

- Usually code using a control flow library is easier to understand because it follows a natural order (from top to bottom). This is not true with callbacks and listeners.
- If your functions or library follows the callback(error, result) pattern and use a flow control library like a Async then your code will flow very nicely from one function to the other, as shown in the first example.

### Cons

- If the signatures don't match as in the second example then you can argue that the flow control library offers little in terms of readability.

Promises
---------

Finally we get to our final destination: promises. Promises are a very powerful tool, but they are the harder to understand of these bunch.


Code using promises may look like this:

```javascript
finder([1,2])
    .then(function(records) {
    	.. do something
    });
```

This will vary widely depending on the promises library you use, in this case I am using [when.js](https://github.com/cujojs/when).

### Setup

My finder and processor functions will look like this:

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

Each function creates a deferred and returns a promise. Then it resolves the deferred when the results arrive.

### Consuming the promises

The code that consumes these functions looks like this:

```javascript
finder([1,2])
    .then(processor)
    .then(function(records) {
            alert(records);
    });
```

As you can see it is quite minimal and easy to understand. When used like this promises bring a lot of clarity to your code. Note how in the first then I simply pass the 'processor' function. This is because this function returns a promise itself so everything will just flow nicely.

You can see the working example here http://jsfiddle.net/sporto/Rhjbn/

### The big benefit of promises

Now if you think that this is all there is to promises you are missing something big. Promises have a neat trick that neither callbacks, listeners or control flows can do. They can be passed around, aggregated and most importantly they will trigger event if the event has already happened. Let me show you an example of this last point:

This is a huge thing for dealing with user interaction, where you don't have control of the order of events. See this other post http://sporto.github.com/blog/2012/09/22/embracing-async-with-deferreds/

Conclusion
---------

Hopefully I have help you to understand some of the tools that you have at your disposal when working with asynchronous JavaScript.

