---
layout: page
title: "Data Analysis 1a(i) Minutes"
permalink: /workshop_2020/data_analysis_1ai_minutes
---

Participants:
- Brian Maranville (chair,scribe)
- Ben Ocko
- Mark Schlossman
- Wojciech Potrzebowski
- Thomas Saerbeck
- Gaetano Mangiapia
- Juan Manuel Carmona Loaiza
- Jean-Francois Moulin

### General comments
There is a desire to have the calculation engine separable from the graphical interface.

Wojciech - We can look to other parallel efforts offering recommendations in the area of software development for scattering and reflectometry, in particular the outputs of the SINE2020 working group

Ben - Construction of flexible platform would be useful, where people can put in their own algorithms.

Ben - Noted that there is a large software group at his facility, and that we may need to engage with the people actually writing the software in order to make progress on some of our goals 

*later in the conversation it was noted that Juan is actually one of the developers of BornAgain, and was on the call*

Mark - there are programs/utilities available at his facility that do reading, and analysis of the software, handling building of a model by elemental composition.  They are addressing multiple types of analysis on the same sample, such as fluorescence and reflectivity. (using Certified Scientific Software, a company: https://certif.com)

A general note that very few programs for reflectivity analysis have scripting capabilities for large numbers of datasets.

Gaetano - kinetics measurements in reflectivity can produce 100 - 200 datasets, and need to be simultaneously fitted to a shared set of parameters across files.

There were some questions about BornAgain, and how they handle the model-building for specular reflectivity, whether it is a three-dimensional model as is required for the off-specular and GISANS modeling also available in that package.
I think the answer was that it is a layer-based model system separate from the 3d models, and that there is a model-builder built into the program.

Ben - really wants to include a way to add weighting to datasets so that higher-Q data gets more emphasis in the fits, which can be dominated by the low-Q data (much smaller relative error bars).  He does not use Chi-Squared as the main goodness-of-fit evaluation

Wojciech - pointed out that there are tutorials available for BornAgain, which is very helpful for getting users engaged.  This overlaps with Education and Outreach mission.

### Action Items
 - identify developers actually working on data analysis software (and engage with them if not already done)
 - add scripting capabilities to list of desired functionality in reflectometry analysis packages
 - see how dissimilar inputs are for some common analsysis packages (Refl1D and BornAgain as a start)
 - survey users/scientists to get an idea of what everyone is using for analysis
 - add soft X-ray analysis as a desired functionality