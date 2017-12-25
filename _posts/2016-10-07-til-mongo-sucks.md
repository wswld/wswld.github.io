---
layout: post
title: TIL: Mongo Sucks
---

MongoDB <tt>$nin</tt> and <tt>$ne</tt> and <tt>$exists: false</tt> queries are <em>super</em> expensive. I mean there is a point at which they are basically unusable. And I learned it the hard way, having developed a service, that relies heavily on this kind of queries to emulate queue-like workflow. 5+ million documents later and I’m hurriedly adding indexes for all flags that take more than 60 seconds to query for.

Information on this issue is limited to a short <a href="http://t.umblr.com/redirect?z=https%3A%2F%2Fdocs.mongodb.com%2Fv3.0%2Ffaq%2Findexes%2F%23using-ne-and-nin-in-a-query-is-slow-why&t=MTFiMDQ2MDBhNjhhYTc4YjZmMzk1MWU1YzhjZmVjODZiOGMxNTkwOCwxaGNFWlBCYw%3D%3D&b=t%3Aco8q4pQsR0pkOuSy1frx2w&m=1">FAQ entry</a> at Mongo docs and a couple StackOverflow <a href="http://t.umblr.com/redirect?z=http%3A%2F%2Fstackoverflow.com%2Fa%2F22639468&t=MDhjODlmOTY2MTY3NmQwYTdlYjczNmVlY2RjZWI4ZTM3NmJhOWJhNSwxaGNFWlBCYw%3D%3D&b=t%3Aco8q4pQsR0pkOuSy1frx2w&m=1">answers</a>. That’s it. Come on, it should be the first thing people see, when they open Mongo docs - a huge dialog you should scroll through three times before you can click accept.