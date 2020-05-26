---
layout: page
title: "Agenda: Data Analysis"
permalink: /workshop_2020/data_analysis_agenda
---

## [ORSO workshop 2020](/workshop_2020)

### <span class="c22">Session 1: Detailed introduction and aims of the working group -- (26th 18:00/27th 08:00)</span>

*   <span class="c4">Selection of a scribe</span>
*   <span class="c4">Introduction to current members of group</span>
*   <span class="c4">The goal of this group is to share as much as possible of the code and practices that have been developed worldwide for analyzing reflectivity data</span>
*   <span class="c4">This primarily means modeling and fitting - issues related to data reduction (standardisation of format, and reproducibility) have been sorted into the other working groups</span>
*   <span class="c4">Tasks for the workgroup:</span>

*   <span class="c4">Verifying that the various engines that perform reflectivity calculations give the same results for the same inputs (validation)</span>
*   <span class="c4">Defining a shared vocabulary for describing reflectivity model problems (that can be used to verify the calculation engines)</span>
*   <span class="c4">Defining “dialects” for domain-specific applications (e.g. magnetic layered structures, membrane protein systems, polymer melts, etc)</span>

*   <span class="c4">Some work (by A. Nelson) already done on verification task</span>
*   <span class="c4">Discussion topics:</span>

*   <span class="c4">Existing tasks: prioritize</span>
*   <span class="c4">Existing tasks: identify “next step” (action item)</span>
*   <span class="c4">Other tasks: what else can we do to share data analysis between reflectometrists?</span>

<span class="c4"></span>

<span class="c4">Desired output from this session:</span>

1.  <span class="c4">Priorities for tasks</span>
2.  <span class="c4">New tasks</span>
3.  <span class="c4">Action item(s) for tasks</span>

<span class="c1"></span>

### <span class="c22">Session 2: Toward a shared calculation engine -- (26th 22:30/27th 18:00)</span>

<span class="c4">...or at least a shared language for the inputs to the engine</span>

*   <span class="c4">Selection of a scribe</span>
*   <span class="c4">What is the best way to collaborate on a shared reflectometry calculation engine?</span>

*   <span>Just t</span><span>he kernel (writte</span><span>n in C or ?)</span>
*   <span>A hig</span><span>her-level program that takes whole problem definitions (parameters, constraints, etc)</span>

*   <span class="c4">What are the required elements for describing the inputs to a reflectivity calculation kernel?</span>
*   <span class="c4">Build on the work already done on validation (a programmatic adapter for each engine being hand-crafted by A. Nelson)</span>

<span class="c4"></span>

<span class="c4">Desired output from this session:</span>

1.  <span class="c4">A strategy for collaborating on reflectometry calculator</span>
2.  <span class="c4">Discussion on standardisation of problem language</span>
3.  <span class="c4">Any new items</span>

<span class="c4"></span>

### <span class="c22">Session 3: Specific problem definitions: building real models -- (27th 22:30/28th 08:00)</span>

<span class="c4">Dealing with distinct reflectometry problem categories, e.g. magnetic layers, polymer melts, biological membranes...</span>

<span class="c4"></span>

*   <span class="c4">Selection of a scribe</span>
*   <span class="c4">Problem categories might naturally follow implementations of fitting routines (everyone with a very specific fitting problem has had to address this somehow), so…</span>
*   <span class="c4">How many fitting programs are out there? (some may support more than one type of model / problem definition)</span>
*   <span class="c4">Your own fitting problem: What terms would you need to describe the fits you do? Constraints?</span>
*   <span class="c4">How can we collaborate on constructing model builders for all these categories of problem?</span>
*   <span class="c4">Currently calculation/fitting engine and model builder are all one application: how to effectively share calculation engine between model builders</span>

<span class="c4"></span>

<span class="c4">Desired output from this session:</span>

1.  <span class="c4">Identification of problem domains (dialects) people are working on</span>
2.  <span class="c4">Strategy for collaboration on model builders</span>
3.  <span class="c4">Strategy for sharing calculation engine between model types (outputs of model builders)</span>
4.  <span class="c4">Any new ideas for collaboration on domain-specific fitting</span>
