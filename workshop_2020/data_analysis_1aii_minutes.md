---
layout: page
title: "Data Analysis 1a(ii) Minutes"
permalink: /workshop_2020/data_analysis_1aii_minutes
---

Participants:
- Jochen Stahn (chair)
- Artur Glavic
- Paul Kienzle 
- Phillip Gutfreund 
- Becky Welbourn
- Max Skoda
- Mrinal Bera
- Andrew Caruana

**priority of topics (verification and vocabulary):**

After a few minutes we only talked about verification. 
This somehow shows our priority set here.

**verification:**
- At the moment for verification there is no distinction needed between engine and program. This caused some confusion among those who do no programming, it is also not that clear where the boundaries of the engine are. We assumed it is just the part of the program doing the simulation / fitting.
- What are the commonly used algorithms? (Parratt, Abeles matrix, ?)
- A list of analysis programs (as on orso page) with their features (x-ray / neutron / polarised / specular / off-specular / ...) would be helpful (in general, but here) to single out what has to be verified and which programs can be compared.
- for the testing / verification:
  - We assume that the tests are based on the simulation performance rather than on fitting. I.e. One starts with a model and compares the resulting reflectivity.
  - If one starts with a well defined, laterally homogeneous density profile, all engines should give the same result. Else there is a bug. So this is boring but necessary.
  - It is getting more interesting if the creation of the density profile is included. E.g. when the roughness is described by a Gaussian whose reach significantly exceeds the thickness of a layer. This then no longer probes the "engine" but the sensibility or robustness of the model-to-density calculation to wrong or critical parameters.
  - We briefly discussed the option of using a simple slab model of a couple of layers and systematically varying each parameter to check the different fitting programs deal with these in the same way. This may be easier to implement than having to create a series of very different models in each program. Testing of more complex models which require the program user to script and interpret the model may test the programmer more than the program. 
  - We discussed the probability to use micro-slabs to model the non-Gaussian or asymmetric roughness. Also an algorithm was suggested which switches from the fast Gaussian roughness to the more robust micro-slabs during fitting.
  - model-free algorithms drop out of this testing scheme.
-	It was not clear if we stick to specular reflectivity, verify also off-specular intensity or even GISAX/GISANS. We recommend to start without gracing incidence.
-	Some participants offered to provide typical sample descriptions or to run verification simulations on their software. It is not clear how this is organised and who is the contact person.
