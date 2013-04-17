---
layout: post
title: "A comparison of Angular, Backbone, CanJS and Ember"
date: 2013-04-12 19:00
comments: true
categories: javascript, mvc
---

Selecting a JavaScript MVC framework can be hard work. There are so many factors to consider and so many options out there that selecting a framework can be overwhelming. To have an idea of all the possible alternatives have a look at [TodoMVC](http://todomvc.com/).

I have had the opportunity to use four of these frameworks: [Angular](http://angularjs.org/), [Backbone](http://backbonejs.org/), [CanJS](http://canjs.us/) and [Ember](http://emberjs.com/). So I decided to create a comparison to help you decide which one to use. I will go through several factors that you might want to consider when choosing one.

To each factor I have assigned a score between 1 and 5. Where 1 is poor and 5 is great. I have tried to be impartial in my comparison, but of course my objectivity is heavily compromised as the scores are based mostly on my personal experience.

<p>
{% img /images/logos/angular.png 160 160 'Angular.js' 'images' %}
{% img /images/logos/backbone.png 160 160 'Backbone.js' 'images' %}
{% img /images/logos/can.png 160 160 'Can.js' 'images' %}
{% img /images/logos/ember.png 160 160 'Ember.js' 'images' %}
</p>

## Features

<img src="https://docs.google.com/drawings/d/1idTwF7_uA0g3c1-ObSIfS6f9x_sGqbrWaFXYMXnKZkY/pub?w=247&amp;h=69">

There are really important features a framework should have to provide the necessary foundation to build useful applications. Does it do view bindings? two way bindings? filters? computed properties? dirty attributes? form validation? etc. This can be a very long list. Below is a comparison of what I consider the really important features in a MVC framework:

Feature                | Angular | Backbone | CanJS | Ember 
----                   | ------- | -------- | ----- | ----- 
Observables            | y       | y        | y     | y     
Routing                | y       | y        | y     | y     
View bindings          | y       |          | y     | y     
Two way bindings       | y       | -        | -     | y     
Partial views          | y       | -        | y     | y     
Filtered list views    | y       | -        | y     | y     

__Observables__: Objects that can be observed for changes.

__Routing__: Pushing changes to the browser url hash and listening for changes to act accordingly.

__View bindings__: Using observable objects in views, having the views automatically refresh when the observable object change.

__Two way bindings__: Having the view push changes to the observable object automatically, for example a form input.

__Partial views__: Views that include other views.

__Filtered list views__: Having views that display objects filtered by a certain criteria.

### Scores

So based on these features my scores are:

Angular | Backbone | CanJS | Ember 
------- | -------- | ----- | ----- 
5       | 2 	   | 4     | 5 

It is important to note that __Backbone__ can do most of this things with a lot of manual code or with the help of plug-ins. But I am only considering the available features in the core framework.


## Flexibility

<img src="https://docs.google.com/drawings/d/1Q-Mkke4HROs9wquvg4Dc9dDpkb_4b4UQT3bE4VBMUD8/pub?w=247&amp;h=69">

There are hundreds of awesome plug-ins and libraries out there that do specialised things. They usually do these things better than what comes bundle with a framework. So it important to be able to integrate these libraries with the chosen MVC framework.

__Backbone__ is the most flexible framework as it is the one with the less conventions and opinions. You are required to make a lot of decisions when using Backbone.

__CanJS__ is almost as flexible as Backbone as it allows you to easily integrate other libraries with minimum effort. With CanJS you can even use a totally different rendering engine if you want, I have used [Rivets](http://rivetsjs.com/) extensively with CanJS without any issues. Although I recommend using what comes with the framework.

__Ember__ and __Angular__ are still flexible frameworks to some degree but you will find that you could end up fighting the framework if you don't like the way it does certain things. There are some things that you just need to buy into when using Ember or Angular.

Angular | Backbone | CanJS | Ember
------- | -------- | ----- | -----
3       | 5 	   | 4     | 3

## Learning curve and documentation

<img src="https://docs.google.com/drawings/d/1TyDLWOE3JhitMUfIFlkE8YPjowU6b8rpLNlRUe_2jlM/pub?w=247&amp;h=69">

### Angular

Angular has a very high wow factor at the beginning. It can do some amazing things - like two-way bindings - without having to learn much. And it looks quite easy at first sight. But after you have learnt the very basics it is quite a steep learning curve from there. It is a complex framework with lots of peculiarities. Reading the documentation is not easy as there is a lot of Angular specific jargon and a serious lack of examples.

### Backbone

The basic of Backbone are quite easy to learn. But soon you find that there are not enough opinions there to know how to best structure your code. You will need to watch or read a few tutorials to learn some best Backbone practices. Also you will find that you will probably need to learn another library on top of Backbone (e.g. [Marionette](http://marionettejs.com/) or [Thorax](http://thoraxjs.org/)) to get things done. So I don't consider Backbone the easier framework to learn.

### CanJS

CanJS is in comparison the easiest to learn of the bunch. Just by reading the one page website (http://canjs.us/) you will know most of what you need to be productive. There is of course more to learn, but I only had the need to reach for help in rare occasions (tutorials, forum, irc).

### Ember

Ember also has a steep learning curve like Angular, I believe that learning Ember is easier than Angular but it requires a highest learning investment at the beginning to get basic things done. Angular in contrast lets you do some amazing things without learning too much. Ember lacks this early wow factor.

### Scores

Angular | Backbone | CanJS | Ember
------- | -------- | ----- | -----
2       | 4        | 5     | 3

## Developer productivity

<img src="https://docs.google.com/drawings/d/1NxQUPyy7GRCS3S5i3JtA41yxiG3ylf2es-HgEpF_SQs/pub?w=247&amp;h=69">

After you learn the framework well what really matters is how productive you are with. You know: conventions, magic, doing as much as possible quickly.

### Angular

Once you know Angular well you be can very productive with it, no doubt about that. It just doesn't get the highest score because I think that Ember has gone a step further in this category.

### Backbone

Backbone requires you to write a lot of boilerplate code, which I think is totally unnecessary. This is in my opinion a direct threat against developer productivity.

### CanJS

CanJS neither shines nor disappoints in this area. But due to the low learning curve you can be quite productive with it very early on.

### Ember

Ember really shines here. Because it is full of strong conventions it does a lot of stuff automagically for you. All you need to do is learn and apply those conventions and Ember will to the right thing.

### Scores

Angular | Backbone | CanJS | Ember
------- | -------- | ----- | -----
4       | 2 	   | 4     | 5

## Community

<img src="https://docs.google.com/drawings/d/1JirNlFdiZOtEyCaJ9ICTAUT1t0On6YWcS7sw-EAPD58/pub?w=247&amp;h=69">

__How easy is to find help, tutorials and experts?__

The __Backbone__ community is huge, there is no doubt about that. You can find dozens of tutorials about Backbone, a very active community on StackOverflow and IRC. 

The __Angular__ and __Ember__ communities are pretty big as well. Also lots of tutorial and activity in StackOverflow and IRC, but not as much as Backbone. 

The __CanJS__ community on the other hand is small in comparison, but fortunately is quite active and helpful. I haven't found the smaller size of the CanJS community to be a liability.

Angular | Backbone | CanJS | Ember
------- | -------- | ----- | -----
4       | 5 	   | 3     | 4


## Ecosystem

<img src="https://docs.google.com/drawings/d/1F61JwcjmqQYpjf0QZLu4C-qWVMzmQWfKMM9cGzmlnsw/pub?w=224&amp;h=83">

__Is there an ecosystem of plug-ins and libraries?__

Here again __Backbone__ beats the others hands down. There are tons of plug-ins for it. The __Angular__ ecosystem is getting quite interesting as well with things like [Angular UI](http://angular-ui.github.com/). I think that the __Ember__ ecosystem is less developed but it should get better due to Ember's popularity. __CanJS__ has the smallest ecosystem if any.

Angular | Backbone | CanJS | Ember
------- | -------- | ----- | -----
4       | 5 	   | 2     | 4


## Size

<img src="https://docs.google.com/drawings/d/15tEa6aHRhCtDiliHQ3KHGFzgVNilvmirHCnTbe3HRQY/pub?w=258&amp;h=68">

This might be an important consideration, specially if you are doing mobile development.

### Size library alone (no dependecies, just min)

Angular | Backbone | CanJS | Ember
------- | -------- | ----- | -----
80k     | 18k      | 33k   | 141k

__Backbone__ is the smallest and people often point to this fact. But this is not the end of the story.

### Size with dependencies

At 80k __Angular__ is the only library of the bunch that doesn't require extra libraries to work.

However all the other need other libraries to work:

__Backbone__ needs at least __[Underscore](http://underscorejs.org/)__ and __[Zepto](http://zeptojs.com/)__. You can use the mini-templates in underscore for rendering views, but most of the time you will want to use a nicer template engine like __[Mustache](https://github.com/janl/mustache.js)__. This is __61K__.

__CanJS__ needs at least __Zepto__. This is __57K__.

__Ember__ needs __[jQuery](http://jquery.com/)__ and __[Handlebars](http://handlebarsjs.com/)__. This is __269K__.

Angular | Backbone | CanJS | Ember
------- | -------- | ----- | -----
80k     | 61k      | 57k   | 269k

### Scores

Angular | Backbone | CanJS | Ember
------- | -------- | ----- | -----
4       | 5        | 5     | 2

## Performance

<img src="https://docs.google.com/drawings/d/1Es5A8jSWVS6JXfTG-_F8bn4z-mdSAsGGGh9D9OoHOJM/pub?w=247&amp;h=69">

I don't consider performance to be a critical factor on choosing a framework because they are all performant enough for most of the things they will be used for. But this of course depends on what you are doing with it. If you are building a game performance should be a big consideration.

I have seen and made many performance tests with these libraries e.g. [this one](http://jsperf.com/angular-vs-knockout-vs-ember/118). But I am not totally convinced on the reliability of these tests. It is really hard to be sure that the test is really testing the right things and in the right way.

However, from what I have seen and read __CanJS__ seems to have the edge when it comes to performance, specially in rendering view bindings. On the other hand I believe that __Angular__ is the less performant based on the fact that it does dirty checking of objects. This cannot possibly be as performant as the others. [See this](http://stackoverflow.com/questions/9682092/databinding-in-angularjs/9693933#9693933).

### Scores

Angular | Backbone | CanJS | Ember
------- | -------- | ----- | -----
3       | 4        | 5     | 4

## Maturity

<img src="https://docs.google.com/drawings/d/1u9ynjn6jbQqdITYHYJVBzYv1WLttguq7G_MrRim5M8E/pub?w=247&amp;h=69">

Is this a mature framework, has it been proven in production, are there many website using it?

__Backbone__ has a ton of websites built with it. Its code base hasn't had major changes in the lasts two year which is a great thing from the maturity perspective.

Although __Ember__ is not that new, it has had major changes along the way, just reaching a stable form in the last couple of months. So at this time I don't consider it to be a mature framework.

__Angular__ seems more stable and proven than Ember. But not as much as Backbone.

__CanJS__ may seem like an unproven solution because you cannot find a ton of site built with it. But CanJS comes with a lot more backing than what you first perceive. CanJS is an extraction of __[JavaScriptMVC](http://javascriptmvc.com/)__ a library that has been around since 2008 and has lots of experience build in.

Angular | Backbone | CanJS | Ember
------- | -------- | ----- | -----
4       | 5        | 4     | 3

### Memory leak safety

This is an important consideration if you are building single page apps that are intended to  stay open for a long time. You don’t want your application to leak memory, this can be a real problem. Unfortunately this can happen quite easily, specially if you are creating listeners for DOM events yourself.

__Angular__, __CanJS__ and __Ember__ will deal with this effectively as long as you follow their best practices. __Backbone__ on the other hand requires you to do this work manually in a teardown method.

Angular | Backbone | CanJS | Ember
------- | -------- | ----- | -----
5       | 3 	   | 5     | 5

## Personal taste

This is probably one of the biggest factors when choosing a library. 

- Do you like declarative html? -> Angular
- Do you like using a template engine? -> Backbone, Can and Ember
- Do you like an opinionated framework? -> Ember
- Do you want a framework that stick closely to the original [SmallTalk MVC](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) pattern? -> None here, maybe CanJS is the closest.
- Do you want to use what seems cool at the moment? -> Ember, Angular

There is no way to score this.


## Tally

Well, putting all together this is my tally. Remember this is just my opinion, please let me know if you think I have scored a library really wrong.

<iframe width="100%" height="600" src="http://jsfiddle.net/sporto/5JVxh/embedded/result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

If you put the same weight to every factor it is a tight competition, there are no clear winners or losers. So I guess it all comes down to personal taste or how much weight you apply to each particular factor.

## A note about Backbone (Impartiality ends here)

I cannot finish my blog post without saying this, I have tried to stay impartial during my post but I want to share my current opinion about Backbone. It was a great library two years ago, but __there are better things now__. I believe that many people choose Backbone just because of its popularity, it is a vicious circle.

I strongly feel that you should think twice before using Backbone in new projects. This is mostly because of its lack of features and developer convenience. Backbone can be very tempting because of the big community and the ecosystem, but this advantage will dissapear as the other frameworks become more popular. __It is time to move on__.

__Update 18/04/2013__: Made it clear that the last section is just my opinion.




