---
layout: page
title: ORSO file format - tasks - ASCII representation
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-08
---

# ASCII representation

For the ASCII representation it was suggested to use wrapped YAML.
Meaning that the header is structured with YAML and that the body consists of
a table. The column description is (the last) part of the 
header.

This *format* is compatible with most analysis software and plotting
programs so that it allows for fast data visualisation on almost any
system and it can be used for simple or standard data reduction. 
Also by inexperienced users.

## tasks

- There are representations for the data equivalent to YAML (in terms of
  hierarchy and content, not necessarily in terms of human readability).
  Which is the *best*?

## decision

- Do we insist / recommend to use wrapped YAML for the ASCII representation?


