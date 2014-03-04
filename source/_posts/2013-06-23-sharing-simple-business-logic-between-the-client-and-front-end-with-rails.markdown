---
layout: post
title: "Sharing simple business logic between the client and front end with Rails"
date: 2013-06-23 08:26
comments: true
categories: javascript ruby rails
---

Recently I was trying to find out a way of sharing some business logic between the client and the server. We have a reactive UI where results get calculated as you change values. These are complex formulas we rather not write in both Ruby and JavaScript so we don't have to maintain them in two places.

The easiest solution would have been to make ajax calls to the server with the parameters get the results. But we wanted a UI that reacts immediately so needed to have the code loaded in the browser.

So this a solution I come up with, in summary:

- Formulas are stored as plain JavaScript
- Ruby Models load and use those formulas using V8
- The formulas are served to the front end as partials

Here is a rundown of what I did step by step. I will use a simple interest formula in this example.

## Storing the formulas 

I needed the formulas to be accessible as partials so I added them in the view folder:

In app/views/formulas/_interest.js

```js
	function formula(principal, rate, years) {
		return principal * rate * years;
	}
```
	
## Using the formulas in my Ruby models

In order to run the JavaScript on the server you will need __therubyracer__ gem, in Gemfile:

	gem 'therubyracer'

In app/models/contract.rb:

```ruby
	class Contract

		def initialize(principal, rate, years)
			@principal, @rate, @years = principal, rate, years
		end

		def interest
			cxt = V8::Context.new
			source = Rails.root.join('app', 'views', 'formulas', '_interest.js')
			cxt.load(source)
			cxt.eval("formula(#{@principal}, #{@rate}, #{@years})")
		end

	end
```
	
The JavaScript code is loaded inside the interest method and ran with the provided parameters. In this way the code is accessible as a plain ruby method and only needs to be tested in one place. In my test:

```ruby
	require 'spec_helper'

	describe "Contract" do

		let(:principal) { 100 }
		let(:rate) { 0.5 }
		let(:years) { 5 }
		let(:contract) { Contract.new(principal, rate, years) }

		it "returns the right interest" do
			expect(contract.interest).to eq(250)
		end
		
		...
	end
```
	
## Loading the formulas in the front-end

I couldn't find a way of injecting the JS code into the assets pipeline, so instead I just loaded in a partial that gets loaded in the application layout.

In app/views/layouts/application.html.erb:

	<%= render 'formulas/index' %>
	
In app/views/formulas/_index.html.erb

```js
	<script>
		(function (APP) {
			"use strict";
			(function (formulas) {
				formulas.interest = <%= render 'formulas/interest.js' %>
				...
			})(APP.formulas || (APP.formulas = {}));

		})(window.APP || (window.APP = {}));
	</script>
```
	
This piece of JavaScript creates a global object APP if not found, then an object formulas inside APP and finally adds my interest formula to APP.formulas.interest.

##Using the formulas in the front end

Once the formulas are loaded as shown above they can be used as:

```js
	var res = APP.formulas.interest(principal, rate, years);
```
	
---------------

So this is a way of sharing code between the back and the front end. If you how how could be improved or of a cleaner way altogether I would love to hear about it, maybe some DSL that can be used in both places.