---
layout: post
title: My Take on Yandex Pre-interview Python Assignment
---

I've applied for a junior Python position at Russian internet giant Yandex (very similar to Google). And although my application has been rejected, due to lack of experience, I think their little pre-interview test and my take on that may be of interest to any inquisitive pythonista. Note, that this has never been properly translated into English before, so this is probably exclusive in that regard.

<h3>Assignment I</h3>

There are two lists of different length. The first one contains keys, the second - values. Write a function, that would create a dict out of these lists. If the key doesn't have a value - it should equal <tt>None</tt>, if the value doesn't have a key, it should be omitted.

``` python
def get_dict(list1, list2):
    ret = dict(map(None, list1, list2))
    if ret.get(None, False):
        ret.__delitem__(None)
    return ret
```

<h3>Assignment II</h3>

Login should start with latin symbol, contain latin symbols, digits, dots and hyphens, but end only with a latin symbol or a digit. Minimum length is 1 symbol, maximum - 20 symbols. Write a function that checks strings for correspondence with these rules. Think of several methods of solving this problem and compare them.

``` python
import re
import time

def check1(login):
    ret = False
    if re.match('^[a-zA-Z][a-zA-Z0-9\-\.]{0,19}(?<![\-\.])$', login):
        ret = True
    return ret

def check2(login):
    ret = False
    if (len(login) >= 1 or len(login) <= 20) and login[0].isalpha() and (login[-1].isalpha() or login[-1].isdigit()):
        for a in login[1:-1]:
            if a.isalpha() or a.isdigit() or a == '-' or a == '.':
                ret = True
    return ret

def compare(login):
    tm = time.time()
    check1(login)
    print(time.time() - tm)
    tm = time.time()
    check2(login)
    print(time.time() - tm)
```

<h3>Assignment III</h3>

There are two tables <tt>users</tt> and <tt>messages</tt> (I changed names and messages to non-Cyrillic):

<table>
<caption>users</caption>
<tr><td>UID</td><td>name</td></tr>
<tr><td>1</td><td>John Doe</td></tr>
<tr><td>2</td><td>Natalie Knaph</td></tr>
<tr><td>3</td><td>Johnatan Yozo</td></tr>
</table>

<table>
<caption>messages</caption>
<tr><td>UID</td><td>msg</td></tr>
<tr><td>1</td><td>Hello, John!</td></tr>
<tr><td>3</td><td>Send me the card, quickly.</td></tr>
<tr><td>3</td><td>I'm waiting on the corner of 5th and Lafayette</td></tr>
<tr><td>1</td><td>This is me again. Please message me more often.</td></tr>
</table>

Create a SQL query, that would return two fields: "User name" and "Total amount of messages".

``` sql
SELECT users.name AS "User name",count(*) AS "Total amount of messages" 
FROM users 
JOIN messages ON users.uid = messages.uid 
GROUP BY users.uid
```

<h3>Assignment IV</h3>
Suppose you have a generic <tt>access.log</tt>. How to get 10 most frequent IP-addresses using standard terminal tools? How to do that with Python?

``` python
# BASH:
grep -o '[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}' access.log | sort -n | uniq -c | sort -n -r | head -10

# PYTHON:
import sys
import re

all = re.findall("[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}", open(sys.argv[1], 'r').read())
srt = sorted(all, key=all.count, reverse=True)
unq = []
for m in srt:
    if not m in unq:
        unq.append(m)
print unq[0:10]
```

If you can think of a better way to solve any of these, let me know.