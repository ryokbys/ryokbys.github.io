---
title: Figures on pelican post
date: 2016-02-28 11:36:29
category: misc
slug: figures-on-pelican-post
authors: Ryo KOBAYASHI
---

tags: pelican
In my case, the images used in the blog post are all uploaded on to
**flickr** and loaded from **flickr** to show in the post.

# Upload the images on flickr

1.  Fist you have to have an account on **flickr**, of course.

2.  Upload a image to be included in a post.

3.  Get the image URL from **flickr**.

    ![](https://farm2.staticflickr.com/1672/25042496440_5e9955456b_o.png){width="400px"}

    Pressing `share photo` button or image, from *Embed* tab as shown in
    the above picture you can get the embedding HTML source code. The
    image URL is included in the HTML source code.

Then you are ready to use the image on your blog post.

# Load from pelican blog

I have already written some posts including images. But since it is kind
of rare to include images in a post, I always forget how to include
images on the posts.

Here I write how to put images on a pelican post. :

    .. figure:: https://farm2.staticflickr.com/1482/25313177185_35009373ae_o.png
       :target: https://farm2.staticflickr.com/1482/25313177185_35009373ae_o.png
       :width: 400px
       :alt: jupyter notebook on Emacs.app

![](https://farm2.staticflickr.com/1482/25313177185_35009373ae_o.png){width="400px"}

This way also works for animation gif files, too.

Reference

-   [documents about
    reStructuredText](http://docutils.sourceforge.net/docs/ref/rst/directives.html#figure)
