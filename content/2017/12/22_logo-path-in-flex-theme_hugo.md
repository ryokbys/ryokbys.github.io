---
title: "Logo path in Flex theme"
Date: 2017-12-22T16:24:39+09:00
Category: misc
Slug: logo-path-in-flex-theme
Authors: Ryo KOBAYASHI
tags: 
- "pelican"
---

Currently this site uses the pelican theme [Flex](https://github.com/alexandrevicenzi/Flex), and I like it.
But one thing annoyed me. There is a logo image on the left side and it works fine with the home, archive, and tag pages. But it does not show up on post pages.
The reason is that the path to the logo image is set in `pelicanconf.py` and it seems to be relative URL from the current content which is dependent on the article.
So setting
```
SITELOGO = "images/logo.png"
```
does not work for post pages.
But I do not know how to set `SITEURL` into it.

So I modified directory `pelican-themes/Flex/templates/base.html` file, line 81,
from
```
        {% if SITELOGO %}
        <img src="{{ SITELOGO }}" alt="{{ SITETITLE }}" title="{{ SITETITLE }}">
        {% else %}
```
to
```
        {% if SITELOGO %}
        <img src="{{ SITEURL }}/{{ SITELOGO }}" alt="{{ SITETITLE }}" title="{{ SITETITLE }}">
        {% else %}
```
I added `{{ SITEURL }}/` before `{{ SITELOGO }}` to make the `SITELOGO` path as a relative path from `SITEURL`.

Then it works fine even at post pages.

But after struggling on this for hours, it turns out that just setting
```
SITELOGO = "/images/logo.png"
```
solves the issue here... What a waste of time...


