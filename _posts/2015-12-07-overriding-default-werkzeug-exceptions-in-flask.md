---
layout: post
title: Overriding Default Werkzeug Exceptions in Flask
---

Let's play a game here. What HTTP code is this exception:

``` js
{
"message": "The browser (or proxy) sent a request that this server could not understand."
}
```

No no, you don't look at the code in response! That's cheating! This is actually a default Werkzeug description for <code>400</code> code. No shit. I thought something is bad with my headers or encryption, but I would never guess simple <code>Bad request</code> from this message. You could use a custom exception of course, the problem is, that the very useful <code>abort(400)</code> object (it's an <code>Aborter</code> in disguise) would stick with the default exception anyway.

Let's fix it, shall we?

There may be several possible ways of fixing that, but what I'm gonna do is just update <code>abort.mapping</code>. Create a separate module for your custom HTTP exceptions <code>custom_http_exceptions.py</code> and put a couple of overridden exceptions there (don't forget to import <code>abort</code> we'll be needing that in a moment):

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

These are perfectly functional, but we still need to add it to the default <code>abort</code> mapping:

``` python
abort.mapping.update({
    400: BadRequest,
    404: NotFound
})
```

Note that I import <code>abort</code> object from <code>flask</code>, not <code>flask-restful</code>, only the former is an <code>Aborter</code> object with mapping and other bells and whistles, the latter is just a function.

Now just import this module with <code>*</code> to your <em>app</em> Flask module (where you declare and run your Flask app) or someplace it would have a similar effect on runtime.

Note that you also should have the following line in your config because of <a href="http://stackoverflow.com/questions/34066290/custom-abort-mapping-exceptions-in-flask">this issue</a>:

``` python
ERROR_404_HELP = False
```

I'm not sure why this awkward and undocumented constant isn't <code>False</code> by default. I opened <a href="https://github.com/flask-restful/flask-restful/issues/545">an issue</a> on GitHub, but no one seems to care.