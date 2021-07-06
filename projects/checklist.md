---
layout: page
title: "Reproducibility Checklist"
permalink: /projects/checklist
author: "Andrew McCluskey"
---

We want to develop a checklist to ensure that we are performing reproducible experiments and analyses. 
For some inspiration please check out the following paper from the [chemical machine learning community](https://doi.org/10.1038/s41557-021-00716-z).
Also check out [Stuart Prescott's talk from the introductory session](https://teaching.complexfluids.net/short-courses/ORSO2021/prescott-orso2021-reproducibility.pdf).

To get things started, we suggest breaking this checklist into two parts: before the experiment and after the experiment. 

Please complete the [survey on the importance of different aspects of the checklist](https://forms.gle/UY5M9v6jBK73ewHGA).

## Before the experiment (and the experimental plan)

1. **Do you have a scientific question or questions that you want to answer?**
   1. Have you consulted an expert/previous literature to determine if reflectivity is able to answer your scientific question?
   2. Did you ask an instrument scientist if the facility/instrument/technique the **best** place to answer your question?
   3. Do you know when the proposal deadline is and what writing a proposal involves?
   4. Do you know how reflectivity data is modeled and analysed?
   5. Have you performed some basic simulations of what you expect to measure to get a feel for how the experiment will work?
   6. The experiment/analysis is improved with precharacterisation, can/have you use complimentary methods to inform your scientific question (XRR/NR/PNR/SQUID/ellipsometry)?
   7. Can you tune the sample to improve experimental sensitivity (contrast matching/multilayers/larger surface)?
   8. The answers to questions 1,4-7 should be discussed in your proposal.
2. **Are you going to measure what you think you are measuring?**
   1. Have you documented how you would make your sample?
   2. Can you make the sample reproducibly enough (including characterisation of the sample), for the scientific question?
   3. Are the samples stable for the duration/conditions of the measurement (ie. beam damage in XRR)?
   4. Have you considered the effect of sample imperfections on your experiment (sample size/flatness/roughness)?
   5. Do you need to prepare the sample at the facility, is there the necessary equipment?
   6. Do you know if there is a suitable sample environment for your study (including simultaneous characterisation), if not can one be developed?
3. **Have you done your home (preparing for the beamtime)?**
   1. Does your experimental team have the person power and knowledge to run the experiment?
   2. Do you know (including alignment/temperature ramping/etc) how long the experiments will take, do you need to do a test measurement?
   3. What control and null measurements are necessary for your scientific question/sample environment?
   4. Do you have a measurement order and a backup plan?
   5. Have you done **all** necessary remote training (facility access stuff/software is possible)?
   6. Has anything changed since the proposal or has the sample changed (may have safety implications)?
   7. Do you that the instrument is setup for the measurements you want to do?
   8. Is there an agreed upon system for documentation (run numbers/metadata/logbook/calibration measurements) and have you shared with the instrument scientist?
4. **Can you answer the question?**
   1. Do you have an understanding of the analysis methodology (ie. the analytical model) that you will use?
   2. Do you know what a "good" reduced dataset should look like, for example from the simulation?
   3. Is it possible to perform on the fly/live experimental analysis?
   4. Do you know how to reduce the data to be suitable for analysis (this might be done for you)?
   5. Can you load the reduced data into the analysis software of choice?
   6. Do you have the expertise/capability to analyse the data?
   7. Can you access all the important metadata for your anlaysis?
   8. Do you know the corrections that have been performed on your data?

## After the experiment and analysis 

(this is heavily borrowed from the above paper)

1. Data sources
    1. Are all data sources listed and publicly available?
    2. If using an external database, is an access date or version number provided?
    3. Are any potential biases in the source dataset reported and/or mitigated?
2. Data cleaning
    1. Are the data cleaning steps clearly and fully described, either in text or as a code pipeline?
    2. Is an evaluation of the amount of removed source data presented?
    3. Are instances of combining data from multiple sources identified, and potential issues mitigated? 
3. Data representations
    1. Are methods for representing data as features or descriptors clearly articulated, ideally with software implementations?
    2. Are comparisons against standard feature sets provided? 
4. Model choice
    1. Is a software implementation of the model provided such that it can be trained and tested with new data?
    2. Are baseline comparisons to simple/trivial models (for example, 1-nearest neighbour, random forest, most frequent class) provided?
    3. Are baseline comparisons to current state-of-the-art provided? 
6. Code and reproducibility 
    1. Is the code or workflow available in a public repository?
    2. Are scripts to reproduce the findings in the paper provided?
