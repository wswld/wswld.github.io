---
layout: post
title: Why Static Site Generators aren't Good for My Blog
date: 2013-08-06 17:24:27.000000000 +04:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories: []
tags:
- blogging
- dynamic site
- dynamic sites
- internet
- Jekyll
- Octopress
- rant
- software
- Sphinx
- static sites
- technology
meta:
  _edit_last: '10080708'
  geo_public: '0'
  _publicize_pending: '1'
  tagazine-media: a:7:{s:7:"primary";s:102:"http://methodandgrit.files.wordpress.com/2013/07/screen-shot-2013-07-25-at-15-46-12-e1374753319968.png";s:6:"images";a:1:{s:102:"http://methodandgrit.files.wordpress.com/2013/07/screen-shot-2013-07-25-at-15-46-12-e1374753319968.png";a:6:{s:8:"file_url";s:102:"http://methodandgrit.files.wordpress.com/2013/07/screen-shot-2013-07-25-at-15-46-12-e1374753319968.png";s:5:"width";i:786;s:6:"height";i:648;s:4:"type";s:5:"image";s:4:"area";i:509328;s:9:"file_path";b:0;}}s:6:"videos";a:0:{}s:11:"image_count";i:1;s:6:"author";s:8:"10080708";s:7:"blog_id";s:8:"54975529";s:9:"mod_stamp";s:19:"2013-08-06
    17:24:27";}
  _oxford-post-cache: a:1:{s:15:"featured-images";a:0:{}}
  _oembed_1cd3994550187cb834acaadc3cda0788: "{{unknown}}"
  _oembed_280b98f1919aff95cb3b615442bdd2ce: "{{unknown}}"
author:
  login: wswld
  email: seva17@gmail.com
  display_name: wswld
  first_name: ''
  last_name: ''
---

I would be speaking about my own experiences and you may have noticed *my* in 
the title, which is there to remind you about the subjective nature of this 
article. I don't dare to speak about *your* blog or any other blog out there, 
but my own. I have no intention to convert you to my side, yet I would be very 
glad to see some of the like-minded people out there. I know they exist. My own 
research on the topic has unveiled, that although static site generators have 
this zealous following, there are sober voices in the crowd, appealing to 
common sense. This is exactly why Kevin Dangoor went 
["From Wordpress to Octopress and Back"](
http://www.kevindangoor.com/2012/03/wordpress-to-octopress-and-back/) or why 
Michael Rooney is ["Migrating from Octopress to WordPress"](
http://mikerooney.rowk.com/2013/02/11/migrating-from-octopress-to-wordpress-really/) 
— in the exact opposite direction from the majority of switchers.

![alt text](
{{ site.baseurl }}/assets/screen-shot-2013-07-25-at-15-46-12-e1374753319968.png 
"Illustrating the distinctive trend among majority of switchers.")

It doesn't boil down solely to Wordpress vs Octopress debate, as the issue in 
hands is much broader and may be represented as *dynamic site engines* vs 
*static site generators*. If you're not aware of the difference between the 
two, here are the basics: with *dynamic site* your content is generated 
dynamically by an application running on the server side, *static sites*, on 
contrary, are pre-generated or written outright in HTML. Basically, *dynamic 
sites* are a web-applications that could change their behaviour instantly, 
depending on input and other factors, while *static sites*: well, they're just 
HTML files passively sitting there, waiting for you to open and read them. 
Sure, with introduction of jQuerry, Java Script and HTML 5 to the mix, the 
difference gets a little blurry, but let's stop at this level.

So, *static* vs *dynamic*. It could be virtually anything: Tumblr vs Hyde, 
Blogger vs Pelican, Movable Type vs Jekyll, etc. Major differences between the 
two paradigms are more or less the same. It means that we should be really 
comparing the paradigms themselves, not their instances.

So, what are the lucrative advantages of the static generation model? What 
makes people switch so quickly and without looking back? I came up with 3 most 
important reasons:

1. Almost endless customizability.
1. It's mostly plain markup text files, that you can store in a Git repository.
1. Increased loading speed and security (*well, no features - no loopholes, 
   obviously*).

All three are valid points and at some moment I went down the static path 
myself with all three in mind. First I went with Jekyll, then switched to 
Octopress. At some point I even [tried](
http://stackoverflow.com/questions/1576340/using-sphinx-to-write-personal-websites-and-blogs
) to make Sphinx-generated site (*sic!*) work as a personal web-page, but lack 
of blog awareness and increasing complexity made me abandon this idea. When 
they say that running this kind of site is the easiest things to do, this is 
complete nonsense. In terms of comfortable workflow, I only can say a couple 
good things about pure Jekyll paired with GitHub Pages, but the result was so 
raw and required so much customization, that straightforward workflow was 
hardly an advantage. It is positioned as a toy for true geeks, tinkerers, but I 
don't see how anyone could really benefit from this kind of tinkering (*yeah, 
even geeks and tinkerers*). I work in IT and we are here mainly to solve 
problems, not create tons of complimentary issues. Instead of reinventing the 
wheel, you could as well invest your time into something, that really needs to 
be done, perhaps — writing good stuff for your website.

I'm not the only one who noticed it, but for several months I've been 
experimenting with static generators, I've hardly written half a post. It was a 
common problem, as I googled it. People dug so deep into the endless 
customization and switching routine that eventually they've stopped writing. I 
may sound conservative to some, but I still think that blogging is mainly about 
writing. Sure, to some extent, this problem is applicable to dynamic platforms 
too (ping pong between Wordpress and Blogger, anyone?), but with static 
generators it grows to catastrophic proportions. They are the ultimate time 
drain for nerds and wannabes. Sure, if your time worth basically nothing and 
constant tweaking your blog is the best part about having it, be my guest. To 
me it looks pretty much like that little joke from Fry&Laurie:

> Oh, yes, my boyfriend's a real DIY enthusiast, DIY mad. He's decorated the 
> whole room and he's put up all these bookshelves, and now he's writing all 
> these books to put on them.

Another seeming advantage of static site generators is their reliance on plain 
text, that could be utilized in distributed version control systems like Git. 
Most switchers assume, that they already use plain text and Git for code, why 
not use it for a blog? The problem is, that it also complicates things instead 
of easing them. Each generator has its own *super-easy-workflow™* with 
different special folders, commands and scripts — quickly it becomes a mess. In 
this regard Git is basically one more noose on your neck. I'd focus on one 
especially nasty implementation of Git in such workflow — Octopress. You should 
fork the original Octopress repo, the `_deploy` folder will be used for deploy 
to the pages repository and sources should be committed to the special 
`source` branch. Now imagine what would happen if a somewhat major update gets 
pushed to the upstream of Octopress and your copy has been significantly 
modified over time. As someone has put it: "Octopress is great, until it 
breaks". If you have some experience with Git and seen the Octopress workflow, 
you may imagine the hell it could possibly be. Actually, Octopress is itself a 
heavily modified version of Jenkins. No offence, but it all seems like Rupe 
Goldberg machine to me.

If you google it, you will find lots of people, performing full-featured 
benchmark tests of Wordpress vs Octopress with a complete disregard for the 
principal differences between the two. People start speaking of security and 
speed benefits of static sites, forgetting about all the advantages of dynamic 
sites, that come at the price of increased complexity and bloat (yes, I do 
think Wordpress is *a tad* bloated). Imagine the situation: you need to jot 
down a post draft on a public or someone else's PC? Will you be cloning the 
repo, installing ruby, Octopress — setting up all the environment to write a 
short post? What about mobile support? Should you attempt to pull Octopress to 
your mobile phone? What about preserving drafts in the cloud without publishing 
them, but having access to them virtually from anywhere and anytime? Can you 
really put a price on that? People start using Evernote or similar service for 
drafts, but does it really worth introducing another tool to your workflow? 
Does mobility and availability worth another couple seconds of load time? My 
own choice is comfort and efficiency. I want my blog to be complimentary to my 
technical endeavors, not the other way around.

I've started thinking that less is actually more a long time ago and it may 
apply to blogging as to almost any other area of our life. You may notice, that 
I don't even use the standalone Wordpress install, but the pretty limited 
hosted Wordpress site. I prefer to pay engineers at Wordpress $13 for domain 
mapping and settle for less choice in themes, plugins and other options to 
focus on writing. We're all too lost in the world of different platform and 
workflow options these days. Google it and you will see hundreds of rants about 
why platform A is *deliberately* better, than platform B, why static sites are 
better, than dynamic. You almost never hear that they help you in writing, no. 
It's all about SEO, storage space, customization, load speed and other 
insignificant stuff, not directly related to blogging. We're too obsessed with 
form and seem to forget about goddamn content. But, as I said before, it is all 
entirely subjective. You may still go down the static route, customize the ass 
out of your blog. You may even spend several years on writing your own static 
or dynamic blog engine from scratch, that will sure be absolutely unique and 
different from anything ever done before. Yet, I'm writing this post in a 
beautiful distraction-free WYSIWYG editor and my draft will be preserved 
online, when I press the *Save* button and no `rake deploy` is needed ever 
again.
