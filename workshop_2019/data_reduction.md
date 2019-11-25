---
layout: page
title: "Data reduction commonalities"
permalink: /workshop_2019/data_reduction
---

### What commonalities do we have?

Efficiencies and background subtractions

- Store the background subtraction and scaling in the output
- Background subtraction is a reduction step and should be stored in a reproducible fashion
- It is equally as important to store what has not been done to the data
- Don't try and deconvolve resolution

Detector corrections 

- Store corrections!
- Masking, noise and monitors

Binning

- No defined q points should be expected
- The bin width should be inclded in the resolution
- Log binning might be a nice general agreement
- Neutron event mode helps with summing (although ill defined)

Instrumental information

- Everything in metadata!!
- Everything should be defined as a value or an array
- The slit positions (and motor uncertainties) should be stored

Documentation: undocumented methods are useless

- a resource to store this in an agreed way 
- link to publications (DOI) 
- include docs in files
- a web resource could help users fix problems themselves (possibly including YouTube videos)
- in particular if reduction method and software is available, however specific metadata needs to be stored

Do the corrections if you can

- with error propagation
- Would a data reduction round robin be interesting?
- correct for "strange" experimental results if you can

Live data reduction is important for a "quick look"

What is reduction and what is analysis?

- These are reduction:
    - footprint
    - overillumination
    - polarisation 