---
layout: post
title: How to package your software to make our job easier
---

A significant part of the job of my team at UCL is to install and maintain user applications on UCL's centrally-owned clusters (Legion, Grace) and the new Thomas national materials modelling service.  A number of years ago, prodded by my colleague Dr Kirker, we embarked on a journey where the installation of all new applications is scripted (in bash, nothing fancy) so that subsequent deployments (either to other machines or upgraded packages) is as painless as possible.  You can see our work [over on Github](https://github.com/UCL-RITS/rcps-buildscripts), because transparancy is also something I think benefits university IT in general, not just "Research Software Development".

We have to install a lot of software.  Most of it is research software, some of it is commericial.  It's fair to say that research software, even software you pay a lot for, varies considerably in quality.  Instead of picking on bad practice though, I thought I'd rather attack this from the other end and suggest things you can do that make our lives easier as well as covering the don'ts.

## Documentation

Please put the installation instructions either a) on your website (best) so I can read them on my client separate from the HPC system I'm logged into over SSH or b) in a plain-text or markdown file within your distribution archive, so that I can read them in a text editor on the HPC system I'm logged into over an SSH (i.e. text only connection).  Please do not give us HTML files in the archive (although we can read them with Lynx, Links or emacs) and PDF files in the archive are the worst possible option.  There's no reason for your installation instructions to need PDF.  

Please clearly list packages you depend on, whether they are optional and if they need to be built with any unusual options.

## Distribution

Please make your software available on the web by a method which makes it http/https gettable with not too much fuss (if it's on Github this is all done for you if you use releases).  As part of our automated install scripts, we, by default, `wget` and then checksum the archive.  Things which make this impossible and therefore involve a lot of manual faffing about include things like having archives behind a login page which we *could* automate but we can't put authentication details into Github.

Please provide checksums for the files in md5, sha1 (both bad but better than nothing) or sha256, from a verifyable (i.e. https) source.  GPG keys are neat but importing them is a bit of a hassle.  Often I double check with some other packaging project that the checksums are correct where possible.

Please update the version of your package when you change *any* code in it.  Being able to state what version of a package a simulation was run with is important for reproducibility. Please do not remove old archives of your package from your website as it means we can't install old versions of software if a researcher needs it for some reason.

If it's a python package, why not put it in PyPi as that makes our lives incredibly easy?

## Build System

In general, please design your package so that it is intended to be installed in a different directory from where it was unpacked (i.e. not "in place").  This allows us to keep a tidy applications directory and also allows us to easily do the build in `/dev/shm` which is extremely fast.

There are no build systems which don't suck.  However, in our experience the ones that cause the least pain for us are (in order of my preference, this varies across the team).

### GNU Autotools/"The GNU Build system"

For people building packages this is the easiest system to build and automate.  For the vast majority of packages, you can get a working install with:

```bash
./configure --prefix=$INSTALL_PREFIX
make 
make install
```

Now I'm aware that there are a lot of problems with this system from the developer's side, but for us it's the easiest.

### Makefile with clearly named variables

This is the simplest system for you to implement and easy for us to set up and automate.  We know where things like FFTW and LAPACK are on our system so it's easy for us to write either a patch file or use a couple of in place `sed`s to automate configuring the `Makefile`.

### Cmake

Well this is controversial, and I'm even in two minds with this.  Cmake is a bit of a hassle.  It's a hassle to work out what the appropriate variables are (poking about in `ccmake` is a bit hit and miss), and many of Cmake's modules for detecting things are extremely poor, buggy and hard to override.  If you are an expert in Cmake, it's possible to produce a package that is a dream to build (although more annoying to automate than Autotools).  If you aren't and you mess it up, it can make your package into a disaster whose name we will curse for all eternity.

Cmake's only advantage over other tools is that it has a Windows GUI.  There are extremely few Windows HPC systems.

### Things not to do:

#### GUI Interactive installers

These can't be scripted and force me to re-login exporting X.  Please don't do this, or if you insist provide a non-gui option.

#### Installers that download lots of stuff off the internet or that contain half of a Linux distribution

Really this should be self-explanatory but since we are installing a lot of packages it's a waste of time, disk space and frankly (given build options are passed inconsistently to other packages in general) sanity to have a special version of FFTW/LAPACK/whatever for each project.  You can provide them as an option, but make it clear how to use the system ones.

The sole exception to this rule is Boost which is, itself, such a hassle to configure, install and maintain that I beg that you include the correct version automatically in your builds if you really must use it.

## License your software correctly

Licenses cause a lot of headache for us.  I strongly recommend that if you are giving your software away anyway you pick MIT, BSD or GPL licenses in preference for rolling your own as the latter sometimes requires me to contact the software purchasing team to scrutinise the terms.  You are not a lawyer, I am not a lawyer, let's leave drafting licenses to the lawyers.

Here are some licensing things that are a real pain:

### Group restrictions

Creating groups means creating a central UCL unix group and then adding people to that individually which can take a day (or more if we run into one of the hideous bugs in LDAP/AD).  It's even worse if you require a different group for each release of your software.

### "Limit the use of the software to people who have signed the license"

This is a horror as it combines the previous problem with a vast amount of paperwork and of course for each package the rules are slightly different.

### Not having a license at all

If you don't provide a license I can't install it for the researchers and they get grumpy at me, even though it's your fault.

## Automated tests

The members of my team are experts at linking Linux programs. We are not experts at the science case that your application is solving.  It's very hard for us to look at a printout of fractionally different floating point numbers and decide if that's a significant difference or not.

Please provide a simple to run test suite (`make check`?) that we can run to prove that our mix of libraries, compilers and CPU versions doesn't horribly break your package.  