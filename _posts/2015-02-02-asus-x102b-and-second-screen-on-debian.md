---
layout: post
title: ASUS X102B and second screen on Debian
date: 2015-02-02 21:17:36.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories: []
tags:
- apt-get
- ASUS
- deb
- Debian
- firmware-linux-nonfree
- hardware
- how-to
- issue
- second screen
- X102B
- xrandr
meta:
  _edit_last: '10080708'
  geo_public: '0'
  _publicize_pending: '1'
  _post_restored_from: a:3:{s:20:"restored_revision_id";i:417;s:16:"restored_by_user";i:10080708;s:13:"restored_time";i:1422912010;}
author:
  login: wswld
  email: seva17@gmail.com
  display_name: wswld
  first_name: ''
  last_name: ''
---
I've stumbled upon a very capricious piece of hardware lately, which is ASUS 
X102B. Basic (very basic) video seems to work out of the box, but there are 
numerous problems here and there. Especially the second screen, which doesn't 
seem to work out of the box. If you run `xrandr` it will only recognize the 
*"default"* output and even that would look pretty much broken. First thing to 
do was:

``` sh
sudo apt-get install firmware-linux-nonfree
```

Actually, it's a go-to solution (as in first thing to try) to many Debian 
hardware-related problems, as the distro doesn't include non-free firmware by 
default. After that it recognizes most outputs the right way, but it may miss 
the right modes. If so, you could add your mode manually.

First, retrieve the full information about the mode:

``` sh
cvt 1920 1080
```

You will get something like that:

``` sh
# 1920x1080 59.96 Hz (CVT 2.07M9) hsync: 67.16 kHz; pclk: 173.00 MHz
Modeline "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
```

Now use the info to add the mode to `xrandr` and then assign it to the output:

``` sh
xrandr --newmode "1920x1080" 173.00  1920 2048 2248 2576  1080 1083 1088 1120 -HSync +VSync
xrandr --addmode VGA-0 "1920x1080"
```

Use the `xrandr` command to check the list of availible outputs and modes. You 
probably know that already, but here is how to use `xrandr` with the newly 
created mode:

``` sh
xrandr --output VGA-0 --mode "1920x1080"
```

Well, as this one issue is officially resolved I'm off to fight the rest of a 
couple hundred problems, that arise, when trying to use this laptop with 
Debian. Wish me luck and leave a comment if you had some trouble with the 
machine (when used with Debian that is) — we could try to work it out together.
