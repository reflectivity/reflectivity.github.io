---
layout: page
title: ORSO file format - tasks - Internal references
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-08
---

# Internal references

In the examples in section [Multiplicity of hierarchies] there is a link
of the type `column: <value>` in case there is an extra column in the
table representing the respective quantity.

This link can be used by the analysis software to use the content of this column.

It is also a way to give extra information to the data in the column by
providing the resolution type and value.

## tasks

- This looks good in the YAML representation, but is it compatible
  with HDF?  
  (Jochen Stahn)
- What is the programmers view?
- Are there other, more intuitive or compatible ways to realise this?  
  (Jochen Stahn)

## decision

- are document-internal links allowed?
- How are they realised?
 
