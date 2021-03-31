---
layout: page
title: ORSO file format - text representation
author: Jochen Stahn, *Paul Scherrer Institut, Switzerland*
date: 2021-03-31
---

## structure of the text data file

following the discussions and decissions made during the workshop on 2021-03-22.

There are still some unsolved issues, marked here in **bold**. Please comment on these!

### the 1st line

The first line contains information about

- the file type;
- the general content;
- the orso file foramt version used.

Artur agreed to provide the layout and rules for this line.

```
#  <to be defined>
```

### meta data (the header)

What follwos arev several blocks with meta data, where the **dictionary is not yet
defined**. Thus here only the format, but not the content is mendatory.

```
# creator:
#     name        : Jochen Stahn
#     affiliation : PSI
#     time        : 2020-04-06T13:21:18
#     system      : lnsa17.psi.ch
# data_source:
#     owner              : Jochen Stahn, PSI
#     facility           : Paul Scherrer Institut, SINQ
#     experiment ID      : 2019 0042
#     experiment date    : 2020/02/02 - 2020/02/04
#     title              : Generation of input for file formatting purposes
#     experiment:
#         instrument     : neutron reflectometer Amor
#         probe          : neutrons
#         sample         :
#             name         : Ni1000
#     measurement:
#         scheme         : energy-dispersive
#         omega: 
#             magnitude  : 1.2
#             unit       : deg
#         wavelength:
#             min        : 4.0
#             max        : 12.5
#             unit       : angstrom
#         polarisation   : +
# reduction:
#     software: eos.py
#     call : eos.py -Y 2020 -n 1925-1927 -y 9,55 ni1000 -O -0.2 -r 1064 -s 1 -i -a 0.005 -e
#     comment:
#         corrections performed by normalisation to measurement on reference sample
#     input_files:
#         references:
#             - file     : amor2020n001064.hdf
#               created  : 2020-02-02T15:38:17
#         data_files:
#             - file     : amor2020n001925.hdf
#               created  : 2020-02-03T14:27:45
#             - file     : amor2020n001926.hdf
#               created  : 2020-02-03T14:37:15
#             - file     : amor2020n001927.hdf
#               created  : 2020-02-03T14:27:02
```

### column description

The last part of the header is the descripotion of the columns to follow.

```
# columns:
#     - {quantity:  Qz, unit: 1/angstrom}
#     - {quantity: RQz, unit: 1}
#     - {quantity:  sR, unit: 1}
#     - {quantity:  sQ, unit: 1/angstrom, comment: 'sigma of Gaussian resolution function'}
#     - {quantity: lambda, unit: angstrom}
```

Optionally, the last line might be a short-notation description

```
# #         Qz             RQz              sR              sQ          lambda
```

where the second `#` means that this is a comment line not to be processed.

### data set

Each data set consists of a rectangular array of numbers. All entries have the same
data type (the default is `float`).

```
1.03563296e-02  3.88100068e+00  4.33909068e+00  5.17816478e-05  4.00000000e+00 
1.06717294e-02  1.16430511e+01  8.89252719e+00  5.33586471e-05  4.10000000e+00
...
```

### multiple data sets

#### empty line

This is optional. An **empty line** is recognised by gnuplot as a separator
for 3 dimensional data sets.

#### separator

If more than one data set is provided, they are saparated by a line starting with

```
# data_set: <identifier>
```

where `<identifier>` is either an unique name or a number. The default numbering 
of dats sets starts with 0, the first additional one thus gets number 2 and so on.

#### overwrite meta data

Below the separator line, meta data might be added. These overwrite the meta data 
supplied in the header (**i.e. data set 2 does not know anything about the changes made
for data set 1**).

```
# data_source:
#     measurement:
#         polarisation: -
# reduction:
#     software: eos.py
#     call : eos.py -Y 2020 -n 1930 -y 9,55 ni1000 -O -0.2 -r 1064 -s 1 -i -a 0.005 -e
#     input_files:
#         data_files:
#             - file     : amor2020n001930.hdf
#               created  : 2020-02-03T15:27:45
```

#### next data set

of the same format as data set 0

```
1.03563296e-02  1.08100068e+00  4.33909068e+00  5.17816478e-05  4.00000000e+00
1.06717294e-02  1.06430511e+01  8.89252719e+00  5.33586471e-05  4.10000000e+00
...
```

## example

all of the above mentioned lines without comments.

`text_example.ort`

```
# <first line>
# creator:
#     name        : Jochen Stahn
#     affiliation : PSI
#     time        : 2020-04-06T13:21:18
#     system      : lnsa17.psi.ch
# data_source:
#     owner              : Jochen Stahn, PSI
#     facility           : Paul Scherrer Institut, SINQ
#     experiment ID      : 2019 0042
#     experiment date    : 2020/02/02 - 2020/02/04
#     title              : Generation of input for file formatting purposes
#     experiment:
#         instrument     : neutron reflectometer Amor
#         probe          : neutrons
#         sample         :
#             name         : Ni1000
#     measurement:
#         scheme         : energy-dispersive
#         omega: 
#             magnitude  : 1.2
#             unit       : deg
#         wavelength:
#             min        : 4.0
#             max        : 12.5
#             unit       : angstrom
#         polarisation   : +
# reduction:
#     software: eos.py
#     call : eos.py -Y 2020 -n 1925-1927 -y 9,55 ni1000 -O -0.2 -r 1064 -s 1 -i -a 0.005 -e
#     comment:
#         corrections performed by normalisation to measurement on reference sample
#     input_files:
#         references:
#             - file     : amor2020n001064.hdf
#               created  : 2020-02-02T15:38:17
#         data_files:
#             - file     : amor2020n001925.hdf
#               created  : 2020-02-03T14:27:45
#             - file     : amor2020n001926.hdf
#               created  : 2020-02-03T14:37:15
#             - file     : amor2020n001927.hdf
#               created  : 2020-02-03T14:27:02
# columns:
#     - {quantity:  Qz, unit: 1/angstrom}
#     - {quantity: RQz, unit: 1}
#     - {quantity:  sR, unit: 1}
#     - {quantity:  sQ, unit: 1/angstrom, comment: 'sigma of Gaussian resolution function'}
#     - {quantity: lambda, unit: angstrom}
# #         Qz             RQz              sR              sQ          lambda
1.03563296e-02  3.88100068e+00  4.33909068e+00  5.17816478e-05  4.00000000e+00 
1.06717294e-02  1.16430511e+01  8.89252719e+00  5.33586471e-05  4.10000000e+00
...

# data_set: ni1000_dn
# data_source:
#     measurement:
#         polarisation: -
# reduction:
#     software: eos.py
#     call : eos.py -Y 2020 -n 1930 -y 9,55 ni1000 -O -0.2 -r 1064 -s 1 -i -a 0.005 -e
#     input_files:
#         data_files:
#             - file     : amor2020n001930.hdf
#               created  : 2020-02-03T15:27:45
1.03563296e-02  1.08100068e+00  4.33909068e+00  5.17816478e-05  4.00000000e+00
1.06717294e-02  1.06430511e+01  8.89252719e+00  5.33586471e-05  4.10000000e+00
...
```

