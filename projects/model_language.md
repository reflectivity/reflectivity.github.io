---
layout: page
title: "Universal Descriptive Model Language"
permalink: /projects/model_language/
---

# Motivation
When building a physical model of samples for analysis of reflectivity, there is no standard naming convention for some shared concepts (e.g. a single layer or multilayer)

## shared names
There will be benefit to the community if the names for these concepts can be standardised, to allow comparisons of models between modeling packages, for example.

## shared representation
If we go a step further and standardise the whole model representation (names and structure), it would provide users with a way to write a model once and use it with the analysis package of their choice.  

Additionally, if an annotated definition of the model building blocks is made available to the public, users could publish their models with their data in a way that can be understood independently, without needing to load it into an analysis package.


# Implementations
Two routes have been discussed for implementing a shared model language: 

## Simple model language to use for sample description prior to analysis
A draft for a computer readable sample description that is quite universal and can be included in the ORSO file header was introduced by the file formats working group:
[Simple Model Language Specification](/projects/simple_model)

## A set of shared names in an agreed-upon programming framework
In this case, a shared programming framwork is chosen (likely **Python** and/or **Matlab**), and then everyone makes a set of classes/functions with the same names that do the same thing, but the implementation is up to the package maintainer

The model definition will be tied to the framework chosen, but hopefully is portable between packages using that framework.  Models will be defined as code, which is how many of the packages work already.

### Some consequences of this path are
1. Complex constraints can be expressed with powerful code
1. Models are inherently extensible, as long as interfaces are fulfilled
   - i.e. you could redefine one of the classes in the fitting package, in your model
1. Learning curve is very steep, but can be mitigated by
   - Smart code editors (tab completion!)
   - Abundant examples to hack about
   - Tools for generating simple scripts
1. Making a full-featured graphical model builder is hard
   - Builder state -> Model: EASY
   - Model -> Builder state: Potentially impossible
1. Round-trip to fitter (update in-place) is hard, except for parameters

## A set of shared names in a declarative format
In this case, models are a structured document that describes the system and constraints

One downside is that model rendering into e.g. equivalent SLD profiles requires additional tools to aid in interpretation

There are a number of candidate container formats for a declarative model, including
- JSON or similar text format
- BSON or messagepack binary formats
- Protobuf or similar schema’d format

### Some consequences of this path are
1. Constraints must be simpler (come in predefined forms)
1. Extensibility of models:
   - Code has to be written first
   - Then declarative language can be updated
1. Direct editing learning curve will be steep
   - But no one will write these by hand
   - Will be mitigated by model builders
1. Making a full-featured graphical model builder is still hard (but possible)
   - Builder state -> Model: EASY
   - Model -> Builder state: EASY
1. Round-trip to fitter (modify in-place) is easy!

# Names
A preliminary list of names that might be common between packages:

FitProblem
Experiment
Probe
PolarizedNeutronProbe
Stack
ParameterSet
FreeVariables
Parameter
Variable
Calculation
Expression
Uniform
Normal
Comparisons
Operators
Constraint
Slab
SLD
Material
BaseMagnetism
Vacuum
Repeat

# Draft
A draft schema that uses these names can be found at this link ([snapshot from AGM 2021](https://github.com/reflectivity/analysis/blob/75c9b913c2369f8952d3de35a5d284ac474d181b/model_language/refl1d.schema.json))

# Contributing
Anyone can request to be added to the github repository https://github.com/reflectivity/analysis or submit pull requests against it, or submit issues or comment directly on the schema.
