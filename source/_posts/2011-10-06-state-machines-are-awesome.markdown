---
layout: post
title: "State machines are awesome"
date: 2011-10-06 09:29
comments: true
categories: general
---


Being mostly a self taught developer, nobody ever taught me what a state machine was. When I finally stumbled upon them I realised I have been missing out by not using them before. 

When I first started learning about them I couldn't find a good explanation in plain english. So here is my attempt to explain state machines in a simple way and why you should use them.

The code examples below are in Ruby using the [state_machine](https://github.com/pluginaweek/state_machine) gem. But this are general concepts for any language.

##What is a state machine?

In essence a state machine tracks the state of an object and describes the actions that this object can perform at any given time.

A state machine has several components: states, events and transitions.

##States

Let's say we have an 'order' object. We could use a state machine to track the state of our order. The order could have the following states:

- Open
- Placed
- Held
- Cancelled
- Shipped
- Returned

The subject of the state machine can be in only one state at any given  time. The order can either be 'Held' or 'Shipped'. These are the states. And we can have only one. 

If we want to have two states at the same time we would need to have two state machine running in parallel. 

##Events

We can define events that our state machine can perform. For example:

- Create
- Cancel
- Hold
- Ship
- Return

Not all events can be called at any time, the available events are dependant on the current state of the object.  A state machine manages this logic for us.

In Ruby (using state_machine) we could do something like this:

	order.can_cancel? #ask if the order can be cancelled at this time
	order.cancel #send the cancel event to the order

The following diagram shows the state machine flow.

<img src="https://docs.google.com/drawings/pub?id=178_CnXn19xnNumvnBY8BxIsJQ245rdoyzvUOwN-MQqM&amp;w=528&amp;h=413">

So as you can see in the diagram there are predetermined paths that the state machine can follow. It can move from the 'Created' state to the 'Placed' state but not to the 'Shipped' state.

The state machine will complain if you try to call an event that is not permitted depending on the current state.

##Transitions

Finally, when an event is called it may trigger a transition. For example the 'cancel' event could trigger the following transitions:

- 'placed' to 'cancelled'
- 'held' to 'cancelled'
- 'any' to 'cancelled'

Transitions are useful for triggering processes that should occur in particular cases. For example sending an email when the state of an order changes from 'placed' to 'cancelled'. 

##So why you should use state machines?

If it is not clear already the main reason to user them is because a state machine can manage all the complexity of knowing what events and states are available at any given time.

So, that's my understanding of state machines so far. I am sure I am missing out some stuff but I believe that the basics are here.
