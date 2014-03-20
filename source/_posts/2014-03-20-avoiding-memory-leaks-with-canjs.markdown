---
layout: post
title: "Avoiding memory leaks with CanJS"
date: 2014-03-20 22:58:14 +1100
comments: true
categories: 
---


Avoiding Memory leaks with CanJS


If you are building a single page application memory leaks can be a real problem, specially if you are application is meant to stay on the screen for long periods of time without the user refreshing it.

In this post I want to show how you can effectively avoid these leaks when developing with CanJS.

Example
------

For example the following control creates two potential memory leaks:

```js
var Control = can.Control({
	init: function (ele, options) {

		// # 1
		window.on('resize', function (event) {
			// ... do something on resize
		});

		// # 2
		setInterval(function () {
			// ... do some polling
		}, 2000);

	}
});
```

When the control is initialised, we are listening to the window resize event (#1) and creating a timeout (#2). Everything is fine until the control is removed from the DOM.

When the element is removed from the DOM we will end up with a ghost object still listening for the resize event and still running an interval, even if there is nothing in the DOM. 

If we don't unbind the events properly the browser doesn't have a way to know that our control object should be garbage collected. We have created a memory leak.

This gets even worst if we have an application that adds and removes a control several times, which is normal in an application with multiple views. In that case we just keep piling ghost objects potentially consuming a lot of memory and creating strange bugs as we have multiple unintended objects listening to the same events.

The CanJS way
------------

Getting rid of the first memory leak is quite easy in CanJS, instead of binding the event manually like shown above, you use the special CanJS syntax:

```js
var Control = can.Control({
	init: function (ele, options) {

	},

	'{window} resize': function () {
		//... do something on resize
	}
});
```

The `{window} resize` declaration tells CanJS to listen for the event we want but unbinding it automatically when the control is destroyed.

The destroy method
-----------------

The second leak has to be removed a bit more manually:

```js

var Control = can.Control({
	init: function (ele, 
		this.interval = setInterval(function () {
			// ... do some polling 
		}, 2000); 
	},

	destroy: function () {
		removeInterval(this.interval);
	}
});
```

We need to manually remove the interval in the `destroy` method of our control, the good thing is that we don't need to call this `destroy` method manually if we follow the correct pattern below.

Destroying a control
--------------------

CanJS will destroy a control automatically when its element is removed from the DOM. But you have to be sure that the element is removed, not just reused. 

For example, this wouldn't work:

```js
	// Create a control using an existing element in the DOM
	var controlA = new ControlA($('existing-element'), {});

	// Later reuse the element for a different control
	var controlB = new ControlB($('existing-element'), {});
	// This will not call destroy() on controlA
```

On the other hand, removing the element first will give us the behaviour we expect:

```js
	// Create a document fragment first 
	// this will be used as the element for the new control
	var view = $('<div>'); 

	// Create the control, and pass the newly created element fragment
	var controlA = new ControlA(view, {});

	// Append that newly created element to our container
	$('.my-app').append(view); 
```

```js
	// Later when replacing this control make sure to remove the element first
	$('.my-app').empty();   

	// Removing the control element will automatically call .destroy on your control

	// Repeat the process for the new control
	var view = $('<div>');
	var controlB = new ControlB(view, {});
	$('.my-app').append(view); 
```

By removing the elements entirely from the DOM, CanJS will trigger `destroy` and unbind the events. So that's it, avoiding memory leaks is quite straight forward when following the right patterns.