---
layout: post
title: "Form validations with CanJS"
date: 2014-03-04 23:29:30 +1100
comments: true
categories: canjs javascript
---

During my time using CanJS I haven't found a canonical way of doing form validations with it, so here I want to share the way I am doing them at the moment.

First let's start with a model:

```js
var Person = can.Map({
	init: function () {
		this.validatePresenceOf('firstName');
		this.validatePresenceOf('lastName');
	}
}, {});
```

This model is using the [validation plug-in](http://canjs.com/docs/can.Map.validations.html) to mix-in the validation functions.

Then we need a control:

```js
var Control = can.Control({

	init: function (ele, options) {
		this.person = new Person();
		this.errors = new can.Map();

		var args = {person: this.person, errors: this.errors};
		var view = can.view('view', args);    
		this.element.html(view);
	},

	'form submit': function () {
		// get errors from person if any
		var errors = this.person.errors();

		// pass the errors to our errors observable
		this.errors.attr(errors, true);  

		if (errors) {
			console.log(errors);
		} else {
			// proceed to saving here
		}
		return false;
	}

});
```

Note how I create a errors can.Map and pass it to the view. This map will allow me to show any validation errors to the user. In form submit we retrieve the errors from the form (using the validation plug-in) and pass those errors to the errors can.Map.

Our view looks like this:

{% raw %}
```
<script type="text/mustache" id="view">

	<form>
		<label for="first_name">First Name:</label>
		<input type="text" can-value='person.firstName' name="first_name" />
		{{showErrors errors 'firstName'}}
		<br>

		<label for="last_name">Last Name:</label>
		<input type="text" can-value='person.lastName' name="last_name" />
		{{showErrors errors 'lastName'}}
		<br>    

		<input type="submit" value="Save" />
	</form>
</script>
```
{% endraw %}
 

Note the `showErrors` helper. Which looks like this:

```js
Mustache.registerHelper('showErrors', function (errors, prop) {
	var attr = errors.attr(prop);
	if (attr) {
		return prop + ' ' + attr[0];
	} else {
		return '';  
	}
});
```

This helper will display the error an error to the user if there is any. 

Here is a [fiddle](http://jsbin.com/jofeq/5/edit?html,js,output) with a complete example.