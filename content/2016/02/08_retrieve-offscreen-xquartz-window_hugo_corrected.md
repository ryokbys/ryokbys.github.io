---
title: Retrieve offscreen XQuartz windows
date: 2016-02-08 11:21:49
category: misc
slug: retrieve-offscreen-xquartz-window
authors: Ryo KOBAYASHI
---

tags: mac
Sometimes XQuartz windows go somewhere off the screen if you are using
multiple monitors.

To bring back the window, you may be able to use `wmctrl` utility which
can be installed as, :

    $ brew install wmctrl

And you can see XQuartz window list as, :

    $ wmctrl  -G -l
    0x00600008  0 0    44   200  200  N/A Gnuplot

It seems the window I am now looking for is at the position (0,0) with
width and height (200,200), but it is not seen on the screen. Try
something like, :

    $ wmctrl -i -r 0x00600008 -e 0,200,2000,200,200

which means specify windows in number `-i -r 0x00600008`, and specify
the position (x,y), and width and height (w,h) as `-e 0,x,y,w,h`.

It seems that the position (x,y) is not correctly pointing the up and
left corner, so we need to specify some large values for (x,y), in this
case (200,2000), but I don\'t know what is the apropriate values for the
coordinate, though. At least, I could bring back my window from outside
the screen.
