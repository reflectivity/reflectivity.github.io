---
layout: page
title: "Data Analysis 3b(i) Minutes"
permalink: /workshops/workshop_2020/data_analysis_3bi_minutes
---

Participants:
- Philipp Gutfreund
- Andrew McCluskey
- Becky Welbourn
- Alexandros Koutsioumpas
- Gaetano Mangiapia
- Chen Shen
- Randolf Beerwerth
- Arwel Hughes

Topics:

Is there a possibility to categorise distinct reflectometry problems, e.g. for lipid layers?
1) Yes, but should be kept flexible enough for generality on one hand and their physical meaning understandable for users not experienced in scattering on the other hand

2) Generally scripting custom-made models should be always possible, but example models as a starting point for these scripts should be available
3) In any case to collect models into one common library we need a common kernel

4) As a first target one could envisage a high level language to describe different models and the resulting output, which is a stratified slab model without roughness (microslabs). This high-level language is independent of the programming language or kernel.

5) The model builders should always output imaginary SLDs

How should the "master" kernel be realised conceptionally?
  6) The model builder should be separate from the calculation kernel.
7) The calculation kernel should be optimised for speed, including the possibility of parallelisation, allowing for e.g. batch fitting of a large number of reflectivity curves
8) Existing algorithms should be reused, avoid rewriting everything from scratch


Actions:

Andrew: Create blank page on Github to allow for description of common models
All attendees: Fill this page with model descriptions we are using regularly
Arwel: Upload a document pointing out efficient ways to parallelise reflectometry fitting programs
