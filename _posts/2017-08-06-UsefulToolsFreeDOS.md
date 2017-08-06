---
layout: post
title: Useful Tools for FreeDOS
---

I thought as I find useful things to add to FreeDOS, I'd maintain a list of them here for other people to find.

## Fortran compiler

By default, FreeDOS has packages for the Open Watcom C Compiler but what about Fortran?  Well Open Watcom has a Fortran 77 compiler but it's not included in the distribution.  It is however available [straight from the source](http://www.openwatcom.org/download.php).  To install it run the installer. I chose to put it in `c:\devel\ow` where the C compiler is put by the FreeDOS package.  Be aware that at the end it will complain because you don't have a `config.sys` file (on FreeDOS it's called `fdconfig.sys`).  If you choose to let it add modifictions to your startup files, copy the additions from the new `config.sys` file to `fdconfig.sys`, otherwise delete it.

*Known good*: `open-watcom-f77-dos-1.9.exe` `3daf851a7916b6fd0d951af94b8c331975c6d522c393538fba05f3439cea8809`

## Scheme interpreter

I've been playing with LISP and wanted something LISPy to play with on FreeDOS.  To run LISP on DOS most of your options are abandonware and therefore legally iffy.  After a couple of days searching I stumbled across [Gambit](http://gambitscheme.org/wiki/index.php/Main_Page), a scheme implementation by Marc Feeley at the University of Montreal.  It's LGPL/Apache 2.0 licensed (your choice) and while version 4.x has dropped explicit DOS support, version 3.0 works fine and binaries are available to download.