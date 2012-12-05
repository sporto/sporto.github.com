---
layout: post
title: "Don't hate tables"
date: 2011-08-01 09:30
comments: true
categories: 
---

Recently I was maintaining a project were the original developer did everything using lists to present data in rows and columns like a table. 

So to present something like this:

	<table>
		<tr>
			<th >Cosher </th>
			<td>To treat with special fondness.</td>
		</tr>
		<tr>
			<th style='padding-right:12px;'>Flamboyant </th>
			<td>strikingly bold or brilliant; showy: flamboyant colors.</td>
		</tr>
	</table>
	<br />

they were using a ul like this:

	<ul>
		<li>Cosher <span>To treat with special fondness.</span></li>
		<li>Flamboyant<span>strikingly bold or brilliant; showy: flamboyant colors.</span></li>
	</ul>
	
And then styling the list using CSS to make it look like a table.

This is a little less markup than a normal table but what for? It seems to me like the 'no tables' mantra taken to the extreme. This is perfectly appropriate content for a table. And using a table makes better sense because:

- Developer looking at the code will immediately know what the intention is
- The code is easier to maintain because you don't have to fight with the css to make something look like a table when it is not
- The display is consistent in any browser
- People disabling CSS will still see a table

So please use tables when appropriate, don't hate them.
