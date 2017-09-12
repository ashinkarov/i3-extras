# i3-extras #


I am a big fan of [i3 window manager](http://www.i3wm.org/) -- it is fast, it
has a minimalistic design, and if you add a couple of simple tools and scripts it
really replaces complex desktop environments.  The main purpose of this
repository is to gather such tools and share it with other i3 users.

Also, Michael has very strong opinions about the amount of features and
functions that i3 should have.  Well, that's his baby, however, I do not share
that strictness.  Having said that, I would also like to have a collection of
patches which work, but which will be never merged due to ideological reasons.

Further down I will describe the extras that I am using right now, and I hope to
get some more stuff from you guys!

## Table of contents ##

- [Patches](#user-content-patches)
   - [Title of the focused window](#user-content-title-of-the-focused-window)
   - [Icons in i3bar](#user-content-icons-in-i3bar)
   - [Higher optimisation level](#user-content-higher-optimisation-level)
   - [Patches for size, borders, cwd](#user-content-patches-for-size-borders-cwd)
   - [Window icons](#user-content-window-icons)
   - [Desktop window](#user-content-desktop-window)
- [dmenu replacements](#user-content-dmenu-replacements)
- [Nagbar replacements](#user-content-nagbar-replacements)
   - [Nagbar replacement 1](#user-content-nagbar-replacement-i3-exit-1)
   - [Nagbar replacement 2](#user-content-nagbar-replacement-i3-exit-2)
- [`i3lock` related](#user-content-i3lock-related)
   - [Wrapper around i3lock](#user-content-wrapper-around-i3lock)
   - [Centered image in i3lock](#user-content-centered-image-in-i3lock)
- [Windows related](#user-content-windows-related)
   - [Switching windows](#user-content-switching-windows)
   - [Closing windows](#user-content-closing-windows)
- [Workspace related](#user-content-workspace-related)
- [Status bar related](#user-content-status-bar-related)
   - [`wsbar` replacement](#user-content-wsbar-replacement)
   - [Scripts for i3bar/i3status](#user-content-scripts-for-i3bari3status)
   - [Status for i3+dzen2 [1]](#user-content-status-for-i3dzen2-1)
   - [Status for i3+dzen2 [2]](#user-content-status-for-i3dzen2-2)
   - [i3blocks](#user-content-i3blocks)
   - [lynks--](#user-content-lynks--)
   - [polybar](#user-content-polybar)
- [Vim syntax for i3 config](#user-content-vim-syntax-for-i3-config)
- [Scratchpad manager](#scratchpad-manager)

## Patches ##

### Title of the focused window ###

This is an
[ancient patch from Kevin Murphy](http://infra.in.zekjur.net/pipermail/i3-discuss/2012-November/001040.html)
that was never accepted to the main line.  I didn't had a chance to test if it
still works, but I like the idea.


### Icons in i3bar ###

The file `i3bar-xbm-icons.patch` is a patch that adds a support for xbm icons to
the i3bar.  The patch adds two fields to the IPC protocol of i3bar which are:
`icon` and `icon_color`.

Here is a picture how it looks like:

![Image](https://github.com/ashinkarov/i3-extras/blob/master/pictures/xbm-icons.png?raw=true "Example of xbm icons")

Here is a [configuration file](https://github.com/ashinkarov/dotfiles/blob/master/i3/i3conky)
for [conky](http://conky.sourceforge.net/) in order to produce the picture
from above.

#### TODO ####
 * Xlib has a function to draw xbm, but it didn't work for me, so I am
   using something like putpixel to draw the pixels of the parsed image.
 * Caching.  Currently the pictures are parsed every time the status changes,
   which is every second or so.  It doesn't waste a lot of CPU, however a
   tiny hash-table would be much cleaner.

### Higher optimisation level ###


The `makefile-gcc-opt.patch` increases default optimisation level.  This patch
assumes that you are going to use GCC compiler, however it should work with
`icc -gcc` and with `clang` as well.

### Patches for size, borders, cwd ###

0x2493 kept a set of small patches to customise
borders, i3bar and window header sizes and use extract cwd from focused window.
He had a repository on github and later moved it on bitbucket and then deleted
it all, so please find some of his patches stored [locally](https://github.com/ashinkarov/i3-extras/tree/master/0x2493-patches):

 * [`cwd-exec`](https://github.com/ashinkarov/i3-extras/blob/master/0x2493-patches/cwd-exec.patch)
   extracts current working directory from the focused window before spawning
   the child.  That would allow to have smarter `Mod + return`, which may open a
   new terminal with cwd taken from the old one.

 * [`smart-border`](https://github.com/ashinkarov/i3-extras/blob/master/0x2493-patches/smart-border.patch)
   doesn't draw a border around the window it is the only window on a workspace.

 * [`tiny-bar`](https://github.com/ashinkarov/i3-extras/blob/master/0x2493-patches/tiny-bar.diff)
   removes extra spaces around the font in i3bar, which makes the i3bar tinier.

 * [`tiny-titles`](https://github.com/ashinkarov/i3-extras/blob/master/0x2493-patches/tiny-titles.patch)
   removes extra spaces around the font in window titles.


### Window icons ###
[Maris Muja](https://github.com/mariusmuja) wrote a [patch](https://raw.githubusercontent.com/ashinkarov/i3-extras/master/window-icons/window-icons.patch)
to draw an application icon in the left corner of the window bar.  _Please have a look at his
[i3wm repository](https://github.com/mariusmuja/i3wm/commits/next), you can find some
more patches there._

### Desktop window ###
[alessandro-g89](https://github.com/alessandro-g89) has created a
[patch](https://github.com/ashinkarov/i3-extras/blob/master/i3-desktop-patch/i3-desktop.patch)
that makes it possible to use desktop windows (with wallpaper and icons) created
by file managers like Natutilus, PCManFMM and others.  Please find some brief
instructions [here](https://github.com/ashinkarov/i3-extras/blob/master/i3-desktop-patch/README.md)
on how to use the patch as well as suggestions for further improvements.

## [dmenu](http://tools.suckless.org/dmenu/) replacements ##
Sean Pringle's [rofi](https://davedavenport.github.io/rofi/) is a generic very powerful
[dmenu](http://tools.suckless.org/dmenu/)-like tool that can be used for all kinds of
custom launchers, e.g. window switching, running commands, ssh sessions, etc.
It is fully configurable and well-maintained.

![Image](https://davedavenport.github.io/rofi/images/rofi/window-list.png)

Also, consider [these two rofi-related scripts](https://github.com/okraits/rofi-tools)
to apply colorscheme from i3 config file to rofi, and to create a shutdown menu that
can be used as a replacement for nagbar.


## Nagbar replacements ##

### Nagbar replacement (i3-exit) [1] ###

The file `i3-exit` is a python script that throws a simple menu in the middle of
the active workspace which allows you to chose your action on exit.  It uses
[i3-py](https://github.com/ziberna/i3-py) to find out an active space and
geometry and it uses [dzen2](https://github.com/robm/dzen) to draw a menu.

Here is a picture of how it looks like:

![Image](https://github.com/ashinkarov/i3-extras/blob/master/pictures/i3-exit.png?raw=true "Example of i3-exit script")

#### TODO ####
 * Sometimes the keys does not go to the application.  Don't really know why.
   It might be a problem of dzen2 itself, it might be the case that i3
   switches the focus as the mouse cursor is not on the window.  Don't know.

### Nagbar replacement (i3-exit) [2] ###

Here is [another version of i3-exit](http://github.com/uranix/i3-settings/blob/master/i3-exit)
created by [Ivan Tsybulin](https://github.com/uranix).  He uses `pygtk` with
buttons and have much more sophisticated choices than I do.

## `i3lock` related ##

### Wrapper around i3lock ###

The `i3lock-wrapper` is a very simple shell script that uses [i3lock](http://i3wm.org/i3lock/) and
[imagemagick](http://www.imagemagick.org/script/index.php) to grab a
screenshot, blur it and launch i3lock with it.  The blurring constant is chosen
in such a way to hide the text, but leave the overall structure of windows etc.
If you don't know that it's a screensaver, the first instinct is to configure
your monitor.  Very nice.

Here is a picture of how it looks like:

![Image](https://github.com/ashinkarov/i3-extras/blob/master/pictures/i3lock-wrapper.png?raw=true "Example of i3lock wrapper")

For Arch Linux users here is an [AUR](https://aur.archlinux.org/packages/i3lock-wrapper/) created by Thiago Perrotta.

### Centered image in i3lock ###

[Ivan Tsybulin](https://github.com/uranix) have created a
[patch](https://github.com/uranix/i3-settings/blob/master/centered_image.patch)
for i3lock that allows to centre the image of the size smaller than the screen
resolution.

## Windows related ##
### Switching windows ###

There are two scripts doing a very similar task -- list all the windows, pipe it
through `dmenu`, focus the selected one.  One implementation can be found in
[Jure Ziberna i3-py/examples](https://github.com/ziberna/i3-py/tree/master/examples/)
repository.  The script is called `winmenu.py`.

Another implementation can be found in
[quickswitch-for-i3](https://github.com/OliverUv/quickswitch-for-i3)
repository.

#### TODO ####
 * Unicode characters are not rendered properly.  Probably it has something to do
   with the encoding that `dmenu` expects.

### Closing windows ###

[AladW](https://github.com/AladW) have created a [patch](https://raw.githubusercontent.com/ashinkarov/i3-extras/master/i3-mouse-close.patch)
that allows you to close a window via the middle mouse click.

## Workspace related ##

[Cameron Leger](https://github.com/cameronleger) has created a
[set of scripts](https://github.com/cameronleger/i3-workspacer) that make it possible
to:
  * rename/renumber workspaces
  * move workspaces
  * go to a workspace

The scripts can be bound to the keys of your choice which gives a convenient interface
to i3 features that otherwise are available only via i3-input.


## Status bar related ##

### `wsbar` replacement ###

In [Jure Ziberna i3-py/examples](https://github.com/ziberna/i3-py/tree/master/examples/)
you can find a script called `wsbar.py` which replaces an original `wsbar` from the i3 repository.
The main difference -- this script is written in Python.

### Scripts for i3bar/i3status ###

[enkore](https://github.com/enkore) is maintaining a python framework called [i3pystatus](https://github.com/enkore/i3pystatus) that allows configure an output that can be
piped into i3bar.  You can do a very similar thing using conky, however here the tool is tailor-made
for i3bar.  Its modular design allows to contribute functionality in the form of modules which can be
imported later in the output-generator.

[py3status](https://github.com/ultrabug/py3status) has a similiar name, but uses a different
approach: It wraps i3status and can add additional functionality through modules. It is maintained
by [ultrabug](https://github.com/ultrabug).

### Status for i3+dzen2 [1] ###

[gpix13](https://github.com/gpix13) Wrote a shell script to generate the status bar.  He uses
`dzen` to display it.  Here is a screenshot of what the bar produced by status.sh looks like:

![ScreenShot](https://raw.github.com/gpix13/i3/master/bar_screenshot.png)

#### TODO ####
 * Here is a general problem with `dzen` -- it doesn't have a support for
   tray.  So the alternatives are either to show dzen and i3bar, which is
   a waste of space, or using something else for tray.
 * Most of the output here, can be obtained from `conky`.  Wouldn't it be
   faster?


### Status for i3+dzen2 [2] ###

[Wang Chao](https://github.com/yueyoum) has another implementation of the status
information piped through dzen. Her is an
[implementation](https://github.com/yueyoum/i3-dzen2-bridge).  He uses
`i3status` to generate the information and the he uses a custom python script
that parse the output of `i3status`, appends it with icons and does some extra
massaging.  Finally dzen is displaying the output.  Here is how it looks like:

![i3-dzen-bridge-1](http://i1297.photobucket.com/albums/ag23/yueyoum/dzen2-s_zps6c50c408.png)

He also has a very nice icons which you can hopefully steal from
[here](https://github.com/yueyoum/i3-dzen2-bridge/tree/master/xbm-icons) :)


### i3blocks ###

[Vivien Didelot](https://github.com/vivien) maintains the
[i3blocks project](https://github.com/vivien/i3blocks)
i3blocks is a highly flexible status line for the i3 window manager. It handles
clicks, signals and language-agnostic user scripts.  The content of each block
(e.g. time, battery status, network state, ...) is the output of a command provided
by the user. Blocks are updated on click, at a given interval of time or on a
given signal, also specified by the user.  It aims to respect the i3bar protocol,
providing customization such as text alignment, urgency, color, and more.  See their
[wiki](https://github.com/vivien/i3blocks/wiki) for more details.

### lynks-- ###

[lynks--](https://github.com/lynks--) is maintaining an i3bar replacement called [lifebar](https://github.com/lynks--/lifebar).  This is a i3bar-like panel combined with
functionality of i3statys.  It has transparency by default, configuration is set via config file.
The project in its early stages but is being actively developed.  Here is a default
liefbar screenshot:

![Image](https://github.com/ashinkarov/i3-extras/blob/master/pictures/lifebar.png?raw=true "Lifebar screenshot")

### polybar ###
[Michael Carlberg](https://github.com/jaagr) created a tool called
[polybar](https://github.com/jaagr/polybar) for creating custom status bars.  It comes with a set
of modules that can be turned on and off in the config file to customize the look of a status bar,
similarly to conky.  The status bar is very configurable and a number of custom user modules are
available.  Note that one can always pipe a custom command in the bar.  The tool and modules are
written in C++.

![Image](https://u.teknik.io/x32YI.png)


## Vim syntax for i3 config ##

[Emanuel Guével](https://github.com/PotatoesMaster) wrote a syntax file for i3
config.  Thanks for that, the repository can be found
[here](https://github.com/PotatoesMaster/i3-vim-syntax).

In order to enable it you can add `# vim:filetype=i3` as a comment to your file,
or you can do it manually.  Another option is to recognise the path to the i3
config and write an autocmd: `autocmd BufEnter *i3/config  setlocal filetype=i3`

## Scratchpad manager ##

[Mustafa Gezen](https://github.com/mstg) have created a manager for i3 scratchpad
called [zx](https://github.com/mstg/zx).  The manager makes it possible to track
the windows that are currently in the scratchpad. 


