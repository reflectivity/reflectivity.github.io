---
layout: page
title: ORSO file format - tasks - Multiplicity of hierarchies
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-08
---

# Multiplicity of hierarchies

## discussion

The level of *reduction* is represented in the various possible
locations in the data tree where information is stored.

E.g. for a monochromatic beam it is sufficient to supply the wavelength information
(value, resolution and unit) only once. The *right* place is the 
`experiment/measurement` section. But for a energy-dispersive 
set-up the wavelength might be given for each measurement point in
and extra column. 

Below several examples of hierarchical structures are given for
various measurement schemes and levels of reduction. Shown are only the
entries relevant for this discussion.

### monochromatic, angle-dispersive scheme

The wavelength and its resolution are given only once because
they are constant. But the `angle` points to the column where
the individual values are given. These allow for footprint correction.

```YAML
experiment:
    measurement:
        scheme:     angle-dispersive
        wavelength: {value: 4.0, resolution: {type: constant, sigma: 0.1}, unit: angstrom}
        angle:      {column: 5}
reduction:
    input files:
        - file: data1
        - file: data2
columns:
    - {column: 1, variable: Qz, unit: 1/angstrom}
    - {column: 2, variable: R}
    - {column: 3, variable: sR}
    - {column: 4, variable: sQz, unit: 1/angstrom}
    - {column: 5, variable: angle, unit: deg}
```

### energy-dispersive scheme, one angle

The `wavelength` column allows for absorption correction.

```YAML
experiment:
    measurement:
        scheme: energy-dispersive
        wavelength: {column: 5}
        angle: {value: 1.2, resolution: {type: const, sigma: 0.02}, unit: deg}
reduction:
    input files:
        - file: data1
        - file: data2
columns:
    - {column: 1, variable: Qz, unit: 1/angstrom}
    - {column: 2, variable: R}
    - {column: 3, variable: sR}
    - {column: 4, variable: sQz, unit: 1/angstrom}
    - {column: 5, variable: wavelength, unit: angstrom}
```

### energy-dispersive scheme, various angular settings

The wavelength information applies for all input files, thus it is
in the `experiment/measurement` section. 
The `angle` here describes the individual raw data files. 
Both entries are information for the user, since the level of
reduction assumed here (merging of the data form both raw files) does not allow
for later absorption or footprint corrections.

```YAML
experiment:
    measurement:
        scheme: energy-dispersive
        wavelength: {min: 3.0, max: 12.0, resolution: {type: const, sigma: 0.1}, unit: angstrom}
reduction:
    input files:
        - file: data1
          angle: {value: 1.2, spread: 0.02, unit: deg}
        - file: data2
          angle: {value: 2.4, spread: 0.02, unit: deg}
columns:
    - {column: 1, variable: Qz, unit: 1/angstrom}
    - {column: 2, variable: R}
    - {column: 3, variable: sR}
    - {column: 4, variable: sQz, unit: 1/angstrom}
```

### angle- and energy-dispersive, various angular settings, compact

Data are re-binned and histogrammed to a (given) `Qz` grid. 
Resolution information is partially lost.

The universal `wavelength` and the raw file specific
`angle` are descriptive. They can hardly be used
for data analysis.

```YAML
experiment:
    measurement:
        scheme: angle- and energy-dispersive
        wavelength: {min: 3.0, max: 12.0, spread: 0.1, unit: angstrom}
reduction:
    input files:
        - file: data1
          angle: (min: 0.6, max: 2.0, unit: deg}
        - file: data2
          angle: (min: 2.4, Max: 3.8, unit: deg}
columns:
    - {column: 1, variable: Qz, unit: 1/angstrom}
    - {column: 2, variable: R}
    - {column: 3, variable: sR)}
    - {column: 4, variable: sQz, unit: 1/angstrom}
```

### angle- and energy-dispersive, various angular settings, detailed

Data might be re-binned, but are not histogrammed. I.e. there might be 
several entries with (about) the same `Qz`, but with different `wavelength` and
`angle` and thus `sQz`.

The analysis software might use either `sQz` or the `(wavelength, angle)`
tuple, where the resolution information for both is given in the universal
part. Besides this, the universal `wavelength` and the raw file specific
`angle` are descriptive and not meant for data analysis.

```YAML
experiment:
    measurement:
        scheme: angle- and energy-dispersive
        wavelength: 
            min: 3.0
            max: 12.0
            resolution: {type: prop, sigma: 0.02}
            unit: angstrom
            column: 5
        angle: 
            resolution: {type: const, sigma: 0.01}
            unit: deg
            column: 6
reduction:
    input files:
        - file: data1
          angle: (min: 0.6, max: 2.0, unit: deg}
        - file: data2
          angle: (min: 2.4, max: 3.8, unit: deg}
columns:
    - {column: 1, variable: Qz, unit: 1/angstrom}
    - {column: 2, variable: R}
    - {column: 3, variable: sR}
    - {column: 4, variable: sQz, unit: 1/angstrom}
    - {column: 5, variable: wavelength, unit: angstrom}
    - {column: 6, variable: angle, unit: deg}
``` 

## tasks

- What are the consequences for the reduction software?
- What is the consequence for the analysis software when the
  information might be found in various places in the 
  file?

## decision
 
