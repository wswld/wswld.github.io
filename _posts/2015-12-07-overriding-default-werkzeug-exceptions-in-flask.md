---
layout: post
title: Overriding Default Werkzeug Exceptions in Flask
date: 2015-12-07 17:47:17.000000000 +03:00
type: post
parent_id: '0'
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
- how-to
- python
- software
- werkzeug
meta:
  _wpcom_is_markdown: '1'
  _edit_last: '10080708'
  geo_public: '0'
  _rest_api_published: '1'
  _rest_api_client_id: '11'
  _publicize_job_id: '17565767605'
author:
  login: wswld
  email: seva17@gmail.com
  display_name: wswld
  first_name: ''
  last_name: ''
---

Let's play a game here. What HTTP code is this exception:

``` javascript 
{ 
  "message": "The browser (or proxy) sent a request that this server could not understand." 
}
```

No no, you don't look at the code in response! That's cheating! This is 
actually a default Werkzeug description for `400` code. No shit. I thought 
something is bad with my headers or encryption, but I would never guess simple 
`Bad request` from this message. You could use a custom exception of course, 
the problem is, that the very useful `abort(400)` object (it's an `Aborter` in 
disguise) would stick with the default exception anyway. Let's fix it, shall we?

There may be several possible ways of fixing that, but what I'm gonna do is 
just update `abort.mapping`. Create a separate module for your custom HTTP 
exceptions `custom_http_exceptions.py` and put a couple of overridden 
exceptions there (don't forget to import `abort` we'll be needing that in a 
moment):

``` python 
from flask import abort 
from werkzeug.exceptions import HTTPException
class BadRequest(HTTPException): 
    code = 400 
    description = 'Bad request.'
class NotFound(HTTPException): 
    code = 404 
    description = 'Resource not found.' 
```

These are perfectly functional, but we still need to add it to the default 
`abort` mapping:

``` python 
abort.mapping.update({ 
    400: BadRequest, 
    404: NotFound 
})
```

Note that I import `abort` object from `flask`, not `flask-restful`, only the 
former is an `Aborter` object with mapping and other bells and whistles, the 
latter is just a function.

Now just import this module with `*` to your *app* Flask module (where you 
declare and run your Flask app) or someplace it would have a similar effect on 
runtime.

Note that you also should have the following line in your config because of [
this issue](http://stackoverflow.com/questions/34066290/custom-abort-mapping-exceptions-in-flask):

``` python 
ERROR_404_HELP = False 
```

I'm not sure why this awkward and undocumented constant isn't `False` by 
default. I opened [an issue](
https://github.com/flask-restful/flask-restful/issues/545) on GitHub, but no 
one seems to care.
