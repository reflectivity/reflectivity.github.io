---
layout: page
title: "Calculation Efficiencies"
permalink: /information/calculation/
---

The Abeles matrix transfer method is an efficient way of calculating reflectivity. In a calculation there are a few aspects that dominate calculation time:

1. `sqrt` of complex numbers to calculate wavevectors
2. `exp` of complex numbers to implement Nevot Croce smearing, and in calculation of characteristic matrices
3. matrix multiplication of the 2x2 characteristic matrices.

There are typically many datapoints measured (`NQ`), and potentially hundreds of layers (`NL`). Calculation over all these would typically require a nested loop in C (outer loop over NQ, then inner loop over NL), so thought should be given to how much calculation can be lifted out of those loops, and how to make the innermost loops efficient as possible. In Python (or Cython) those nested loops can be completely removed by using numpy vectorisation. However, in my experience this still isn't as fast as straight C. In Julia it's better to follow the C route of nested loops.
When compiling C code there *may* be some compiler flags that are ok to use, that give minor speedups (e.g. `-funsafe-math-optimizations, -ffinite-math-only`). This will be compiler and platform specific, and make assumptions about input and output.
It is not worth trying to write functions to perform complex arithmetic, those provided by the maths (C/C++) libraries are typically faster, and also lookout correct mathematical performance for things like overflow and the correct branch cuts to use when calculating square roots.

There are some language specific ways of performing quick matrix multiplication, it's typically faster to hardcode the matrix multiplication for each of the entries (but less clear as to what's being done).

As a relative guide for a two layer model with 1000 datapoints it should be possible to calculate a reflectivity curve in C in ~250 microseconds, or about 433 microseconds in Python (using numpy). The reflectivity calculation should be O(N) for both NQ and NL.

Lastly, the use of the fastest kernel calculation speed (`C`?) may not be desirable in all circumstances. For example many optimisation analysis packages (using Bayesian MCMC or scalar minimisation) may be able to use gradient or jacobian information in their operation. Historically these gradients have been obtained via numerical differentiation, but Automatic Differentiation (AD) packages such as JAX/Autograd (Python) or ForwardDiff.jl (Julia) can calculate gradients directly from the function of interest. These AD packages typically require that function are coded in a certain manner, and can't autodiff C code. The gains from AD may outweigh the cost of a slightly lower reflectivity calculation speed.

## Parallelisation in reflectivity analysis software

There are a few different opportunities for parallelisation, depending on the type of calculation being performed and the type of computer system it's being done on:

1. a specular reflectivity kernel can be parallelised over the number of datapoints in a calculation. If the kernel is implemented in C or Cython then using a threading library like OpenMP, or pthreads, gives good performance. If resolution smearing is being performed, which often requires an oversampling approach, then it makes sense to send all the oversampled points for calculation at the same time (rather than creating an extra loop for a resolution integral), as this maximises the usefulness of the thread library.

2. the optimisation approach may be parallelisable. For example, Bayesian MCMC or differential evolution often needs to independently calculate hundreds (or thousands) of cost functions at once. In Python one would typically use the `multiprocessing` module if you are on a desktop machine, or the `mpi4py` package if you're on a cluster. There is always overhead to distribute this kind of task, and the overhead becomes less costly as the problem becomes more computationally expensive (i.e. each processor is given more work to do). If each cost function is quick to calculate it may be better to stay on a desktop computer; if it takes a long time then a cluster may be better.

Nesting of parallelisation can create issues, and significantly degrade performance. For example, when doing MCMC with hundreds of cost functions to evaluate it may make sense to distribute those over the entire computational capacity you have access to (e.g. an MPI setup on a cluster). However, if each cost function evaluation then uses a thread library to calculate a specular reflectivity kernel, requesting compute from more than 1 processor, there will be oversubscription of resources, and the whole setup slows down dramatically. In such a situation it's better to do the reflectivity calculation in a serial manner (single thread), with the cost functions being distributed over all the available processors. These considerations mean that it's a good idea for analysis packages to retain fine-grained control over the parallelisation they employ, so they can be tuned at different levels depending on the problem. Note that the Python `multiprocessing` module does not work with many `omp` libraries.

I have experimented using GPU (`pyopencl`) to calculate reflectivity in a highly parallelised manner. If the calculation system is small it's better to calculate on a CPU as there is overhead to offloading the calculation to a GPU. At the calculation system becomes larger (more datapoints, more layers in the model) there is a crossover point for the GPU calculation becoming faster.
