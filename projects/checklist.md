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

## Before the experiment (and the experimental plan)

1. Do you have a scientific question or questions that you want to answer?
    1. Have you spoken to an instrument scientist to determine if reflectivity can help you answer the question? 
    2. Don’t just say this system is interesting, please give us the beamtime? 
    3. Can you leverage techniques like contrast matching?
    4. Can you use complimentary x-ray reflectivity to improve the scientific outcome? IE. Resonant magnetic scattering/XMCD and PNR or NR and XRR. 
    5. Have you performed some basic simulations of what you expect to measure to get a feel for how the experiment will work? 
    6. Is the facility/Instrument answer the question that you are posting?
2. Samples:
    1. Can you make the system if it is proposed? 
    2. If it’s not made can you do experiments to work out how to make it? (This will be its thread) 
    3. Can you make the sample reproducibility
    4. Can you make the sample big enough and flat enough for an NR/PNR/PA measurement? 
    5. Pre-characterisation of the sample? What do you need to do to know you have the correct effect (issues with chopping up samples etc) 
3. Experimental plan: 
    1. Do you have an experimental plan on how to do the measurement to answer the scientific question or questions you state in the proposal? 
    2. Again have you spoke to the instrument scientist about what this entails and how much neutron time will be required (on a facility/instrument by instrument basis) 
    3. Is the beamline characterised/setup correctly, detector corrections, transmissions, Polarisationcorrects etc? 
    4. Have you run a pilot meausement to check how long you need to run for?
    5. Have you characterised your blocks in required correctly? 
    6. Have you taken sufficient transmissions etc through blocks and Sample environment etc?  
4. Analysis planning:
    1. Do you have the expertise/capability to analyse the data? (Time /Personpower) 
    2. If not what do you think is need to get to a place you can analyse your data? 

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

