---
layout: page
title: ORSO file format - tasks - Several data sets
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2021-03-21
---

How doe we deal with series of data sets? 

Examples are 

- various spin states,
- time-resolved measurements or 
- measurements with varying external partameter.
- a combination of those.

*simple* approach: one data set - one file

NCNR-online tool export: data blocks for spin polarised data are preceded by a marker line

```
#  "polarization": "++"
```

and separated by blank lines. 
