---
layout: post
title: "My experience with Backbone, Ember and CanJS"
date: 2012-08-18
comments: true
external-url:
categories: backbone ember canjs javascript
---

Before coming to the JavaScript world I was doing Flex development, so I was quite familiar with MVC paradigm in the front-end side of applications. 

When I first started using JavaScript I learnt to do things in the usual way of that time - which is having the DOM as the source of truth. e.g. We will add elements to the DOM and if we wanted to know the length of a collection we will just use jQuery to ask the DOM for how many elements there were.

I soon grew tired of this way of doing things, it was just messy.

Backbone
---------

[Backbone](http://backbonejs.org/) captured a lot of attention when it came up, it was the first MVC framework that looked easy to get into. So I decided to try it. It was a great start, the structure of my code started to resemble a lot more what I was used to in Flex e.g. collections, views, controllers. 

But there were many times where I would think "What? Do I need to do this myself? Shouldn't Backbone take care of this?". For example when a collection changed the view needed to know and I had to hook all these events myself.

Another thing that was a let down is that there is no build-in way of doing live binding in the views. You have to do this yourself e.g. re-render a view manually when something changes (via events). Also doing nested views was more complex that it needed to be.

Backbone just left me wanting a lot more from an MVC framework. So I decided that Backbone was not my cup of tea, it wasn't really doing enough useful things for me.

Ember
-----

[Ember](http://emberjs.com/) also came up with quite a hype and it looked like the perfect fit for everything I was expecting. It has live binding, a great object model and an excellent template engine. 

It takes a while to learn as it deviates quite a bit from the usual way of doing JavaScript, Ember relies heavily on binding instead of events. I got a lot into it, even did a talk about it in my local user group.

I liked it but I wasn't completely happy with it because:

- It is huge, totally overkill for small projects or mobile apps.
- Performance wise it is not that good either, quite slow compared to Backbone. <http://jsfiddle.net/jashkenas/CGSd5/>
- It is hard to debug, when something fails it gives you very obscure error messages that are hard to track back to the source.
- It adds scripts tags around elements, which breaks CSS styling in some cases.
- It required me to declare lots of small components, so for simple things I ended up with too many objects.
- It also force me to declare objects in the global space, I couldn't find a way of not having to do this.


CanJS
----

Then came [CanJS](http://canjs.us/) (without the hype of the other two), it is a reboot of the venerable JavascriptMVC project. Bitovi has done a great job at making JavascriptMVC a lot more accessible to newcomers.

CanJS looked intriguing, so I decided to use it in my next project. It was a great success for me. It stroked a great balance between Backbone and Ember. The features I like a lot in CanJS are:

- It is quite small, just a bit bigger than Backbone. Tiny compared to Ember.
- It is fast (as fast as Backbone). <http://jsfiddle.net/sporto/Ek9am/>
- It has live binding out of the box, and they work perfectly well (although I prefer handlebars, I can live with EJS).
- It is a lot more general purpose than Backbone and Ember. For example CanJS has an observable object that can be used in a huge variety of situations, not just when doing MVC.

CanJS has become my MVC library of choice. I really recommend you give it a go in your next project.

The future
----------

I am planning to keep using CanJS in the future, but [Angular](http://www.angularjs.org/) looks very appealing, I really like the declarative way of doing things directly in html without having to use templates. I am planning to give Angular a go soon.
