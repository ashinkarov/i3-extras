i3-extras
=========

I am a big fan of [i3 window manager](http://www.i3wm.org/) -- it is fast, it
has a minimalistic design, and if you add a couple of simple tools and scripts it
really replaces complex desktop environments.  The main purpose of this
repository is to gather such a tools and share it with other i3 users.

Also, Michael has very strong opinions about the amount of features and
functions that i3 should have.  Well, that's his baby, however, I do not share
that strictness.  Having said that, I would also like to have a collection of
patches which work, but which will be never merged due to the ideological reasons.

Further down I will describe the extras that I am using right now, and I hope to
get some more stuff from you guys!

Icons in i3bar
==============

The file `i3bar-xbm-icons.patch` is a patch that adds a support for xbm icons to
the i3bar.  The patch adds two fields to the IPC protocol of i3bar which are: 
`icon` and `icon_color`. 

Here is a picture how it looks like:

![Image](https://github.com/ashinkarov/i3-extras/blob/master/pictures/xbm-icons.png?raw=true "Example of xbm icons")

Here is a [configuration file](https://github.com/ashinkarov/dotfiles/blob/master/i3/i3conky)
for [conky](http://conky.sourceforge.net/) in order to produce the picture
from above.

TODO
----
 * Xlib has a function to draw xbm, but it didn't work for me, so I am
   using something like putpixel to draw the pixels of the parsed image.
 * Caching.  Currently the pictures are parsed every time the status changes,
   which is every second or so.  It doesn't waste a lot of CPU, however a
   tiny hash-table would be much cleaner.

Nagbar replacement
==================

The file `i3-exit` is a python script that trows a simple menu in the middle of
the active workspace which allows you to chose your action on exit.  It uses 
[i3-py](https://github.com/ziberna/i3-py) to find out an active space and
geometry and it uses [dzen2](https://github.com/robm/dzen) to draw a menu.

Here is a picture of how it looks like:

![Image](https://github.com/ashinkarov/i3-extras/blob/master/pictures/i3-exit.png?raw=true "Example of i3-exit script")

TODO
----
 * Sometimes the keys does not go to the application.  Don't really know why.
   It might be a problem of dzen2 itself, it might be the case that i3
   switches the focus as the mouse cursor is not on the window.  Don't know.


Wrapper around i3lock
=====================

This is a very simple script that uses [i3lock](http://i3wm.org/i3lock/) and 
[imagemagick](http://www.imagemagick.org/script/index.php) to grab a
screenshot, blur it and launch i3lock with it.  The blurring constant is chosen
in such a way to hide the text, but leave the overall structure of windows etc.
If you don't know that it's a screensaver, the first instinct is to configure
your monitor.  Very nice.

Here is a picture of how it looks like:

![Image](https://github.com/ashinkarov/i3-extras/blob/master/pictures/i3lock-wrapper.png?raw=true "Example of i3lock wrapper")


