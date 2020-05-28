---
layout: page
title: "Data Analysis 2b(i) Minutes"
permalink: /workshop_2020/data_analysis_2bi_minutes
---

Participants:
Chair: Andrew McCluskey
Scribe: Brian Maranville

**shared work on a kernel**
Mrinal and Arwel and Andrew described a desire for a shared calculation kernel, than can be optimized for efficiency, with an API available in various languages and validation available.  
Emphasize speed, assume correctness

Arwel pointed out that speed = parallelization, which means openmp in many contexts, which becomes architecture and platform dependent quite fast.

Mrinal: start with something more accessible
Brian: like ANSI C?
Mrinal: yes.

Action item: create a github repo for working on shared kernel with verified outputs, along with wrappers (api) in at least
1.	python
2.	matlab
3.	c++
4.	java

Optimizers
Also useful to have api access to optimizers of different types, to work with the kernel
Action item: make one of these repos too

Resolution
Also useful to have libraries available for resolution smearing.
Arwel and Mrinal: just fourier-transform and multiply with gaussian, and fourier-transform back.
Action item: another repo with algorithms for doing this (fast)

Shared problem definition language:
Arwel: there’s no way to make a one-size-fits-all interface for users.
Need to support: soft matter, polarized, magnetic layers, X-rays, etc.
A library of approaches would be more helpful.

Strong resistance to single shared vocabulary.

Example: Arwel would parametrize a problem according to thickness,  SLD, hydration of layers.  
Share all parameters between models except SLD, one SLD for different deuteration-hydrogenation mixtures.  
Andrew would use similar language to describe his problem.

Becky: using different modeling programs is made much more difficult by inconsistencies in the handling of e.g. roughness specification (does roughness n correspond to between layer n and n+1, or n-1 and n)

General agreement that there should be different user interfaces for specifying problems from different scientific areas.

Arwel pointed out that imposing a shared vocabulary for defining problems automatically creates restrictions on each program (how would we extend the vocabulary, for instance?)

Action item: standardise at least the way we talk about the roughness of layer N?
