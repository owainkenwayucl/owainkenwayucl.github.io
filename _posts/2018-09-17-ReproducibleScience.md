---
layout: post
title: Thoughts on Reproducible Science
---

I'm afraid this is one of those posts which are at least partly deliberately contrarian.  I hope this will provoke people to think differently about how they do "open science", about what they share and how.  In a way this is an expansion of some comments I've made previously about open science on Twitter and I might seem flippant but I'm slightly concerned we (computational scientists) have taken a wrong turn, based on a confusion of a number of similar ideas.  Luckily, it's easily resolved.

## Reproducibility

It has for some years now been the received wisdom that when you publish a paper you should publish the code you used to generate your results.  All the code, no matter the state.  And indeed there are very good reasons why you should do so; not least of which is leaving your method open to scrutiny helps avoid [long-running bitter disputes](https://physicstoday.scitation.org/do/10.1063/PT.6.1.20180822a/full/) and of course increasingly funding bodies and publishers are demanding it (strange though that while journals demand open access to your code they don't demand open access to their publications but that's another issue).

Everyone is busy telling each other that this is Very Good Thing(tm) and no-one stops to question it or think about the problems.  I think one of the problems relates to how we think about reproducibility.

What is reproducibility?  I contend that there are two things you want to verify when you publish your method.  The first, is that the abstract (Platonic ideal?) version of the method is correct.  The second is that your code accurately represents this method and is not producing results tainted by some programming error either on your part or on a dependency.  The movement to opening code is drastically focused on the latter, to the complete detriment of the former.  In fact, it makes it so easy for someone to verify the latter that they don't really look at the former.

I'm going to risk using an analogy here, but imagine in the early 1900s you are replicating someone's experiment from a paper.  You read the paper, beg, buy, borrow or steal the equipment, re-run the expermiment and get similar results.  This gives some verification that the method is correct.  What you do *not* have access to is the ability to instantly summon from the ether an exact replica of the original scientist's lab, complete with bits of cardboard wedging rattling stands or whatever little fixes have been made to the environment.  If you could, what are you proving?  That if you do exactly the same thing to the same inputs you get exactly the same result?

Today, you can, from github, clone the exact experimental set-up and run the same experiment and show that yes, if you do exactly the same thing you get the same results.

```
Calculating PI using
  50 slices
  1 process
Obtained value of PI: 3.2216179877229236d0
Time taken: 0.0 seconds
```

Here is my central problem; if you do this, what do you know about the method itself?  Or even the implementation of the method?  The only way you can know if the method itself is useful (as opposed to a lucky guess) is to change that environment.  Does it "work" with different inputs?  Does it "work" if you use a different compiler, a different language, if you re-implement it from first principles?  There is at least one well known, often published package that requires a specific version of a specific compiler to work.  Is that method correct?  It sounds iffy to me.

Publishing the code allows you to forensically investigate what was done, once you know there is a problem, it's is a lot less helpful for verifying that something is correct.  It is my belief that what has happened is that accidently we've imported the software engineer version of reproducibility into science where we fetishise bit-perfect reproduction of the method and I think that's useful, but if that's all we care about then we are making a mistake.

We need to better educate new computational scientists on what we mean by reproducibility and not just say "use containers, it'll all be solved".  It's not solved, it's fragile.  A *good* implementation of a method should be stable across compiler versions, different library versions, operating systems etc.

## If you publish your code, someone will download and run it.

The worst of these problems is of course that if you publish your code in a form which makes it easy to run, then other people will download and run it.  "But that's the point" I can hear people heckle from the back.  Well, I ask, is it?

Unfortunately, I think as with reproducibility, we are mistakenly mixing up two concepts which I will split into "application" and "method".

Applications are large bodies of code intended to be used by multiple research groups on different problems.  Think [LAMMPS](https://lammps.sandia.gov/) or [GROMACS](http://www.gromacs.org/).  This code is absolutely designed to be run by other people.  Methods on the other hand are the scaffold a researcher builds around it.  This code is highly tailored to both the environment they work in and the experiments they are doing.  Going back to the lab example from the previous section, applications might be a laser while method is "balance the laser on that text-book".

Applications absolutely need to be published and should be run by other people.  Methods on the other hand should be published and _replicated_ by other people.

Unfortunately, we don't separate this in our heads and so our lives go horribly wrong.  At least once a week I am asked to help some poor grad student who is trying to install a "code" they were told to investigate by their PI and then apply to their dataset.  This code is very much method, usually undocumented, with no-one supporting it or responding to email.  Quite often it's in a language the grad student doesn't know, is not well written and is full of hard-coded paths, submission script etc.  Who even knows if it is *appropriate* to apply these scripts to their data?

I have had users who have been provided with, as part of the "open science" a binary compiled on some long ago lost version of Linux that now mysteriously seg-faults and no-one has ever documented what it does beyond converting filea to fileb.  I have another user who has been turned off python for life because she had been told to investigate an "application" which turns out to be a single nightmarish monolithic python script full of all kinds of awful obfuscations.

Now, it's important to point out that none of this is really the fault of the person publishing the code either (well the binary one is, but you get my point).  They have published their method as they were told to.  It's not actually reasonable and not anything we've asked previous generations of scientists to do, to package up their experiments as usable tools for other people.  They are _experiments_ not applications.  Equally, it's not the fault of the grad student either; they are new to this environment and almost certainly have a rosy idea of what the code can do based on publications or presentations.  Worse, they are in an environment where they have to get things running as quickly as possible and are not incentivised to do the due diligence that perhaps they should.

The problem is in us, the way we talk about publishing code and the what we educate students to expect from scientific code that is published.

## In conclusion...

Of course we need to do Open Science and you should publish all your code.  But I think we as a community need to be more explicit with new researchers as to what these things mean.  It would be helpful if when publishing code, there was some clear, standardised way of publishing with it what you should expect from the code - is it the method or the application?