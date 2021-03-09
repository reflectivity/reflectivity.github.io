---
layout: page
title: ORSO file format - tasks - binary representation
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-08
---

The binary representation is meant to store and provide extensive data sets.

HDF5 seems to be the favoured 
*format*. But there is a discussion whether or not to use / allow attributes.


### attributes

HDF5 (abd other containers) provide *attributes* for entries. The advantage is
that ....

On the down-side, attributes are not forseen in all (relevant) representations. 
Especially YAML (and related languages) does not know them. It is in principle possible
to mark entries in YAML in a way that they are recognised as attributed when converting
to HDF5, but these marks and the required type-description contradict our requirement
for easy readibility of the ASCII representation.

There are some [frightening examples](https://github.com/canSAS-org/NXcanSAS_examples/tree/master/canSAS2012_examples/structure)
for this on the NXcanSAS pages.

## tasks

- The attribute-free HDF5 might contradict the orso decision to stay 
  compatible with canSAS and Nexus. This has to be checked.

## decision

- Do we insist on / recommend HDF5 for the binary representation?
- If so: with or without attributes?


