---
layout: post
title: Compiling in Gambit 3.0 on FreeDOS
---

In my [previous post](https://owainkenwayucl.github.io/2017/08/06/UsefulToolsFreeDOS.html) about FreeDOS tools I mentioned that I hadn't yet tried to get Gambit Scheme to compile `.scm` code to a stand-alone `.exe`.  There are a couple of things you need to do.

1. If you've not installed to `C:\GAMBC`, set the environment variable `%GAMBCDIR` ro point to where you installed it.  This allows `gsc` to find `_gambc.c` which it needs to compile scheme to c.

2. Make sure you have the DJGPP C/C++ compilers installed.

3. For some "program.scm":

 * `gsc -c program.scm` - this will create `program_.c` and `program.c`
 * `gcc -L%GAMBCDIR -I%GAMBCDIR program_.c program.c -o program.exe -lgambc`

Simple!
