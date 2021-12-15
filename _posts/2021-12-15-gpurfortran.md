---
layout: post
title: Making use of the Nvidia HPC SDK to use GPUs from Fortran on Myriad
---

If you have been around High Performance Computing for any serious period of time you will have heard about GPUs, usually in reverent tones, that you have to learn a new programming langauge (CUDA) and become a master of arcane and terrible magicks to apply this to your application.  The good news is that for a long time, since the invention of OpenACC this has not been true, you could write code in Fortran like God intended and add directives and the PGI compilers would generate GPU code for you.  The problem being of course that the PGI compilers were commercial and cost a reasonable amount of money.  Some years ago, Nvidia bought PGI and last year made the rather amazing decision, after switching the backend to LLVM to make their compilers available for free.

You can download the compilers for use on your own system for free from [Nvidia](https://developer.nvidia.com/hpc-sdk) but we are talking about Myriad here which is our centrally run HTC cluster and has a large number of GPUs attached in a range of generations from slightly geriatric P100s, to brand shiny new A100s and as part of the software stack, we have these compilers installed.

You can switch out the default Intel compilers for the latest Nvidia ones by running:

```
module remove compilers
module load compilers/nvidia/hpc-sdk
```

If you run module avail compilers/nvidia/ you’ll see the versions available – you want the most modern centrally installed version, at the time of writing 21.3 as that supports OpenMP on V100s and higher.

Version 21.3 and higher support four different ways of accessing GPUs from Fortran:
1. DO CONCURRENT
2. CUDA Fortran
3. OpenACC
4. OpenMP

In all instances, you can invoke the Nvidia Fortran compiler by running:

```
nvfortran
```

You will then add the appropriate options to turn on compiler features to compile the code.

For this post I will be demonstrating snippets of code for our “canonical” Pi estimation example, a repo for which can be found here: [https://github.com/UCL-RITS/pi_examples](https://github.com/UCL-RITS/pi_examples)

This code does an expensive and not very accurate numerical integration to estimate Pi, something that’s explained in the repository’s `Readme.md`.  What’s interesting from our perspective is this loop, which is extremely parallelisable:

```fortran
  s = 0d0
  step = 1.0d0 / num_steps

  do i = 1, num_steps
    x = (i - 0.5d0) * step
    s = s + 4.0d0 / (1.0d0 + x*x)
  end do

! Evaluate PI from the final sum value, and stop the clock

  mypi = s * step
```

Right - on to the GPU options.

## DO CONCURRENT

A recent addition to the Fortran standard is `do concurrent` which allows you to tell any compiler that the iterations of a loop are independent (order does not matter) and that it can parallelise that loop.  The Nvidia compilers can use this to generate GPU code.

Here we have to modify our loop syntax to use `do concurrent` instead of `do`.

```fortran
  s = 0d0
  step = 1.0d0 / num_steps

  do concurrent (i = 1: num_steps)
    x = (i - 0.5d0) * step
    s = s + 4.0d0 / (1.0d0 + x*x)
  end do

! Evaluate PI from the final sum value, and stop the clock

  mypi = s * step
```

The code can then be compiled with:

```
nvfortran -O2 -stdpar -Minfo=accel -o pi pi.f90
```

This will generate a `pi` binary for you to run on a node with a GPU.

## CUDA Fortran

CUDA Fortran uses a technique that we’ll see for subsequent options – adding special comments or “directives” to your code to tell the compiler that if it supports this feature, it can generate code for it.  Instead of writing your code into a `.f90` file, you put it into a `.cuf` file.  If you use Vim, it probably will not recognise this as Fortran code so you will lose your syntax highlighting.

You can fix this either by manually setting the syntax mode to Fortran, or adding the following to your `~/.vimrc`:

```vim
" Cuda-Fortran
au BufRead,BufNewFile *.cuf set filetype=fortran
```

So what do we need to do to tell nvfortran that we want to parallelise this loop?  It’s actually fairly simple, we add a specialised comment in front of the loop like this:

```fortran
  s = 0d0
  step = 1.0d0 / num_steps

!$cuf kernel do <<< *, * >>>
  do i = 1, num_steps
    x = (i - 0.5d0) * step
    s = s + 4.0d0 / (1.0d0 + x*x)
  end do

! Evaluate PI from the final sum value, and stop the clock

  mypi = s * step

```

You then compile the code with:

```
nvfortran -O2 -Minfo=accel -cuda -gpu=cuda11.2 -o pi pi.cuf
```

Your code is now ready to run!

## OpenACC

OpenACC has been available for a long time as part of the PGI compiler suite and is possibly the best supported way of accessing Nvidia GPUs from Fortran. It has very strong performance, indeed the GPU port of VASP makes use of it to get impressive performance out of A100s.

As with CUDA Fortan, you add in some special compiler directives, the difference being that you put them directly into the `.f90` file.

```fortran
  s = 0d0
  step = 1.0d0 / num_steps

! Specify that loops within this region should have kernels built for GPU

!$ACC KERNELS
  do i = 1, num_steps
    x = (i - 0.5d0) * step
    s = s + 4.0d0 / (1.0d0 + x*x)
  end do
!$ACC END KERNELS

! Evaluate PI from the final sum value, and stop the clock

  mypi = s * step
```

You can then compile the code with:
```
nvfortran -O2 -acc -ta=tesla:cc60,cc70,time,multicore -Minfo=accel -o pi pi.f90
```

As before, this gives you a `pi` binary to run on a node with a GPU.

## OpenMP

The newest option is OpenMP.  You may look at the exmaple and wonder "this is more hassle, why do it this way when OpenACC exists?" and the simple answer is that OpenMP promises to be more portable.  Alhought both OpenACC and OpenMP are supported by Nvidia, OpenACC is *only* supported well by Nvidia and the commercial Cray compilers.  That said, performance for OpenMP on Nvidia is worse and it's limited to V100 or newer cards as well as requiring the newest versions of Cuda + the drivers.

*Due to the card restriction, you must do compilation on a node with a suitable graphics card, so the easiest way to do this is via `qrsh` - check the UCL RC documentation to see how.*

As with OpenACC we modify the `.f90` file directly:

```fortran
  s = 0d0
  step = 1.0d0 / num_steps

! Specify that the loop be parallelised, with summation of individual
! threads' s values to yield overall sum. Specify that the variable
! x is local to each thread.

!$OMP TARGET 
!$OMP TEAMS DISTRIBUTE PARALLEL DO PRIVATE(x) REDUCTION(+:s)
  do i = 1, num_steps
    x = (i - 0.5d0) * step
    s = s + 4.0d0 / (1.0d0 + x*x)
  end do
!$OMP END TEAMS DISTRIBUTE PARALLEL DO
!$OMP END TARGET

! Evaluate PI from the final sum value, and stop the clock

  mypi = s * step
```

You can then compile the code with:
```
nvfortran -O2 -mp=gpu -Minfo=accel -o pi pi.f90
```
As usual, this will give you a `pi` binary which you can run directly as you are already logged into a node with a GPU.

## Conclusions

Well you can now write GPU code in Fortran and don't just have one option but are spoiled for choice.  In the end the best option is probably `do concurrent` for portability or OpenACC for performance and supported features on Nvidia hardware - OpenMP lags behind performance-wise (but is getting better with each release) and CUDA Fortran is too Nvidia-specifc.