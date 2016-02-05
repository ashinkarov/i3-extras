i3-desktop.patch
=================

This is a small patch for the i3 window manager to support the use of a
desktop window (with wallpaper and icons) created by a file manager like
Nautilus, PCManFM and others.

How to use
===========
* Download and unzip the i3 source code (this patch is made for version
  4.11, can't say if/when newer versions will break it).
* Apply the patch:

  ```shell
  $ patch -p1 < /path/to/i3-desktop.patch
  ```
* Compile, install and restart i3
* Launch a desktop manager, e.g.

  ```shell
  $ pcmanfm --desktop
  ```
  or
  ```shell
  $ nautilus
  ```
  (nautilus should start the desktop manager automatically, unless you have
  previously disabled it).
* Go to an empty workspace, you should see the icons of your ~/Desktop
  folder on the desktop. If you open any tiling window it should hide
  the desktop.

Todo
====
* Test what happens on a dual monitor setup.
* The code does not handle the case where the window desktop is closed if
  e.g. it crashes or the user kills it. Nothing really bad should happen,
  but it should be handled properly.
* It looks like the height for the i3bar is not subtracted from the desktop
  window (so, i3bar may partially cover some elements).
* More desktop managers should be tested.

Contacts
=========
You can send bug reports, comments, suggestions, improvements to:
  alessandro.g89@gmail.com
