---
layout: post
title: Useful Tools for FreeDOS
---

I thought as I find useful things to add to FreeDOS, I'd maintain a list of them here for other people to find.

## Fortran compiler

By default, FreeDOS has packages for the Open Watcom C Compiler but what about Fortran?  Well Open Watcom has a Fortran 77 compiler but it's not included in the distribution.  It is however available [straight from the source](http://www.openwatcom.org/download.php).  To install it run the installer. I chose to put it in `c:\devel\ow` where the C compiler is put by the FreeDOS package.  Be aware that at the end it will complain because you don't have a `config.sys` file (on FreeDOS it's called `fdconfig.sys`).  If you choose to let it add modifictions to your startup files, copy the additions from the new `config.sys` file to `fdconfig.sys`, otherwise delete it.

**Known good:** `open-watcom-f77-dos-1.9.exe` `3daf851a7916b6fd0d951af94b8c331975c6d522c393538fba05f3439cea8809`

## Scheme interpreter

I've been playing with LISP and wanted something LISPy to play with on FreeDOS.  To run LISP on DOS most of your options are abandonware and therefore legally iffy.  After a couple of days searching I stumbled across [Gambit](http://gambitscheme.org/wiki/index.php/Main_Page), a scheme implementation by Marc Feeley at the University of Montreal.  It's LGPL/Apache 2.0 licensed (your choice) and while version 4.x has dropped explicit DOS support, version 3.0 works fine and binaries are available to download.  Unpack the .zip file (I put it in `c:\devel\gambit`) and add that directory to your `%PATH%`.  You can then run it by typing `gsi`. The package also contains a scheme->c translator which works with DJGPP although I've not tried it with the packages from FreeDOS yet.

Like most interpreters on DOS, you can associate `.scm` files with it and run them like batch files.

```
set .scm=c:\devel\gambit\gsi.exe
```

**Known good:** `gc30-dj.zip 3120e5ddf8b4434fcc3c5f0b75f162d55d01eb649999298fe6f9ca82a8ec26bd` *[Virustotal](https://www.virustotal.com/en/file/3120e5ddf8b4434fcc3c5f0b75f162d55d01eb649999298fe6f9ca82a8ec26bd/analysis/1501671659/)*

## UCB Logo

Version 5.3 of the UCB Logo interpreter for DOS is available from [the developer's website](https://people.eecs.berkeley.edu/~bh/logo.html).  The double self-extracting .EXE/.ZIP file actually contains version 5.3 for 286 protected mode, and version 3.6 for older machines.  I found that 5.3 for 286 works fine on real hardware under FreeDOS but crashes on DOSBox.  It also includes a copy of the [JOVE text editor](https://en.wikipedia.org/wiki/JOVE), a bunch of examples and the source code, all in just over a megabyte meaning it fits on an HD 3.5" floppy.

You need to set a bunch of environment variables to get it to work outside of its own directory but these are explained in the README file.

**Known good:** `blogo.exe 5016ea9530e9645a67360945c3408083c01970a55ea83f5ee3c4691af70e426d` *[Virustotal](https://www.virustotal.com/#/file/5016ea9530e9645a67360945c3408083c01970a55ea83f5ee3c4691af70e426d/detection)*