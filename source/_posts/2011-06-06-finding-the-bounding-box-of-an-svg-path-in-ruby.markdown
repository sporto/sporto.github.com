---
layout: post
title: "Finding the bounding box of an SVG path in Ruby"
date: 2011-06-06 09:32
comments: true
categories: 
---

Here is a way for finding out the bounding box (left, right, width and height) of an SVG path in Ruby. You will need [ImageMagick](http://www.imagemagick.org/) and [RMagick](http://rmagick.rubyforge.org/) for this. In Javascript this is quite simple using the getBBox method, but I couldn't find a similar thing in Ruby.

First install ImageMagick if you don't have it already. I like to use Homebrew:

	brew install imagemagick
	
Then install RMagick

	gem install rmagick
	
With these two requirements in place you can find the bounding box using a script like this:

```ruby

	require 'rubygems'
	require 'RMagick'
	
	def find_bounding_box(path)
		include Magick	
	
		#create a drawing object
		drawing = Magick::Draw.new
	
	
		#create a new image for finding out the offset
		canvas = Image.new(1500,1000) {self.background_color = 'white' }
	
		#draw the path into the canvas image
		drawing.path path
		drawing.draw canvas
	
		#trim the whitespace of the image
		canvas.trim!	
	
		#here is the bounding box information we are looking for
		{ :x=> canvas.page.x, :y=>canvas.page.y, :width=> canvas.columns, :height=> canvas.rows}
	end
	
```

Let's test it:
	
	
```ruby

	svg_path = "M555.545,95.489c-9.67,4.589-19.34,9.178-29.01,13.767c9.097,19.235,18.194,38.47,27.291,57.705c9.667-4.598,19.335-9.196,29.002-13.794C573.733,133.941,564.64,114.715,555.545,95.489z"
	
	bbox = find_bounding_box(svg_path)
	
	puts "x="+bbox[:x].to_s
	puts "y="+bbox[:y].to_s
	puts "width="+bbox[:width].to_s
	puts "height="+bbox[:height].to_s
	
```
