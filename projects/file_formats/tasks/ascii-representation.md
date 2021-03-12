---
layout: page
title: ORSO file format - tasks - ASCII representation
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-08
---

For the ASCII representation it was suggested to use wrapped YAML.
Meaning that the header is structured with YAML but with a leading `#`, 
and that the body consists of a non-YAML table.
The column description is (the last) part of the header.

The header will look like this (content under discussion):

```YAML
# reflectivity data file                                 orso file format 0.0
# author:
#     name        : Jochen Stahn
#     affiliation : PSI
#     time        : 2020-09-06T13:21:18
# data source:
#     origin:
#         owner              : Jochen Stahn, PSI
#         facility           : Paul Scherrer Institut, SINQ
#         experiment ID      : 2019 0042
#         experiment date    : 2020/02/02 - 2020/02/04
#         title              : Generation of input for file formatting purposes
#     experiment:
#         instrument         : neutron reflectometer Amor
#         probe              : neutrons
#             polarisation   : +0.97
#         measurement:
#             scheme         : angle- and energy-dispersive
#             wavelength: 
#                 min        : 3.0
#                 max        : 12.0
#                 resolution : {type: prop, sigma: 0.02}
#                 unit       : angstrom
#                 column     : 5
#             angle: 
#                 resolution : {type: const, sigma: 0.01}
#                 unit       : deg
#                 column     : 6
# reduction:
#     software: eos.py
#         call : eos.py -Y 2020 -n 1925-1927 -y 9,55 ni1000 -O -0.2 -r 1064 -s 1 -i -a 0.005 -e
#         comment:
#             - corrections performed by normalisation to measurement on reference sample (-r)
#         corrections:
#             - full footprint correction (-r)
#             - incident intensity (-r)
#             - detector spatial resolution (-i)
#     binning:
#             - Qz range : [:0.01] # Aa^-1
#               type     : linear
#               delta Qz : 0.001 # Aa^-1
#             - Qz range : [0.01:] # Aa^-1
#               type     : exponential
#               calculus : Qz_n = 0.01 * (1+0.005)^n # Aa^-1
#     input files:
#         references:
#             - file     : amor2020n001064.hdf
#               created  : 2020-02-02T15:38:17
#         datafiles:
#             - file     : amor2020n001925.hdf
#               created  : 2020-02-03T14:27:45
#             - file     : amor2020n001926.hdf
#               created  : 2020-02-03T14:37:15
#             - file     : amor2020n001927.hdf
#               created  : 2020-02-03T14:27:02
# columns:
#     - {column: 1, variable: Qz,         unit: 1/angstrom}
#     - {column: 2, variable: R}
#     - {column: 3, variable: sR}
#     - {column: 4, variable: sQz,        unit: 1/angstrom}
#     - {column: 5, variable: wavelength, unit: angstrom}
#     - {column: 6, variable: angle,      unit: deg}
#         1 Qz             2 R            3 sR           4 sQz    5 wavelength         6 angle
```

directly followed by a rectangular table of the type

```
1.03563296e-02  3.88100068e+00  4.33909068e+00  5.17816478e-05  3.50000000e+00  4.00000000e+00
1.06717294e-02  1.16430511e+01  8.89252719e+00  5.33586471e-05  3.55000000e+00  4.00000000e+00
...
```

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


