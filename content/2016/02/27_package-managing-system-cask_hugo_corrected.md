---
title: Package managing system Cask
date: 2016-02-27 20:57:10
category: misc
slug: package-managing-system-cask
authors: Ryo KOBAYASHI
---

tags: emacs
# What is Cask?

Usually, without cask, we need to download and install packages manually
and write some configurations in `.emacs.d/init.el` to make them work.
Cask makes it much easier.

[cask documentation](http://cask.readthedocs.org/en/latest/)

It seems to require the following conditions,

-   emacs \>= 24
-   python \>= 2.6

## Install

    $ brew install cask

It seems different from :

    $ pip install cask

## Setup

    $ cd ~/.emacs.d
    $ cask init
    $ ls
    Cask
    $ head Cask
    (source gnu)
    (source melpa)

    (depends-on "bind-key")
    (depends-on "cask")
    (depends-on "dash")
    (depends-on "drag-stuff")
    (depends-on "exec-path-from-shell")
    (depends-on "expand-region")
    (depends-on "f")

You can modify this Cask file to manage which packages to be installed?
Every time you modify the `Cask` file, you should perform the following
command to reflect the change, :

    $ cask install

Important cask dependencies for python usage in Emacs are,

-   elpy: extended python mode
-   ein: ipython notebook integration

## Setup in init.el

After installing packages via cask, cask should be loaded by Emacs. To
do so, the following lines should be written in `init.el`. :

    (require ‘cask)
    (cask-initialize)

In case of MacOSX, this works. But I am not sure whether it works in
Linux, too. You may have to specify cask elisp file as :

    (require ‘cask ‘~/.emacs.d/.cask/cask.el')

## In case of using different versions of emacs

In case you want to use Emacs.app and the version of Emacs.app is
different from that of emacs in Terminal.app. You have to specify the
emacs binary as :

    $ EMACS="/Applications/Emacs.app/Contents/MacOS/Emacs" cask install

Or if you want to use emacs in Terminal.app, :

    $ EMACS=“/usr/local/bin/emacs” cask install
