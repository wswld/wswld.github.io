---
layout: post
title: 'Humble Collection of Python Sphinx Gotchas: Part I'
date: 2013-09-02 19:11:05.000000000 +04:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories: []
tags:
- CSS
- customization
- design
- DIY
- documentation
- HTML
- interactive
- JavaScript
- jQuerry
- software
- Sphinx
- static sites
- style
- technology
- template
- theme
- writing
meta:
  geo_latitude: '0.000000'
  geo_longitude: '0.000000'
  geo_accuracy: '0'
  _edit_last: '10080708'
  geo_public: '1'
  _wpas_skip_google_plus: '1'
  tagazine-media: a:7:{s:7:"primary";s:90:"http://methodandgrit.files.wordpress.com/2013/09/screen-shot-2013-09-02-at-10-14-52-pm.png";s:6:"images";a:2:{s:90:"http://methodandgrit.files.wordpress.com/2013/09/screen-shot-2013-09-02-at-10-14-52-pm.png";a:6:{s:8:"file_url";s:90:"http://methodandgrit.files.wordpress.com/2013/09/screen-shot-2013-09-02-at-10-14-52-pm.png";s:5:"width";i:893;s:6:"height";i:615;s:4:"type";s:5:"image";s:4:"area";i:549195;s:9:"file_path";b:0;}s:57:"http://methodandgrit.files.wordpress.com/2013/08/tocs.png";a:6:{s:8:"file_url";s:57:"http://methodandgrit.files.wordpress.com/2013/08/tocs.png";s:5:"width";i:493;s:6:"height";i:326;s:4:"type";s:5:"image";s:4:"area";i:160718;s:9:"file_path";b:0;}}s:6:"videos";a:0:{}s:11:"image_count";i:2;s:6:"author";s:8:"10080708";s:7:"blog_id";s:8:"54975529";s:9:"mod_stamp";s:19:"2013-09-02
    19:11:05";}
  _oxford-post-cache: a:2:{s:10:"char-count";i:6753;s:15:"featured-images";a:0:{}}
  _wpas_skip_7306783: '1'
  publicize_facebook_url: https://facebook.com/561545037309516
  _post_restored_from: a:3:{s:20:"restored_revision_id";i:384;s:16:"restored_by_user";i:10080708;s:13:"restored_time";i:1415969970;}
  _wpas_done_7306783: '1'
  _publicize_done_external: a:1:{s:8:"facebook";a:1:{i:100003620757387;b:1;}}
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  _wpas_skip_tumblr: '1'
  _wpas_skip_path: '1'
  _wp_old_slug: humble-collection-of-sphinx-gotchas-1
author:
  login: wswld
  email: seva17@gmail.com
  display_name: wswld
  first_name: ''
  last_name: ''
---

### Gotcha 1: Getting Started with Sphinx Theming

**Update 02.10.2014:** It seems absolutely obvious to me, but apparently it is not that obvious to [some](http://stackoverflow.com/questions/25800264/bootstrap-theme-ugly-looking-font-for-inline-literal/25890178): you can't do anything about your Sphinx theme without some basic CSS and HTML skills. You can only change some of the theme parameters, if the developer was kind enough to add some, but it is [well 
covered](http://sphinx-doc.org/theming.html#using-a-theme) in the official 
documentation.

Customizing Sphinx visuals was somewhat upsetting to me at first, since the 
process is not straightforward and documentation is scarce. It could be a 
little bit user-friendlier. However, as soon as you get a grip on the basics, 
it gets pretty smooth and simple. You may have already tried copying over the 
default theme only to discover, that there is nothing particularly useful in 
there for an inquisitive scholar. Only one of the standard Sphinx themes is in 
fact complete, the rest of them simply inherit its properties to add some minor 
alterations. This theme is called *Basic* and it's a minimal sandbox template, 
the only theme that could be helpful for getting to the very bottom of Sphinx 
customization. Later you'd be able to inherit it and create a template, 
consisting only of alterations, but for a start it's OK to copy the *Basic* in 
its entirety.

Hopefully, you've already created a folder for your Sphinx project and 
initiated it by issuing:

``` sh
$ sphinx-quickstart
```

Or you may have an already existing Sphinx project, you want to theme — it's up 
to you. In your project folder create the `_themes` directory and then rename 
and copy the theme folder there. *Basic* theme, perhaps, should be located in 
`site_packages` folder of your active Python install.

Next step would be to change `conf.py` of your project, accordingly. First, we 
should make sure, that the following line is uncommented and correct:

``` python
html_theme_path = ['_themes']
```

Don't forget to check the value of the `html_theme` parameter above:

``` python
html_theme = 'renamed_basic'
```

![Basic theme without customization looks like vanilla HTML with no stylesheet 
whatsoever.]({{ site.baseurl }}/assets/screen-shot-2013-09-02-at-10-14-52-pm.png)

As of now you may start making alterations to the theme. You will find that 
HTML files in there aren't really HTML files, but templates (Sphinx uses 
[Jinja](http://en.wikipedia.org/wiki/Jinja_(template_engine)) template engine) 
with some staff automatically inserted on build. You can combine these 
automatic tags with basic HTML tags and as soon as you figure out how it works, 
you could move some of the interactive tags around, or get rid of some of them 
altogether. Don't forget to check whether you're not breaking anything though. 
Most visual aspects of Sphinx theme are modified through the main CSS file, 
which is located in the `static` folder. For *Basic* theme it would be 
`basic.css_t`. Notice, that `t` in the extension brings to our attention the 
fact, that this is a template. Other than that it could be viewed and edited as 
a simple CSS file. If you're interested in the values, provided by Sphinx 
templates and how you cam make use of them, consult [the official 
documentation](
http://sphinx-doc.org/templating.html#jinja-sphinx-templating-primer). If you 
want the main CSS file to have some other name, you can change that in 
`theme.conf`, there are also some other settings, that could be of interest.

### Gotcha 2: Spoiler

If you play with Sphinx templates for some time, you may want to start adding 
something more interactive. We could easily implement these kinds of things 
trough plain JavaScript or jQuery.

Let's go trough a little example project, to recognize, what could be achieved 
with combined power of Sphinx and JavaScript. You may notice, that as for 
common elements in HTML themes there is usually a way to address them in 
JavaScript, it's not that easy to introduce new classes and wrap some parts of 
the text into them. There is no custom `div` tag in Sphinx, unless you do that 
as `.. raw` directive which is not really a native way, and breaks lots of 
things. What I needed to do was to wrap some section of text into a `div` and 
make it collapsible on button pressed. You could achieve that by creating your 
own admonition and assigning it to certain HTML class.

For example:

``` text
.. admonition:: Request
		:class: splr
 		Request example and parameters.
```

You can then easily reference this admonition by its class in your JavaScript. 
For example you could put this jQuery script to your `page.html` template:

``` js
<script type="text/javascript">
$('.splr').css("background-color", '#F0F0F0');
$('.splr').css('box-shadow', '0px 0px 1px 1px #000000');
$('.splr > .last').hide();
$('.splr > .expanded + .last').show('normal');
$('.splr > p').click(function() {
    $(this).find('.last').hide();
    $(this).toggleClass('expanded').toggleClass('collapsed').next('.last').toggle('normal');
});
</script>
```

If implemented correctly, this script should turn the `splr` admonition into a 
collapsible drawer. I've been actively using this, when documenting HTTP APIs, 
since it's very helpful to hide JSON responses by default. Note, that you could 
also use this method for special CSS effects. Imagine, if aside from usual 
note, warning and tip you could have yellow, blue and purple boxes for whatever 
reason you can think of? Well, it couldn't be any simpler. Admonitions are good 
default containers for some parts of your text, that should differ in design or 
function from the rest of the page.

### Gotcha 3: Interactive TOC in Sidebar

![Not the worst case, but still a valid comparison of TOC trees in Default 
theme and our JavaScript enhanced Basic.]({{ site.baseurl }}/assets/tocs.png)

You may have noticed, that Sphinx often adds TOC to sidebar automatically, if 
it's not explicitly placed in the page itself. While this is certainly a very 
useful feature, sometimes things get out of control. I didn't use the worst 
case in the picture on the right, but it could get up to innumerable 1st level 
sections, each of them could have a number of subsections and so on. Sometimes 
it is a clear sign, that the page should be reorganized into multiple 
standalone pages, united by a category, but it's not always possible or needed. 
It's perfectly alright to have a long and deep TOC in some cases and *Default* 
Sphinx theme is terrible in that regard (*as of 2014 it is fixed and children 
categories are shown only for the currently selected one*).

You could use an updated version of the algorithm from the previous example to 
collapse some parts of the TOC by default. Note, how this script works with 
`ul` and `li` tags of the TOC tree list. Some stuff is applied recursively on 
the highest level of the list, some — on the subsequent levels. Especially 
noticeable with different styles applied to different levels of the list, so 
that you could tell whether it is 1st, 2nd or even 3rd level title. Here is the 
full script:

``` js
<script type="text/javascript">
$('.sphinxsidebar').attr('position', 'fixed');
$('.sphinxsidebar').css('position', 'fixed');
$("h3:contains('Table Of Contents')").css('border-bottom', '1px');
$("h3:contains('Table Of Contents')").text('TOC:');
$('.sphinxsidebarwrapper li').css('background-color', '#B8B8B8');
$('.sphinxsidebarwrapper li').css('box-shadow', '0px 0px 1px 1px #000000');
$('.sphinxsidebarwrapper li').css('color', 'white');
$('.sphinxsidebarwrapper li').css('border-color', '#000000');
$('.sphinxsidebarwrapper li > ul > li').css('background', '#D0D0D0');
$('.sphinxsidebarwrapper li > ul > li').css('box-shadow', '0px 0px 1px 1px #000000');
$('.sphinxsidebarwrapper li > ul > li > ul > li').css('background', '#F0F0F0');
$('.sphinxsidebarwrapper li > ul').hide();
$('.sphinxsidebarwrapper li > .expanded + ul').show('normal');
$('.sphinxsidebarwrapper li > a').click(function() {
  //hide everything
  $(this).find('li').hide();
  //toggle next ul
  $(this).toggleClass('expanded').toggleClass('collapsed').next('ul').toggle('normal');
});
</script>
```

Perhaps, it won't look very well, but it is a very simplified version to 
illustrate the concept. If you get the idea, you may alter or add `.css` 
methods to achieve more plausible visuals. You could also work on lower title 
levels, but you will have to figure it all out for yourself. In this version 
the snippet works best only with three headings levels from `<h1>` to `<h3>`, 
but it could definitely handle more.

That's it. <del datetime="2014-10-04T14:32:09+00:00">I've compiled these 
examples as a full-featured theme project on GitHub. I'm going to polish it to 
some extent and perhaps, implement more interactive stuff over time. Feel free 
to contribute to this little project.</del> (*Nope, never happened. Well, it did 
happened, but it was no good, sorry.*) If you come upon any issue with this 
little gotchas of mine, let me know. Sure, I've tested everything myself, but 
you never know. Also, you're free to use any of these examples as a building 
block in your work with no attribution, since they're rather generic and simple.
