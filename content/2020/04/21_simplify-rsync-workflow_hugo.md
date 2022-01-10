---
title: "Simplify rsync workflow"
Date: 2020-04-21T12:59:40+09:00
Category: misc
Slug: simply-rsync-workflow
Authors: Ryo KOBAYASHI
tags: 
- "linux"
- " mac"
---

If you are using remote computers on a daily basis, you probably use `rsync` command for transfering data
between the local machine and remote machines. And you may need to type remote directory every time you
use `rsync` command, which is a small effort for a single case but it costs a lot if you do the smae thing many times.
The script [rsync_wrapper.py](https://github.com/ryokbys/rsync_wrapper) is a helper script to avoid
typing remote directory every time you use `rsync`.

## Before using `rsync_wrapper.py`...

You can see at its [github page](https://github.com/ryokbys/rsync_wrapper) how to use.
But before using the script, I strongly recommend that you make the paths of working directories on the local machine
and remote machines identical so that you don't need to specify the corresponding directory on remote machines.
For example, if the current working directory on the local machine is `/Users/kobayashi/work/2020-04-01/` and the home directory is `/Users/kobayashi/`,
the corresponding directory on remote machines should be `~/work/2020-04-01/`.

## Further tips

You can sync (upload/download) files and directories in the current working directory between the local and remote machines,
by the following command:
```bash
$ /path/to/rsync_wrapper.py up -r remote
```
If you make an alias to `/path/to/rsync_wrapper.py up` as `upsync`, you can do the same by,
```bash
$ upsync -r remote
```
And from the second time, you can do by just
```bash
$ upsync
```
since the first run of `rsync_wrapper.py` will make a configuration file `.sync` in the directory
and the script knows which remote host is used for the sync.

I think it is very helpful for some people, don't you think?
