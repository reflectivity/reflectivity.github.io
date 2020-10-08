---
layout: page
title: ORSO file format - tasks - Common structure and nomenclature for all representations
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-08
---

# Common structure and nomenclature for all representations

J. Wuttke [suggested](https://github.com/jwuttke/file_format/blob/master/current_discussion/dataRepresentation.md)
to define a hierarchy and a dictionary for the data which is
compatible with various representations, e.g. 
wrapped YAML for [ASCII representation] and 
HDF5 for [Binary representation].

This will allow to easily transform one representation into an other, and it
avoids different dictionaries for different representations. I.e. the work
is done only once and confusion is reduced. 

Though this sounds reasonable there might be some problems with this:

- A feature of HDF5 is that an entry might have an *attribute*, where there is
  no equivalent in YAML.
- It might result in incompatibility with canSAS.
- The clientele for the ASCII and the binary representation are not identical
  and the different requirements result in different content of both.

  E.g. the ASCII representation contains much less data to stay 
  *human readable* so that a transformation to the binary representation
  might not be useful anyway.

## tasks

- Check the compatibility with canSAS
- Discussion on the necessity of transforming in both ways
- Collect arguments for or against the common structure (what does it
  mean for programmers? what about possible future formats?)

## decision

- Do we insist on an attribute-free representation of the data?


