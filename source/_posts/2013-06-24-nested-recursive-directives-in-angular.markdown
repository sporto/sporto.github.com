---
layout: post
title: "Building nested recursive directives in Angular"
date: 2013-06-24 11:18
comments: true
categories: 
---

I learnt a new trick over the weekend using Angular, how to build a recursive tree of objects using directives. In this post I want to share how to do it.

Let's say that you have some data that looks like this, it can be as deep as you want:

```js

	[
		{	name: 'Europe',
			children: [
				{	name: 'Italy',
					children: [
						{	name: 'Rome'	},
						{	name: 'Milan'	}
					]}, 
				{	name: 'Spain'}
			]
		}, 
		{	name: 'South America',
			children: [
				{	name: 'Brasil'	},
				{	name: 'Peru'	}
			]
		}
	];
```

And using this data you want to build a tree, e.g.:

	Europe
		Italy
			Rome
			Milan
		Spain
	South America
		Brasil
		Peru

So to build something like this you will need some kind of recursive code to loop over all the elements and their children.

Let's start with the html:

```html
	<html ng-app='APP'>
		...
		<div ng-controller="IndexCtrl">
		
		</div>
		...
	</html>
```

First we have a controller 'IndexCtrl' which looks like this:

```js
	var app = angular.module('APP', []);


	app.controller('IndexCtrl', function ($scope) {
		$scope.locations = [ ..this is the array of locations shown above ..]; 
	});
```

Here we have `$scope.locations` pointing to the array of locations we want to render in our tree. 

Then we need a directive for rendering a collection, the html for the __collection__ looks like this:


```html
	<div ng-controller="IndexCtrl">
		<collection collection='locations'></collection>
	</div>
```

This directive takes a `collection` parameter which are the locations we want to render.


Our directive definition looks like this:

```js
	app.directive('collection', function () {
		return {
			restrict: "E",
			replace: true,
			scope: {
				collection: '='
			},
			template: "<ul><member ng-repeat='member in collection' member='member'></member></ul>"
		}
	})
```

- __restrict: 'E'__ tells angular that we want to apply this directive to any html tags matching `collection`.
- __replace: true__ tells angular that we want to replace the tag with the content of specified template in the directive.
- __scope:__ creates a new scope for the directive.
- __collection: '='__ tells angular to use the `collection` attribute in the directive and create a variable in the directive scope with the same name, `=` means that it should be passed as an object.
- __template: "…"__ is the new html that will be inserted instead of the original tag.

Notice the following code in the template:

```html
	<member ng-repeat='member in collection' member='member'></member>
```

This is another directive, used to render each member of the collection (we use the build-in ng-repeat for looping), in this directive we pass the current member as the `member` attribute.

The directive for __member__ looks like this:

```js
	app.directive('member', function ($compile) {
		return {
			restrict: "E",
			replace: true,
			scope: {
				member: '='
			},
			template: "<li>{{member.name}}</li>",
			link: function (scope, element, attrs) {
				if (angular.isArray(scope.member.children)) {
					element.append("<collection collection='member.children'></collection>"); 
					$compile(element.contents())(scope)
				}
			}
		}
	})
```

This looks a lot like the __collection__ directive, except for the link function. I will explain what is happening here, but before that, let me tell you about my first approach for trying to do this.

The first thing I tried is to simply add the __collection__ directive inside the template:

```js
	template: "<li>{{member.name}} <collection collection='member.children'></collection></li>"
```

But this doesn't work, it throws angular into an endless loop, because it tries to render the collection directive regardless if the member has children or not.

So instead you need to add the __collection__ directive manually only if there are children, thus the link function:

```js
	link: function (scope, element, attrs) {
		//check if this member has children
		if (angular.isArray(scope.member.children)) {
			// append the collection directive to this element
			element.append("<collection collection='member.children'></collection>"); 
			// we need to tell angular to render the directive
			$compile(element.contents())(scope);
		}
	}
```

Note the line `$compile(element.contents())(scope);`. As the html is appended manually we need to tell angular to re-render the directive.

That is it, [here is the complete example](http://jsbin.com/acibiv/3/edit). Thanks.

__Update 2013-12-31__: Please read the comments, there are some good suggestions on how to do this better.
