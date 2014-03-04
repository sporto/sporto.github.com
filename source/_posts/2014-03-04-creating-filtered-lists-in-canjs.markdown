---
layout: post
title: "Creating filtered lists in CanJS"
date: 2014-03-04 21:56
comments: true
categories: canjs javascript 
---

Here is a simple tutorial on how to build a filtered list in CanJS. For example, lets say that we have a list of people and we want to display people that have birthdays this month.

Our mustache template would like something like this:

{% raw %}
```
<script type="text/mustache" id="view">
	<h2>Birthday this month</h2>

	<ul> 
		{{#birthdayThisMonth people}}
			<li>{{name}} - {{birthdate}}</li>
		{{/birthdayThisMonth}}
	</ul>
</script>
```
{% endraw %}

`people` is a CanJS model list , `birthdayThisMonth` is a Mustache helper. This Mustache helper looks like this:

```js
birthdayThisMonth: function (people, options) {
	var person, bd, bdMonth, thisMonth;

	if (people && people.length) {
		var res = [];
		for (var a = 0; a < people.length; a++) {
			person = people[a];
			bd = person.attr('birthdate');
			bdMonth = new Date(bd).getMonth();
			thisMonth = (new Date).getMonth();

			if (bdMonth === thisMonth) {
				// we want to display this birthday
				// so add this to the array
				// options.fn is a function that will return the final string using the mustache template
				res.push(options.fn(person));
			}
		}
		// we have an array, but we need to return a string
		return res.join(' ');
	}
}
```
 
Then we just need to pass the helper to the view (or declare it as global helper):

```js
var helpers = {
	birthdayThisMonth: function (...) { ... }
}
var view = can.view('view', {people: people}, helpers);
```

JsBin [here](http://jsbin.com/moriq/1/edit?html,js,output).