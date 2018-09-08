---
layout: post
title: Horrors of Matlab
---

In a way up until now, my life has been somewhat blessed; namely I have never had to understand Matlab for work.  I started out on actual programming languages and stayed there.  I have had to install and test it for other people though.  And I did, in a fit of masochism, do the ubiquitous pi example for Matlab and its achingly slower Open Source semi-compatible cousin Octave.

I've been helping out a couple of final year students with advice for an end of year project which has involved them replicating some methods in a paper that (in good Open Science style) come with a tarball of scripts for replicating the analysis steps in Octave.  We didn't (at the time) have Octave installed on our clusters because we pay Mathworks a lot of money for Matlab licenses and no-one has ever actually asked us for Octave.

It's OK, though because we should be able to run the scripts on Matlab, right?

Well actually no.

It turns out that the Octave developers added functionality over Matlab (good!) but some of this stuff is things like [fputs](http://octave.org/doc/v4.0.0/Simple-Output.html).

It was at this point that I realised I needed to understand a bit more about these two stalwarts of mathematical computation in order to make sure that the students had the tools available that they needed.

False start: Giving Matlab fputs
--------------------------------

`fputs` is actually a relatively simple function - you give it a file handler and a string and it writes it to the file handler.  This is not a lot of code, but implementing it, we learn a few things about Matlab that are kind of horrifying to normal programmers, namely how you define and import functions.  In Matlab, you define one function per file, in a file with the name of the function and `.m` appended.  When Matlab comes across a function call it doesn't have defined, it searches down a search path for `function_name.m` and if it finds one, it runs it.  

![octave loading a function from disk](/images/octave_function0.png)
![octave loading a function from disk](/images/octave_function1.png)

The implications of this are kinda terrifying, but it does mean that for our purposes we can just put an `fputs.m` in the same directory as we run Matlab in and as long as it does the right thing, it should work.

Based on the documentation above, I wrote the following:

```matlab
function fputs(fid, line)
   fprintf(fid, '%s', line);
end

```

It turns out that this is not quite enough because sometimes `line` is not a string and Matlab pukes.  Which brings us to ask, what does octave do if you `fputs` something that's not a string?  Normal programmers might expect it to do one of two things; either convert the variable to a string and print it, or throw an error.  It turns out that neither of these two options is correct; what Octave does is silently drop it and do nothing.  Again, there are probably some people in the back screaming internally, but this is the world we live in.  This means that we need to slightly modify our `fputs.m` to replicate this interesting behaviour.

```matlab
function fputs(fid, line)
  if isstring(line)
    fprintf(fid, '%s', line);
  end
end
```

This of course works perfectly.  If you want to save typing this all in, you can find my `fputs.m` over in my utils repo [here](https://github.com/owainkenwayucl/utils/blob/master/src/matlab-octave/fputs.m).

As an aside, Matlab/Octave checks to see if the function has been modified each time you call it so you can run it, change the file and run it again and it will use the new behaviour.

OK, fine, installing Octave
---------------------------

There were other issues, which appeared to be Matlab seg-faulting on the code (it wasn't, there was a mess of inter-related error messages) so I thought I should build Octave on the clusters to eliminate Matlab from a the list of possible sources of error.

This was a lot more challenging that I had hoped.  It turns out that the dependencies list for Octave is extremely long and it's not even clear which ones are optional.
  You can find the script I ended up with [here](https://github.com/UCL-RITS/rcps-buildscripts/blob/master/octave-4.4.1_install) after many false starts.

You defined that how??
----------------------

Of course the problem wasn't that we were using Matlab rather than Octave; it turned out that the problem was multiple layers of stuff with the top-most error being it running over an array index out of bounds.  Debugging this was hard, because the array bounds seemed to come from nowhere.

I say out of nowhere, because it was coming out of somewhere.  After asking the students, it turns out that one way you can define and set variables from a file is to `load()` a file and the variable with the same name as the file (- the `.txt`) is loaded with the contents of the file.

["I now want to know what happens if the file name contains white space."](https://twitter.com/eddedmondson/status/1038035734895226880), asked Ed on twitter.  So did I so I stumbled out of my sick-bed, got my laptop, loaded octave and had a look.  It's easy really, it replaces the spaces with `_`s.

![screenshot of octave loading a file with spaces in the name](/images/octave_space.png)


"You can do open science in Matlab, people can replicate it in Octave"
----------------------------------------------------------------------

No.

![Performance comparison](https://pbs.twimg.com/media/DmPqZ7fXgAEYxo4.png)

(Matlab 0.2 seconds, Octave 475 seconds)

Notionally there's an optional JIT you can compile into Octave but it is experimental and it only works with a specific version of LLVM which is not documented anywhere.

Where do we go from here?
-------------------------

Well I'm sure none of these things are particularly surprising to even short term users of Matlab but they are pretty horrifying to people who use other programming languages.  Other people have found worse things, for example, because of the way Matlab types work, well [this horror](https://twitter.com/augeas/status/1017862450169905153).  I'm not going to go as far as other people and say that you should never use Matlab; I'm just not familiar with it enough beyond intallation and licensing complexities; but what little I've seen is really bad.  If you use it you should realise that it is training you to write code in a way that is if not wrong then at best highly unusual, hard to read and extremely non-portable.

Don't believe anyone who tells you to [use Jupyter either](https://docs.google.com/presentation/d/1n2RlMdmv1p25Xy5thJUhkKGvjtV-dkAIsUXP-AL4ffI/edit).
