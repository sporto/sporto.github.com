---
layout: post
title: "Concurrency: JavaScript vs Go"
date: 2013-08-18 07:40
comments: true
categories: 
---

The Go programming language has capture my interest latelly, there are a few things I like about it very much:

- Simplicity: It is a language with a small set of constructs
- Static typing: I want to know that my code is correct without having to write ridicuslous tests like checking what happens when you pass the wrong type to a function
- No classes: Instead of classes Go simply uses interfaces to determine what objects has what methods
- Concurrency primitives: Go has baked in all the necessary language constructs for dealing with concurrency in a clean and coherent way

As part of learning Go I wanted to see how it concurrency model compares to what we have in Node. So I created a simple prototype that captures a common pattern:

- Fetch a value from an API (value x)
- Grab another value from the API (value y) - in parallel with step 1
- Send those two values to the API

I made a simple Node server for this. (The code is here)[https://gist.github.com/sporto/6258909#file-server-js]. This server has the following API:

- server/x => returns "Hello"
- server/y => returns "World"
- server/concat?x=value&y=value => takes two values and returns the values concatenated

The Node Version
----------------

For the node code I am using promises, (here is the client code in JavaScript)[https://gist.github.com/sporto/6258909#file-client-js].

Note the following lines:

```js
	var request = require('request');
	var Q = require('q');
	 
	var defX = Q.defer();
	var defY = Q.defer();
	var oneAndTwo = Q.all([defX.promise, defY.promise]).then(processConcat);
	 
	var baseUrl = "http://localhost:8080";
	 
	request(baseUrl + '/x', makeValueHandler(defX));
	request(baseUrl + '/y', makeValueHandler(defY));
```

I create two deferreds: defX and defY. Then I create a promise that is the combination of defX and defY. Immediatelly after that I make the first two calls to the server in parallel. When those to calls are done the deferreds will be resolved and the function `processConcat` will be called with the results (in the correct order). 

The JavaScript code is not complex but is not straighforward either, you still need to jump around when reading the source code to understand exactly what is happening.

The Go Version
--------------

The complete client in (Go is here)[https://gist.github.com/sporto/6258909#file-client-go].

The key lines of code are below:

```go
	var cx chan string = make(chan string)
	var cy chan string = make(chan string)

	go getValue("/x", cx)
	go getValue("/y", cy)

	x := <-cx
	y := <-cy

	processConcat(x, y)
```

Here we create two channels, one for value x and the other for value y. Immediatelly after that we fetch the values from the server but using the special `go` keyword. This means that those call will be made in their own processes and thus in parallel.

The next two lines wait for the results of the API calls, the results are communicated using the channels. 

The Go code looks like typical sync code but it is all happing in parallel thanks to the goroutines. So this code is as fast (event faster) than the Node version but a lot simpler to understand.



