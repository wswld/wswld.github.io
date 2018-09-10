---
layout: post
title: I Created an Issue in a Flask-RESTful Repository, You Won't Believe What Happened Next
date: 2018-06-14 17:00:00.000000000 +03:00
type: post
published: true
password: ''
status: publish
categories: []
tags:
- code
- development
- exceptions
- Flask
- Flask-RESTful
- python
- software
- FOSS
author:
  login: wswld
  email: seva17@gmail.com
  display_name: wswld
  first_name: ''
  last_name: ''
---

Two and a half years ago (Dec 4, 2015) I've [written](
http://bardakist.com/2015/12/07/overriding-default-werkzeug-exceptions-in-flask/) 
about a minor problem in Flask-RESTful, that puzzled and amused me. An issue [was 
created](https://github.com/flask-restful/flask-restful/issues/545) on the same 
day. I then went on with my life, hoping for someone to step in and fix this 
trivial design inconsistency. Several people including the repo maintainer 
enthusiastically expressed support for proposed changes but no code was ever 
commited.

Fast forward almost a year (Oct 30, 2016), I noticed this issue while skimming 
through my  Github activities and decided to open a PR. I wasn't using Flask at 
the moment, so I was doing it for the greater good. Having commited some code 
and hoping for the maintainer to review it ASAP, I switched to other 
commitments. Needles to say there was no review in several days, weeks 
and even months. Feeling more and more irritated with the whole affair, I 
emailed the maintaner and several other people, whose PRs have 
been merged recently - to no avail. I gave up and decided to finally let it go. 

Fast forward almost two years (Jun 6, 2018) the maintainer of the repository 
suddenly started reviewing the code. It is now merged for good and I'm glad to 
have contributed to the project, but somehow this whole ordeal of trying to 
commit my trivial change for almost 2.5 years makes me worried about the 
ever-increasing entropy of Open Source. Perhaps several thousand new libs and 
frameworks were created last couple of minutes. Do we have the guts to 
support and maintain it all? Could it be that we got carried away with a sexy 
idea of starting our own new thing and developed intolerance for the actual 
grind and commitment?
