---
layout: page
title: "Data Analysis 2b(ii) Minutes"
permalink: /workshops/workshop_2020/data_analysis_2bii_minutes
---

Participants:
- Tom Arnold (chair)
- Jean-François Moulin (scribe)
- Gaetano Mangiapia
- Mathieu Doucet
- Andrew caruana
- Ben Ocko
- Tom Arnold
- Jean-François Moulin

It was acknowledged that the bare-bones reflectivity calculation is easy and already available.

The real problem lies more in the parametrization of the fitting problem. The description of roughness was given as an example.

Essentially three families of parameters/informations where identified:
    - those necessary for a box model description of the sld
    - Energy dependent parameters (which includes potentially corrections to the data like footprint, resolution, absorption...)
    - possible coupling between parameters.
It was remarked that this variety could make a universal approach challenging.

An essential point was agreed upon: the software should be able to deal with neutrons and X-rays in order to allow co-refinement. The issue of a correct scaling of the respective error bars was acknowledged.

The software should allow correction/fitting of systematic errors

Using several computation engines driven from a single front end should be the aim (overlap with reproducibility sessions). This imposes standardization of input. In order to identify the full set of parameters needed to satisfy the users requests, a consultation approach was proposed. A spreadsheet listing the desired parameters should be setup on the github repository.
This consultation should also try to identify the use cases and for each of them, the parameters, constraints and types of fits. Ideally, examples should be provided. This collection of examples would be the basis for a vocabulary definition which would then be widely acceptable.
