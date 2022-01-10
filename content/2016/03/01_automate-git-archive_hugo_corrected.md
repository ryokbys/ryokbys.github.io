---
title: Automate git archive
date: 2016-03-01 18:08:05
category: misc
slug: automate-git-archive
authors: Ryo KOBAYASHI
---

tags: linux, git
I made a bash function to replace `git archive` command, :

    function gitarch(){
        basedname=$(git rev-parse --show-toplevel)
        dname=$(basename $basedname)
        cwd=$(pwd)
        cd $basedname
        fname=${dname}_`ymd`.tgz
        git archive --format=tar --prefix=${dname}/ HEAD | gzip > $fname
        cp $fname ${cwd}/
        cd $cwd
    }

Writing this function in `.bashrc` file, I can create git archive with
name `projectname_date.tgz` a little bit easier.

BTW, by typing :

    $ type gitarch

you can see what is this function doing.
