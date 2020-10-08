---
layout: page
title: ORSO file format - tasks - binary representation
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-08
---

# Binary representation

For the binary representation HDF5 seems to be the favoured 
*format*. But there is a discussion whether or not to use / allow attributes.

This representation is meant ....

canSAS uses attributes, but they are not compatible with 
YAML, ....

## tasks

- The attribute-free HDF5 might contradict the orso decision to stay 
  compatible with canSAS and Nexus. This has to be checked.

## decision

- Do we insist on / recommend HDF5 for the binary representation?
- If so: with or without attributes?


