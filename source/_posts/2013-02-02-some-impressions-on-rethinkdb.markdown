---
layout: post
title: "Some impressions on RethinkDB"
date: 2013-02-02 08:52
comments: true
categories: 
---

Tried RethinkDB for a week. Overall it gave me a very good impression. 

- Very easy to setup and run
- Setting up sharding and replication is quite simple as well
- The composable query language is a big plus. I really like the way I can build queries in my code by adding refinements if I need them. Reminds me of ActiveRecord in Rails.
- The admin interface is gorgeus and super useful, but I wish I could save my queries.
- The developers are very active in IRC and the forum, they responded all my questions very quickly.

Some rough edges I came accross are:

- Queries that require boolean logic (e.g. ‘where user.id IN (1,2,3) and user.active = 1’ in SQL) are very cumbersome in RethinkDB right now. The good thing is that this is being addressed on the next release.
- I manage to send several JavaScript expressions to the server that made RethinkDB spawn a bunch of processes that ate the machine CPU very quickly, making the DB almost unusable, I had to kill RethinkDB several times because of this. It would be nice if RethinkDB will  prevent this.
- At the moment you cannot connect instances of RethinkDB on different architectures, e.g. Ubuntu to Mac, 32 bits to 64 bits. Again this is in their list of things to address in the future.

At the end of our exploration we ended up with a flat data structure so we reverted back to our established SQL solution. But I look forward to use RethinkDB again soon. 

__Update July 2013__

Since I wrote this post some issues on RethinkDB has already been fixed. I was using rethink 1.5 at the time. Fixed issues are:

- "Queries that require boolean logic (e.g. ‘where user.id IN (1,2,3) and user.active = 1’ in SQL)" was addressed by the inclusion of array operations in RethinkDB 1.6. The equivalent ReQL command would be either of the following:

		r.table('users').filter(lambda user: r.expr([1,2,3]).contains(user['id']) & (user['active'] == 1))
		r.table('users').filter(lambda user: r.all(r.expr([1,2,3]).contains(user['id']), user['active'] == 1))

- JavaScript expressions (via the r.js command) have an optional timeout flag so the user can prevent single expressions from taking down the server, since RethinkDB 1.5 (see issue #69: https://github.com/rethinkdb/rethinkdb/issues/69)

