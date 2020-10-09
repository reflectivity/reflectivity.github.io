---
layout: page
title: ORSO file format - tasks - Internal references
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-08
---

In the examples in task [ascii-representation](ascii-representation.md) 
there is are references of the type `column: <value>` given with the 
wavelength and angle information, respectively. They point to the columns in the data
table containing the respective values for each data tupel.

This link can be used to tell the analysis software to use the content of this column.

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

- are document-internal references allowed?
- How are they realised?
 
