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

