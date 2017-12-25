---
layout: post
title: Humble Collection of i3wm Lifehacks: Part I
---

<h3>Lifehack 1: Locking the Screen</h3>

This is kinda a nobrainer, but me myself sometimes look for a place, where to copy some of the syntax (I'm lazy and don't always keep that in my head), so let's start with this one. i3wm ships with beautiful and robust screen locker <tt>i3lock</tt>, which can be launched like that:

``` bash
i3lock -c 000000
```

It will lock the screen with black overlay. The problem is, that you wouldn't be typing this command every time you want to lock the screen. We need to add a shortcut to i3 config file:

``` bash
bindsym $mod+Shift+Tab exec "i3lock -c 000000"
```

Now when we press the combination of mod-button (<kbd>Win</kbd> in my case) and <kbd>Shift</kbd>+<kbd>Tab</kbd> - our screen gets locked.

<h3>Lifehack 2: Activating/Disactivating the Second Screen</h3>

If you use i3wm on daily basis, you probably know, that the second screen is not turned on automatically. You should manage displays manually with <tt>xrandr</tt> command. If we run this command without attributes, we're gonna get something like this:

``` bash
Screen 0: minimum 320 x 200, current 3200 x 1080, maximum 8192 x 8192
LVDS1 connected 1280x800+1920+0 (normal left inverted right x axis y axis) 261mm x 163mm
   1280x800      60.02*+  50.05  
   1024x768      60.00  
   800x600       60.32    56.25  
   640x480       59.94  
VGA1 connected 1920x1080+0+0 (normal left inverted right x axis y axis) 521mm x 293mm
   1920x1080     60.00*+
   1680x1050     59.95  
   1280x1024     75.02    60.02  
   1440x900      59.89  
   1280x960      60.00  
   1280x720      59.97  
   1024x768      75.08    70.07    60.00  
   832x624       74.55  
   800x600       72.19    75.00    60.32    56.25  
   640x480       75.00    72.81    66.67    60.00  
   720x400       70.08  
HDMI1 disconnected (normal left inverted right x axis y axis)
DP1 disconnected (normal left inverted right x axis y axis)
```

On some rare occasion writing something like this would not be a problem:

``` bash
xrandr --output VGA1 --right-of LVDS1 --auto
```

<em>Well, I know you're probably well aware of how</em> <tt>xrandr</tt> <em>works. Just in case.</em>

It may get pretty redundant if you use second screen on daily basis and regularly unplug it from your laptop. The best way to go would be to add script shortcut to your <tt>/usr/bin/</tt> or <tt>/bin/</tt> directory. Run the following lines:

``` bash
printf '#!/bin/bash\n\nxrandr --output VGA1 --right-of LVDS1 --auto' > /usr/bin/screenswitch
chmod 755 /usr/bin/screenswitch
```

We can use <tt>screenswitch</tt> command which doesn't make it much easier. What would certainly help us is a key shortcut, so let's add a line similar to one in the previous hack to our i3 configuration:

``` bash
bindsym XF86Display exec "screenswitch"
```

Now when you press on your special key combo (<kbd>Fn</kbd>+<kbd>F7</kbd> on my ThinkPad), you enable/disable the second screen. Try some other key combination if you have no special display button. Of course the script itself is pretty basic and it would work only if your screen works well in auto and you use the same second screen daily. However, there are more complex scripts available all over the web (<a href="http://www.thinkwiki.org/wiki/Sample_Fn-F7_script">example</a>).

<h3>Lifehack 3: Locking the Screen on Wake</h3>

If you use <tt>pm-utils</tt> with your i3wm setup, you've probably noticed that the screen is not locked, when the laptop is awakened after suspend or hibernate. It's very insecure. Let's try to fix it. Create file <tt>cat /etc/pm/sleep.d/91blocker</tt> and add the following lines to it:

``` bash
#!/bin/sh
case "$1" in
        thaw|resume)
                su youruser -c '/usr/bin/i3lock -c 000000'
                ;;
        *) exit $NA
                ;;
esac
```

Don't forget to change <tt>youruser</tt> to your username. Now let's make sure we have all the right permissions:

```
chmod 755 /etc/pm/sleep.d/91blocker
```

Now, if we run <tt>pm-suspend</tt> or <tt>pm-hibernate</tt> our screen is going to be locked on wake. This script has one shortcoming though: it doesn't lock the screen instantly, so you may see stuff for a couple of seconds before it gets locked. If it is not a critical issue to you, feel free to use it, othrewise you may need to work on it or find a different solution altogether. If you have any ideas how to improve it, let me know.