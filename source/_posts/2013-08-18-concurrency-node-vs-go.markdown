---
layout: post
title: "Comparing Concurrency in Node and Go"
date: 2013-08-18 07:40
comments: true
categories: golang node javascript
---

The [Go programming language](http://golang.org/) has capture my attention lately. There are several things I really like about:

- Simplicity: Go is a small language but still very flexible, I appreciate this simplicity as it can be learnt fairly quick.
- Static typing: I like knowing that my code is correct without having to write a bunch of test you to deal with the lack of static typing.
- No classes: Instead of classes Go simply uses interfaces to determine what objects has what methods, I personally find this a flexible and powerful approach.
- Concurrency primitives: Go has baked in all the necessary language constructs for dealing with concurrency in a clean and coherent way.

As part of learning Go I wanted to compare how concurrency compares to Node. For this I created a simple prototype that captures a common pattern:

<img src="https://docs.google.com/drawings/d/1I-CqdRyXtQ0ZVFPh1kn8-jVC3jWMRgVYmv6EIH2NDxk/pub?w=486&amp;h=216">

- Fetch two values from different urls in parallel.
- When those two values have arrived, send them together to another url.

The Server
-----------

I made a simple Node server for this. [The code is here](https://gist.github.com/sporto/6258909#file-server-js). This server has the following API:

- http://localhost:8080/x => returns "Hello"
- http://localhost:8080/y => returns "World"
- http://localhost:8080/concat?x=value&y=value => takes two values and returns them concatenated

The Node Client
----------------

For the Node code I am using promises, [here is the complete client code in JavaScript](https://gist.github.com/sporto/6258909#file-client-js).

Note the following lines:

```js
	var request = require('request');
	var Q = require('q');
	 
	var defX = Q.defer(); // will be resolved when x arrives
	var defY = Q.defer(); // will be resolved when y arrives
	
	var oneAndTwo = Q.all([defX.promise, defY.promise]).then(processConcat);
	 
	var baseUrl = "http://localhost:8080";
	 
	request(baseUrl + '/x', makeValueHandler(defX));
	request(baseUrl + '/y', makeValueHandler(defY));
```

I create two deferreds: defX and defY. Then I create a promise that is the combination of defX and defY. 

Immediately after that I make the first two calls to the server in parallel. When those to calls are done the deferreds will be resolved and the function `processConcat` will be called with the results (in the correct order). 

The JavaScript code is not complex but is not completely straightforward either, you still need to scan up and down when reading the source code to understand exactly what is happening.

The Go Client
--------------

The complete client in [Go is here](https://gist.github.com/sporto/6258909#file-client-go).

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

Here we create two channels (a way of communicating when using concurrent processes in Go), one for value x and the other for value y. 

Immediately after that we fetch the values from the server but using the special `go` keyword. This means that those call will be made in their own processes so they will not block. These two requests happen in parallel.

The next two lines wait for the results of the API calls, the results are communicated using the channels. 

The Go code looks a lot like typical sync code but still it is all happing in parallel thanks to the goroutines. So this code is as fast (event faster) than the Node version but simpler to understand IMO.

Conclusion
----------

I am excited about Go, it has a lot to offer in multiple spaces. I will like to explore how something like Node Streams will look in Go.


