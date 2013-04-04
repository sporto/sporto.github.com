---
layout: post
title: "A comparison of Angular, Backbone, CanJS and Ember"
date: 2013-04-04 19:00
comments: true
categories: javascript, mvc
---

Selecting a JavaScript MVC framework can be hard work. There are so many factors to consider and so many opinions out there. In this post I compare four leading libraries I have personal experience with: [Angular](http://angularjs.org/), [Backbone](http://backbonejs.org/), [CanJS](http://canjs.us/) and [Ember](http://emberjs.com/). I have tried to make an objective comparison as much as I can, but of course this is really hard as it is based mostly on my experience.

For each category I have used a scale from 1 to 5. Where 1 is poor and 5 is great.

## Features

How many features a MVC framework has? Can it do view bindings, two way bindings, filters, computed properties, dirty attributes, form validation? This is a long list. I am not going to compare every single feature here just the ones I think are the important ones.

|Feature                 | Angular | Backbone | CanJS | Ember |
|----                    | ------- | -------- | ----- | ----- |
| Observables            | y       | y 	     | y     | y |
| Routing                | y       | y 	     | y     | y |
| View bindings          | y       | - 	     | y     | y |
| Two way bindings       | y       | - 	     | -     | y |
| Partial views          | y       | - 	     | y     | y |
| Filtered list views    | y       | - 	     | -     | y |

__Observables__: objects that can be observed for changes. 

__Routing__: Pushing changes to the browser url hash and listening for changes to act accordingly.

__View bindings__: Using observable objects in views, having the views automatically refresh when the observable object change.

__Two way bindings__: Having the view push changes to the observable object automatically, for example a form input.

So based on these features my scores are:

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 5       | 2 	     | 4     | 5 |


## Flexibility

There are thousand of awesome plug-ins and libraries out there that do specialised things. They usually do these things better that what comes bundle with a framework. So it important to be able to bend the rules and integrate these libraries with the chosen MVC framework. 

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 3       | 5 	     | 4     | 3 |

Backbone is the most flexible framework as it is the one with the less conventions and less features bundle in. CanJS is almost as flexible as Backbone as it allows you to easily integrate other libraries with minimum effort. Ember and Angular are still flexible to some degree but you will find that you could end up fighting the framework if you don't like the way they it does things.

## Learning curve and documentation

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 2       | 4 	     | 5     | 3 |


### Angular

Angular has a very high wow factor at the beginning. And it looks quite easy at first sight. But after you have learnt the very basic it is quite a steep learning curve from there. It is a complex library with lots of peculiarities. Reading the documentation is not easy as they use a lot of Angular only jargon.

### Backbone

The basic of Backbone are quite easy to learn. But soon you find that there are not enough opinions there to know how to best structure your code. It takes a while to watch or read a few tutorials to learn some best Backbone practices. Also you will find that you will probably need to learn another library on top of Backbone (e.g. [Marionette](http://marionettejs.com/) or [Thorax](http://thoraxjs.org/)) to get things done. So I don't think that Backbone is the easies framework to learn.

### CanJS

CanJS is in comparison the easiest to learn of the bunch. Just by reading the one page website you will know most of what you need to be productive. There is of course more to learn, but I have found myself having to dig to much into tutorials or the forum.

### Ember

Ember also has a steep learning curve like Angular, I think it is easier to learn Ember well than Angular but it requires a highest learning investment at the beginning. Angular in constract lets you do some amazing things without knowing to much about it.


## Developer productivity	

You could argue that the learning curve is not so important and what really matters is how productive you are with it.

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 4       | 2 	     | 4     | 5 |

### Angular

Once you know Angular you can very quite productive with it. No doubt about that. It just doesn't get the highest score because I think that Ember beats Angular in this category.

### Backbone

In my opinion Backbone gets a low score because it requires you to write a lot of boilerplate code yourself. This threats developer productiviy in a major way by having to write unnecessary code.

### CanJS

Not much to say here. CanJS gives you all the features to be productive with it. 

### Ember

Ember really shines here. Because Ember is full of conventions it does a lot of stuff automatically for you. All you need to do is apply those conventions and Ember will find and show the right model and views.

## Community

How easy is to find help and experts in this framework?

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 4       | 5 	     | 3     | 4 |

The Backbone community is huge, there is no doubt about us. The Angular and Ember communities are quite big as well. The CanJS community on the other hand is quite small in comparison, but fortunatelly is quite active and helpful.

## Ecosystem

Is there an ecosystem of plug-ins and libraries build to work with you choosen framework?

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 4       | 5 	     | 2     | 4 |

Here again Backbone beats the others hands down. There are tons of plug-ins for it. The Angular ecosystem is getting quite interesting as well. I think the Ember ecosystem is a bit less developed but it should get better due to Ember's popularity. CanJS has the smallest ecosystem if any.

## Size

This is important some times, specially for mobile development.

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 4       | 5 	     | 5     | 2 |


### Size library alone (no dependecies, just min)

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 80       | 18 	     | 33     | 141 |

### Size with dependencies

At 80K __Angular__  is the only library of the bunch that doesn't require extra libraries to work.

However all the other need other libraries to work:

__Backbone__ needs at least Underscore, Zepto and a template engine probably like Mustache. This is 61K.

__CanJS__ needs at least Zepto. This is 57K

__Ember__ needs jQuery and Handlebars. This is 269K.

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 80       | 61 	     | 57     | 269 |

So Ember gets a lot score because it takes a lot more bytes than the other three.


## Performance

I consider performance not to be an important factor on choosing a framework because they all are performant enough for most of the thing they will be used for. But this of course depends on what you are doing with it.


| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 4       | 5 	     | 5     | 5 |


I have seen and made many performance test with these libraries e.g. see this [one](http://jsperf.com/angular-vs-knockout-vs-ember/118). But I am not convinced on the reliability of these performance test. It is really hard to be sure that the test is really testing the right things when it comes to view bindings because some libraries have the own run loops like Ember.

I have put a lower score to Angular based on the fact that it cannot possibly be as performant as the others. This is because Angular polls objects for changes instead of listening for changes.

## Personal taste

This is probably one of the biggest factors when choosing a library. Do you like extending html with library specific attributes (Angular). Do you like using a template engine? (Backbone, CanJS and Ember).

## Tally

Well, putting all together this is my tally:

<iframe width='500' height='300' frameborder='0' src='https://docs.google.com/spreadsheet/pub?key=0Alu_S-aQLtvvdGpNaWJxMFlQM0pLM1JiSGd5MjJMYnc&output=html&widget=false'></iframe>

Overall is a tight competition, there are no clear winners or loosers. So I guess it all comes down to personal preference.









