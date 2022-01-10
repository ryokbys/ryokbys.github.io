---
title: "Markdown on Pelican"
Date: 2016-12-16T10:10:23+09:00
Category: misc
Slug: markdown-on-pelican
Authors: Ryo KOBAYASHI
tags: 
- "pelican"
---


Since I got to know that Markdown is available in pelican, I tried to use Markdown.

[TOC]

## Markdown setting

To activate the Markdown, I wrote the following line in `pelicanconf.py`.

```
MD_EXTENSIONS = ['codehilite','extra','smarty', 'toc']
```

And to make `toc` to be available in Markdown I added `'extract_toc'` as,

```
PLUGINS = ["tag_cloud","render_math",'extract_toc']
```

These are what I did. It is easy.


## Test

This post is written in Markdown, and now I am going to see whether it works or not...

## Changes from reST format

* Preambles are different as

        ---
title: "Markdown on Pelican"
        Date: 2016-12-16T10:10:23+09:00
        Category: misc
        Slug: markdown-on-pelican
        Authors: Ryo KOBAYASHI
        Tags: pelican
---

* Above preambles should start at the begining of the text. There should not be any black lines before that.


## About Markdown format

* Code in a list is tricky. Usually code block is achieved by enclosing three inverted quotation marks (I don't know how to call it). 
  But in case in a list, it doesn't seem working. Instead one should put code with 8 white spaces enclosed by blank lines.
  This is completely counter-intuitive...


