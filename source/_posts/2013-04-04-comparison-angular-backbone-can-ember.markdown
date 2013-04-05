---
layout: post
title: "A comparison of Angular, Backbone, CanJS and Ember"
date: 2013-04-04 19:00
comments: true
categories: javascript, mvc
---

Selecting a JavaScript MVC framework can be hard work. There are so many factors to consider and so many opinions out there. To have an idea of all the possible alternatives have a look at [TodoMVC](http://todomvc.com/). 

In this post I compare four leading libraries I have personal experience with, these are [Angular](http://angularjs.org/), [Backbone](http://backbonejs.org/), [CanJS](http://canjs.us/) and [Ember](http://emberjs.com/). I will go through several factors that you might want to consider when choosing a library. 

To each factor I have assigned a score from 1 to 5. Where 1 is poor and 5 is great. I have tried to be impartial in my comparison, but of course my objectivity is heavily compromised because the scores are mostly based on my experience with each of these frameworks.

{% img left /images/angular.png 350 350 'image' 'images' %}
{% img left /images/backbone.png 350 350 'image' 'images' %}
{% img left /images/can.png 350 350 'image' 'images' %}
{% img left /images/ember.png 350 350 'image' 'images' %}

## Features

There are really important features a framework should have to provide the necessary foundation to build usefull things. Can it do view bindings? two way bindings? filters? computed properties? dirty attributes? form validation? etc. This is a very long list. Below is a comparison of what I consider the really important features in an MVC framework:

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

__Partial views__: Views that include other views.

__Filtered list views__: Having views that display objects filtered by a certain criteria, without having to manually create a secondary list for this.

### Scores

So based on these features my scores are:

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 5       | 2 	     | 4     | 5 |

It is important to note that __Backbone__ can do most of this things with a lot of manual code or with the help of plug-ins. But I am only considering the available features in the core framework.


## Flexibility

There are hundreds of awesome plug-ins and libraries out there that do specialised things. They usually do these things better than what comes bundle with a framework. So it important to be able to integrate these libraries with the chosen MVC framework. 

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 3       | 5 	     | 4     | 3 |

__Backbone__ is the most flexible framework as it is the one with the less conventions and set ways of doing things. You are required to make a lot of decisions when using Backbone. 

__CanJS__ is almost as flexible as Backbone as it allows you to easily integrate other libraries with minimum effort. With CanJS you can even use a different rendering engine totally if you want, I have used [Rivets](http://rivetsjs.com/) extensively with CanJS without any issues. 

__Ember__ and __Angular__ are still flexible frameworks to some degree but you will find that you could end up fighting the framework if you don't like the way it does certain things. There are some things that you just need to buy into when using Ember or Angular.

## Learning curve and documentation

### Angular

Angular has a very high wow factor at the beginning. It can do some amazing things like two-way bindings without having to learn to much. And it looks quite easy at first sight. But after you have learnt the very basics it is quite a steep learning curve from there. It is a complex library with lots of peculiarities. Reading the documentation is not easy as there is a lot of Angular specific jargon and a serious lack of examples.

### Backbone

The basic of Backbone are quite easy to learn. But soon you find that there are not enough opinions there to know how to best structure your code. It takes a while to watch or read a few tutorials to learn some best Backbone practices. Also you will find that you will probably need to learn another library on top of Backbone (e.g. [Marionette](http://marionettejs.com/) or [Thorax](http://thoraxjs.org/)) to get things done. So I don't consider Backbone the easier framework to learn.

### CanJS

CanJS is in comparison the easiest to learn of the bunch. Just by reading the one page website (http://canjs.us/) you will know most of what you need to be productive. There is of course more to learn, but I only had the need to reach for help in rare occassions (tutorials, forum, irc).

### Ember

Ember also has a steep learning curve like Angular, I believe it is easier to learn Ember well than Angular but it requires a highest learning investment at the beginning. Angular in contrast lets you do some amazing things without learning too much. Ember lacks this early wow factor.

### Scores

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 2       | 4 	     | 5     | 3 |

## Developer productivity	

You could argue that the learning curve is not so important and what really matters is how productive you are with the framework after you know how to use it.

### Angular

Once you know Angular well you can quite productive with it. No doubt about that. It just doesn't get the highest score because I think that Ember has gone a step further in this category.

### Backbone

Backbone requires you to write a lot of boilerplate code yourself, which I think is totally unnecesarry. This is in my opinion a major threat against developer productiviy.

### CanJS

CanJS neither shines nor dissapoints in this area. But because of the low learning curve you can be quite productive with it very early on.

### Ember

Ember really shines here. Because Ember is full of conventions it does a lot of stuff automagically for you. All you need to do is apply those conventions and Ember will find and show the right models and views.

### Scores

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 4       | 2 	     | 4     | 5 |

## Community

__How easy is to find help, tutorials and experts?__

The __Backbone__ community is huge, there is no doubt about that. You can find dozens of tutorials about Backbone, a very active community on StackOverflow and IRC. The __Angular__ and __Ember__ communities are quite big as well. Also lots of tutorial and activity in StackOverflow and IRC, but not as much as Backbone. The __CanJS__ community on the other hand is small in comparison, but fortunatelly is quite active and helpful. I haven't found the small size of the CanJS community to be a liability.

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 4       | 5 	     | 3     | 4 |

## Ecosystem

__Is there an ecosystem of plug-ins and libraries?__

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 4       | 5 	     | 2     | 4 |

Here again __Backbone__ beats the others hands down. There are tons of plug-ins for it. The __Angular__ ecosystem is getting quite interesting as well with things like [Angular UI](http://angular-ui.github.com/). I think that the __Ember__ ecosystem is less developed but it should get better due to Ember's popularity. __CanJS__ has the smallest ecosystem if any.

## Size

This might be an important consideration, specially if you are doing mobile development.

### Size library alone (no dependecies, just min)

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 80k       | 18k 	     | 33k     | 141k |

__Backbone__ is the smallest and people often point to this fact. But this is not the end of the story.

### Size with dependencies

At 80k __Angular__  is the only library of the bunch that doesn't require extra libraries to work.

However all the other need other libraries to work:

__Backbone__ needs at least __[Underscore](http://underscorejs.org/)__ and __[Zepto](http://zeptojs.com/)__. You can use the mini-templates in underscore for rendering views, but most of the time you will want to use a nicer template engine like __[Mustache](https://github.com/janl/mustache.js)__. This is __61K__.

__CanJS__ needs at least __Zepto__. This is __57K__.

__Ember__ needs __[jQuery](http://jquery.com/)__ and __[Handlebars](http://handlebarsjs.com/)__. This is __269K__.

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 80k       | 61k 	     | 57k     | 269k |

### Scores

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 4       | 5 	     | 5     | 2 |

__Ember__ gets a low score because it takes a lot more bytes than the other three.

## Performance

I don't consider performance be a critical factor on choosing a framework because they are all performant enough for most of the things they will be used for. But this of course depends on what you are doing with it. If you are building a game performance will be a big consideration.

I have seen and made many performance tests with these libraries e.g. [this one](http://jsperf.com/angular-vs-knockout-vs-ember/118). But I am not convinced on the reliability of these tests. It is really hard to be sure that the test is really testing the right things and in the right way.

I have put a lower score to Angular based on the fact that it does dirty checking of objects. This cannot possibly be as performant as the others. [See this](http://stackoverflow.com/questions/9682092/databinding-in-angularjs/9693933#9693933).

### Scores

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 4       | 5 	     | 5     | 5 |

## Maturity

Is this a mature framework, has it been proven in production, are there many website using it?

__Backbone__ is has a ton of websites built with it. Its code base hasn't had major changes in the lasts two year. So it is quite a mature framework by now.

__Ember__ is relatively new. It has had major changes along the way, just reaching a stable form in the last couple of months. So I don't consider it to be a mature framework.

__Angular__ seems more stable and proven than Ember. But not as much as Backbone.

__CanJS__ may seem like an unproven solution because you cannot find a ton of site built with it. But CanJS comes with a lot more maturity than what you first perceive. CanJS is an extraction of [JavaScriptMVC](http://javascriptmvc.com/) a library that has been around since 2008 and has a ton of experience build in.

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 4       | 5 	     | 4     | 3 |

### Memory leak safety

This is an important consideration if you are single page apps that are intented to  stay open for a long time. You don’t want your application to leak memory. Unfortunatelly this can happen quite easily. Specially if you are creating listeners for DOM events yourself.

__Angular__, __CanJS__ and __Ember__ will deal with this effectively as long as you follow their best practices. __Backbone__ on the other hand requires you to do this work manually in a teardown method.

| Angular | Backbone | CanJS | Ember |
| ------- | -------- | ----- | ----- |
| 5       | 3 	     | 5     | 5 |

## Personal taste

This is probably one of the biggest factors when choosing a library. 

- Do you like declarative html? -> Angular
- Do you like using a template engine? -> Backbone, CanJS and Ember
- Do you like an opinionated framework? -> Ember
- Do you want a framework that stick closely to the original [SmallTalk MVC](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) pattern? -> None here, maybe CanJS is the closest.

There is no way to score this.


## Tally

Well, putting all together this is my tally. Remember this is just my opinion, please let me know if you think I have scored a library really wrong.

<iframe width='500' height='300' frameborder='0' src='https://docs.google.com/spreadsheet/pub?key=0Alu_S-aQLtvvdGpNaWJxMFlQM0pLM1JiSGd5MjJMYnc&single=true&gid=0&range=A1%3AE11&output=html'></iframe>

Overall is a tight competition, there are no clear winners or loosers. So I guess it all comes down to personal taste or how much do you weight a particular factor.









