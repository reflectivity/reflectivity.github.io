---
layout: page
title: "Neutron and X-ray analysis tutorial text"
permalink: /workshops/workshop_2021/tutorial_text/
author: "Andrew McCluskey"
---

[Current Draft](https://github.com/reflectivity/edu_outreach/releases/download/paper/paper.pdf)

The aim of this tutorial text is to introduce the model-dependent analysis of specular, non-polarised, neutron and X-ray reflectometry data to those that haven't seen it before. 
The text should be comprehensive but basic, additionally, we want to cover **both** neutron and X-ray reflectometry with this text. 

Below, I will run through each section that will be in the text and briefly discuss the scope. 
The aim for the session is to discuss each section, what is missing and what is uneccessary. 

### Introduction

- Introduction of what reflectometry is, the geometry we are investigating, and mention some systems that reflectometry is used for.
- A figure showning the reflectometry geometry, defining specular condition and the *z* dimension.
- Discussion of the Born approximation interpretation of reflectometry analysis, and where this is inaccurate.

## Dynamical approach

- Outline of dynamical approach mentioning Nevot-Croce roughness. 
- Discussion of roughness being too large and "leaking" through.
- Role of microslicing of the SLD profile under certain conditions.
- Plot of microsliced reflectometry against non-microsliced.

## The effect of resolution on analysis

- Discussion of instrumental resolution **in analysis**. 
- Using Gaussian resolution smearing. 
- Monte-Carlo for non Gaussian resolution (is this necessary?)

## Optimisation

- Defined, focused discussion of local and global optimisation in reflectometry analysis.
- Mentioning differential evolution as a popular optimisation approach.
- Multi-model optimsation (summing likelihoods) for multiple contrasts/resonances.

## Uncertainty quantification 

- Uncertainty in parameters can come from gradient descent optimisation methods. 
- Sampling approaches for greater certainty in parameter uncertainty.

## Overfitting

- A "hand-waving" introduction to the problem of overfitting in reflectometry.
- Short discussions of the role of new methodologies (Bayesian and otherwise) to quantify/define overfitting.

## Ill-posedness

- A discussion of the ill-posedness (phase problem) of reflectometry data in terms of the autocorrelation function. 
- This is a big motivation for "model-dependent" analysis, but is complex so comes at the end. 
