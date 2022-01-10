---
title: "Password manager: pass"
date: 2016-03-29 17:39:35
category: misc
slug: pass
authors: Ryo KOBAYASHI
---

tags: linux, mac
Here is memorandom of how I set up password manager **pass**.

# Install pass

On MacOSX (El Capitan in my case), :

    $ brew install pass

will install `pass` command and others that the `pass` depends on.

# Prepare gpg

To generate PGP key using GnuPG `gpg`, :

    $ gpg --gen-key

You have to answer some questions to generate a pgp key.

Once you generate a pgp key, you can confirm it with `--list-keys`
option as, :

    $ gpg --list-keys
    /Users/kobayashi/.gnupg/pubring.gpg
    -----------------------------------
    pub   2048R/BFB474F7 2016-03-29
    uid                  Ryo KOBAYASHI <ryo.kbys@gmail.com>
    sub   2048R/2B3848F5 2016-03-29

As you can see, public key identifier is `BFB474F7`.

# Initialize pass

From here, you can just follow the instruction on the **pass** website
as,

    $ pass init BFB474F7
    $ pass git init
    $ pass insert hoge@gmail.com
    $ pass ...

Important thing here is that you have to specify the PGP key identifier
at initialization. Or you have to change pgp key priviledeg using `gpg`.
