---
title: Pelican blog post script
date: 2016-02-28 22:43:34
category: misc
slug: pelican-blog-post-script
authors: Ryo KOBAYASHI
---

tags: python, pelican
I updated my blog automation script. This version make blog posting
process much more efficient and I am much happier than before.

This version is uploaded on Gist.

-   [gist
    ryokbys/myblog.py](https://gist.github.com/ryokbys/d4f343cdbb2f5d33c305)

Now I can create a new blog post by typing, :

    $ blogpost 'BLOG TITLE'

then a blog-post file is created at the apropriate place in the system
with the date and title in the filename. And also open it with
*Emacs.app*.

After writing the post, just type :

    $ blogmake

will update the blog post.

If this `blogmake` command should be called from *Emacs.app*, it is
necesarry that the following lines should be written in
`~/.emacs.d/init.el`. :

    (setq shell-file-name "bash")
    (setq shell-command-switch "-ic")

in order to let emacs know that the interactive shell (-i option) is
bash with aliases in `.bashrc` file.
