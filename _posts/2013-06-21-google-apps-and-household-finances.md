---
layout: post
title: Google Apps and Household Finances
date: 2013-06-21 01:41:06.000000000 +04:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories: []
tags:
- DIY
- Google
- household finances
- JavaScript
- software
- technology
meta:
  _edit_last: '10080708'
  _publicize_pending: '1'
  _wp_old_slug: google-script-and-household-finances
  geo_public: '0'
  _oxford-post-cache: a:1:{s:15:"featured-images";a:0:{}}
author:
  login: wswld
  email: seva17@gmail.com
  display_name: wswld
  first_name: ''
  last_name: ''
---
I'm in charge of household finances. By this I imply recording every single 
money transaction, occurring within our household, and providing on demand 
information about balance by account, overall household value, etc. I also 
issue monthly financial reports, so that me and my wife could analyze them and 
come up with a better spending strategy. When the whole thing works, it 
provides outstanding insights into the economic condition of our household. 
When it works.

Let’s assume you want to try it for yourself. Casual Google search will expose 
you to the vast amount of options, some of them are paid, some — free as in 
beer. But I dare you wouldn’t be *completely* satisfied with any of those apps 
and services:

* Some of the superb options out there are probably unavailable in your 
  country, if you live outside of US. For example, the famous 
  [Mint](https://www.mint.com) app is unavailable in my country without a good 
  VPN.
* Some of the projects don’t provide any mobile app or provide a 3rd party 
  solution, which will have all sorts of quirks and unsupported features.
* Only a small subset of projects gets it right. Experience is always to some 
  extent limiting even with the paid applications. I’ve never seen a personal 
  finance app that would provide all the features I need, and I’m not among the 
  most demanding users out there, believe me.
* You usually have no access to data except for export (if you're lucky). If 
  your data is somehow lost or ruined, it all happens under-the-hood. You won’t 
  be able to fix the thing yourself, unless you’re a developer and the app is 
  open source.
* Platforms may be also the thing. Some developers provide only iOS and Mac 
  binding and other try to please everyone except Apple users. I’m using Mac 
  and iPhone, and my wife has been a die hard Android zealot (*just kidding*). 
  Since she couldn't access the service herself, she notified me about every 
  transaction and I placed it into the system on her behalf. Can you imagine 
  how exhausted I was after several months of this workflow? It was all because 
  the app developer didn’t provide any sane way of syncing between Android and 
  iOS. Moreover, when my wife has switched to the iPhone, we discovered that 
  they don’t even provide syncing between iOS devices.

I’m speaking now about [Money by Jumsoft](http://www.jumsoft.com/money/). Not 
only it never really implemented syncing between different mobile devices, but 
it also failed to provide simple Mac to iPhone sync, the feature that is 
actually listed on their site. It used to sync through iCloud but when all the 
drama with iCloud not supporting databases occurred, they went back to Wi-Fi 
syncing. It never worked right and ultimately it crippled our data. Five months 
of carefully collected entries for every single transaction gone in a second. 
It was the moment I started to look for some other approach.

New approach came as an idea of using something simple and omnipresent. 
Something that would be available for all of the popular mobile and desktop 
platforms out there. I was thinking of using Google Docs. First, this idea 
seemed a little bit crazy (*it still does*), but then as it developed into a 
working prototype, it was actually a very smart move (*still crazy though*). 

Let’s break the concept down in theory:

*  We need some kind of **interface** for creating entries.
*  We need a **database** for storing entries.
* We need some kind of a **script** to process the results.

I looked closely at all the products provided by Google, and found all three 
components for implementing this concept:

*  Interface is going to be implemented as [Google Forms](
   http://support.google.com/drive/bin/answer.py?hl=en&amp;amp;answer=87809)
   document.
*  Database is best implemented as Google Spreadsheet. Jumping ahead of myself, 
   I can also reveal that the forms may write responses to the spreadsheets. 
   That’s exactly what we need!
*  Script was a little bit harder to figure out. First, I was hoping to process 
   everything with built-in spreadsheet functions, but it never really worked 
   for this kind of calculations. So I went with [Google Apps Script](
   https://developers.google.com/apps-script/). More of that in a bit.

It all look quite simple in theory, but in reality there are quite a few 
pitfalls here and there. I’m going to guide you trough all the major steps of 
implementing this concept in practice. 

Creating the form and collecting responses is not actually that hard. In Google 
Drive create the form document and populate it with elements. However, there 
are a couple of minor considerations which may affect your output data to some 
extent:

1. Watch the question titles as they directly affect the number of columns and 
   their caps. Section caption is of no interest in regard to the output data 
   though.
2. Questions do not override. So, if you have question called *Amount* in one 
   section and a question with the same name in another, it will result in two 
   columns *Amount* in your table with different values.
3. Also you may want to avoid nested sections as they complicate things a 
   little.

Here is an example of how you should **not** organize your form:

```
Section: Start
Element: Type [Bills, Food]

* Depending on type value go to one of the following:

- Section: Bills
Element: Subtype [Electricity, Cellular, Rent]
Amount: [Number]

* Commit results.

- Section: Food
Element: Subtype [Grocery, Fast Food]
Amount: [Number]

* Commit results.
```

Above you could probably notice an attempt to override the `Subtype` and 
`Amount` elements. It will result in a table with duplicate *Subtype* and 
*Amount* columns. It’s not that smart but it is the way it is. What I did is 
getting rid of subtypes and creating only the number of types I would certainly 
need. For example I have no *Bills* in types, but *Electricity*, *Cellular*, 
etc. So, I ended up with only one section like this:

```
Section: Start
Element: Type [Electricity, Cellular, Rent, Grocery, Fast Food]
Amount: [Number]
* Commit results.
```


I was geared towards creating the system as simple as possible, so I tried to 
exclude all the nice but potentially useless stuff leaving only the core 
functionality, that would be harder to break. In menu go `Responses` → 
`Choose response destination`. It will show a dialog window allowing you to 
choose a spreadsheet for your output data. Quite easy. If you test the form now 
you can see how the responses are being added to the table in your Google 
Drive. It even creates the human-readable timestamp column, which saves you the 
trouble of inputing the date and time yourself. Note, that *live* form may be 
accessed as a bookmark, or opened in mobile Google Drive apps for Android and 
iOS.

Creating the form and gathering the data aren’t exactly the trickiest stages of 
our little project. Providing  somehow valuable analytics on top of that data – 
that’s the real challenge. Let’s imagine, that we’ve collected all the data and 
now we want to analyze it. We’ll need some automatically updated metrics for 
our project:

* Household Balance
* Balance by Account

As soon as we figure out the algorithm for these two, we can easily implement 
any other metric (*Balance by Category?*) using the same method. Note that 
built-in spreadsheet functions probably won't work, we need something much more 
extensive and smart. Here comes the [Google Apps Script](
https://developers.google.com/apps-script/). It’s basically a full-featured 
scripting API for simple JavaScript web apps. Google provides the server and 
the ability to bind the script with the variety of Google Products. If you 
haven’t heard of that before, there are lots of examples and learning materials 
on their site — believe me, there is a lot of magic going on over there.

Let’s see how we can apply the scripting capabilities of Google Apps to our 
case. Open your destination spreadsheet and in menu go: `Tools` → 
`Script Editor`, in the `Google Apps Script` dialog window select `Spreadsheet`.
We will need a little script, that should run, when the spreadsheet is opened. 
Example script will leave you with `onOpen` and `readRows` functions. You can 
pretty much start with that.

Let’s see my take on the latter:

1. In the first part we start with `SpreadsheetApp.getActiveSpreadsheet()` and 
   end with row values as a 2-dimensional array `values`. Please keep in mind, 
   that the array is 2-dimensional.

    ``` js
    var ss = SpreadsheetApp.getActiveSpreadsheet();
    var sheet = ss.getSheetByName("Form Responses");
    var rows = sheet.getDataRange();
    var numRows = rows.getNumRows();
    var values = rows.getValues();
    ```

2. Now we should declare all the variables we will need. In my case it was the 
   output array and a separate variable for every account within family, 
   including the cash accounts:

    ``` js
    var arr = [];
    var hhld = 0.0;
    var vcsh = 0.0;
    var c4545 = 0.0;
    ```

   Note that `hhld` here stands for the total household value, `vcsh` for 
   Victor's cash, and `c4545` — a fictional credit card by its last four 
   digits.

3. Let’s iterate trough every row and match the entry with the account 
   accordingly:

    ``` js
    for (var i = 0; i <= numRows - 1; i++) {
        var row = values[i];
        if (row[1]=='Victor Cash')
        {
            vcsh = vcsh+parseFloat(row[3]);
            hhld = hhld+parseFloat(row[3]);
        }
        if (row[1]=='4545')
        {
            c4545 = c4545+parseFloat(row[3]);
            hhld = hhld+parseFloat(row[3]);
        }
    };
    ```

4. Now we may append the values to the array and return it:

    ``` js
    arr[0]=hhld;
    arr[2]=vcsh;
    arr[3]=c4545;
    return arr;
    ```

Let’s get to the main `onOpen()` function. It is shorter, but a little more 
tricky:

1. We open an active spreadsheet again, but this time use the other sheet, 
   since no one would ever consider mixing data and results a good idea. We 
   also should create two arrays.

    ``` js
    var ss = SpreadsheetApp.getActiveSpreadsheet();
    var targetSheet = ss.getSheetByName("Analytics");
    var array = [];
    var data = [];
    ```

2. Now let’s call the `readRows` function and get an array of account balances. 
   Note how we use `data[0]` to create a two-dimensional array.

    ``` js
    array = readRows();
    data[0]=array;
    ```
3. Finally, we should get the sheet range and assign data to it. Note that 
   `setValues` can work only with 2-dimensional arrays and it was the reason we 
   created one in the first place:

    ``` js
    var range = targetSheet.getRange('A2:M2');
    range.setValues(data);
    ```

You can test your app by both opening the spreadsheet or calling the `onOpen` 
function directly by clicking `Tools` → `Script Manager` → `Run`. For now the 
script doesn’t really work with mobile devices, so you will need to open the 
sheet on your machine to update the metrics. There are innumerable ways you
may improve this script. Please let me know if you come up with something cool.


**Update 18.06.2013:** I've found a way to automate the account counters and 
therefore enable full support for mobile devices. If you follow the workflow 
described in the post, you would end up with the script running only when 
opened, however for a spreadsheet paired with a form there is another kind of 
trigger available: *on form sent*. It runs the script every time, when the form 
is sent and unlike `onOpen` it seem to be performed on the side of Google, 
which makes our script platform-agnostic. Trigger can be enabled by going to 
`Resources` → `Current project's triggers`. In the dialog window add a new 
trigger and then select your main function (`onOpen`), next — 
`From spreadsheet` (yes, they have time-driven triggers too) and 
`On form submit`. You may test it now by filling the form from your phone and 
then checking the account counters in a couple of seconds or so.

**Update 14.11.2014**: We've spent some time with this system (a year or so), 
but eventually I've decided to make something more reliable: <del>check out my 
Le Ménage project on Github</del> (*got rid of the project becasue YNAB*).
