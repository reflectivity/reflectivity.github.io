---
layout: page
title: ORSO file format - tasks
author:
- Jochen Stahn  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-07
---

---

Attempt to collect the ideas and contributions from the ORSO meeiting
in 2020 and the file formats group on September 30.

# Project organisation

how do we structure our work?

## project organisation

ideas / concepts

discussion

preliminary decission

realisation for testing

revision / final decission

release


J. Wuttke [suggested](https://github.com/jwuttke/file_format/blob/master/current_discussion/process.md)
to prepare *chapters* and to release them
individually once they are mature enough. This organisation
would also allow to opt out certain chapters according to 
institutional policy.

The problem there is that a lot of basic decissions are 
entengled and that once we agreed on them, there is not much
to split of as a separate chapter.


# Common structure and nomenclature for all representations

J. Wuttke [suggested](https://github.com/jwuttke/file_format/blob/master/current_discussion/dataRepresentation.md)
to define a hirarchy and a dictinary for the data which is
compatible with various representations, e.g. 
wrapped YAML for [ASCII representation] and 
HDF5 for [Binary representation].

This will allow to easily transform one representation into an other, and it
avoids different dictionaries for different representations. I.e. the work
is done only once and confusion is reduced. 

Though this sounds reasonable there might be some problems with this:

- A feature of hdf5 is that an entry might have an *attribute*, where there is
  no equivalent in YAML.
- It might result in incompatibility with canSAS.
- The clientele for the ascii and the binary representation are not identical
  and the different requirements result in different content of both.

  E.g. the ascii representation contains much less data to stay 
  *human readable* so that a transformation to the binary representation
  might not be useful anyway.

## tasks

- Check the compatibility with canSAS
- Discussion on the necessity of transforming in both ways
- Collect arguments for or against the common structure (what does it
  mean for programmers? what about possible future formats?)

## decission

- Do we insist on an attribute-free representation of the data?

# Links to canSAS

the vocabulary and partially also the structure is copied from
canSAS where ever possible.

## tasks

- Take the canSAS dictionary and strip from all non-reflectemetry content.
  (A. Nelsen volonteered for this)


# Steep vs. flat hirarchical structure

There are various ways to represent physical quantities and related parameters. 
As an example the wavelength of the incident radiation was discussed. 

(Suggestions not compatible with YAML or HDF are not shown here.)

The hirarchical approach:

    wavelength:
        value:            for a monochromatic beam
        min:              minimum for a wavelength range
        max:              maximum for a wavelength range
        resolution:
            constant:     Delta lambda (same unit as value)
            proportional: Delta lambda / lambda (no unit!)
            type:         declaration of Delta: FWHM, sigma ....
        unit:             either nm or angstrom

which allows for a compact notation:

    wavelength:{min:4, max:12, resolution:{constant:0.1}, unit: nm }

The flat apprach:

    wavelength:                         for a monochromatic beam
    wavelength_min:                     minimum for a wavelength range
    wavelength_max:                     maximum for a wavelength range
    wavelength_resolution_constant:     Delta lambda
    wavelength_resolution_proportional: Delta lambda / lambda
    wavelength_unit:                    either nm or angstrom

## tasks

- Collect arguments for one or the other approach.

## decission

- Steep or flat hirarchy?

# Multiplicity of hirarchies

## discussion

The level of *reduction* is represented in the various possible
locations in the data tree where information is stored.

E.g. for a monochromatic beam it is sufficient to supply the wavelength information
(value, resolution and unit) only once. The *right* place is the 
`experiment/measurement` section. But for a energy-dispersive 
set-up the wavelength might be given for each measurement point in
and extra column. 

Below several examples of hirarchical structures are given for
various measurement schemes and levels of reduction. Shown are only the
entries relevant for this discussion.

### monochromatic, angle-dispersive scheme

The wavelength and its resolution are given only once because
they are constant. But the `angle` points to the column where
the individual values are given. These allow for footprint correction.

```YAML
experiement:
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
experiement:
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
experiement:
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

Data are rebinned and histogrammed to a (given) `Qz` grid. 
Resolution information is partially lost.

The universal `wavelength` and the raw file specific
`angle` are descriptive. They can hardly be used
for data analysis.

```YAML
experiement:
    measurement:
        scheme: angle- and energy-dispersive
        wavelength: {min: 3.0, max: 12.0, spread: 0.1, unit: angstrom}
reduction:
    input files:
        - file: data1
          angle: (min: 0.6, nax: 2.0, unit: deg}
        - file: data2
          angle: (min: 2.4, nax: 3.8, unit: deg}
columns:
    - {column: 1, variable: Qz, unit: 1/angstrom}
    - {column: 2, variable: R}
    - {column: 3, variable: sR)}
    - {column: 4, variable: sQz, unit: 1/angstrom}
```

### angle- and energy-dispersive, various angular settings, detailed

Data might be rebinned, but are not histogrammed. I.e. there might be 
several entries with (about) the same `Qz`, but with different `wavelength` and
`angle` and thus `sQz`.

The analysis software might use either `sQz` or the `(wavelength, angle)`
tuple, where the resolution information for both is given in the universal
part. Besides this, the universal `wavelength` and the raw file specific
`angle` are descriptive and not ment for data analysis.

```YAML
experiement:
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
          angle: (min: 0.6, nax: 2.0, unit: deg}
        - file: data2
          angle: (min: 2.4, nax: 3.8, unit: deg}
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

## decission
  
# Binary representation

For the binary representation HDF5 seems to be the favoured 
*format*. But there is a discussion whether or not to use / allow attributes.

This representation is ment ....

canSAS uses attrributes, but they are not compatible with 
YAML, ....

## tasks

- The attribute-free HDF5 might contradict the orso decission to stay 
  compatible with canSAS and Nexus. This has to be checked.

## decission

- Do we insist on / recommend HDF5 for the binary representation?
- If so: with or without attributes?

# ASCII representation

For the ASCII representation it was suggested to use wrapped YAML.
Meaning that the header is structured with YAML and that the body consists of
a table. The column description is (the last) part of the 
header.

This *format* is compatible with most analysis software and plotting
programs so that it allows for fast data visualisation on almost any
system and it can be used for simple or standard data reduction. 
Also by unexperienced users.

## tasks

- There are representations for the data equivalent to YAML (in terms of
  hirarchy and content, not necessarily in terms of human readability).
  Which is the *best*?

## decission

- Do we insist / recommend to use wrapped YAML for the ASCII representation?

# Units

## discussion: How do we write reciprocal and non-SI units?

The following table shows the physical dimensions typically used in reflectometry 
with the *accepted* units. The
4 last columns contain suggested shortcuts for the units to be used by ORSO: the very
short notoation, the short notation, the full word and a LaTeX type notation. The 
bold notations are those more or less recomended during the September workshop.

| dimension         | unit         |            | short     | long             | LaTeX-like | 
| :---------------- | :----------- | :--------: | :-------: | :--------------: | :--------: |
| length            | meter        | **`m`**    |           |                  |            |
|                   | milimeter    | **`mm`**   |           |                  |            |
|                   | nanometer    | **`nm`**   |           |                  |            |
|                   | Aangstroem   | `A`        | `Aa`      | **`angstrom`**   | `\AA`      |
| reciprocal length | 1/nanometer  | **`1/nm`** |           |                  | `nm^{-1}`  |
|                   | 1/Aangstroem | `1/A`      |           | **`1/angstrom`** | `\AA^{-1}` | 
| angle             | radian       | `1`        | **`rad`** | `radian`         |            |
|                   | degree       |            | **`deg`** | `degree`         | `^o`       |
| time              | second       | **`s`**    | `sec`     |                  |            |

There is an obvious difference between strict SI units and related units. The Si units
have a short, well defined and accepted notation. I.e. the discussion should concentrate on
angstrom (or so).

## discussion: Where are units declared?

There are 2 principle approaches: 

- Units are declared once at the beginning of the header and then apply to
  all quantities with the same dimension.
- Units are given for every quantity individually.

In the first case a conflict might arrise between
the slit size given in mm and the wavelength in nm.

The second approach has the advantage that there is no need to
collect the consistuents of a physical quantity from various
places in the document. 

## defaults

If no unit is given it is assumed to be `1`. Thus no `unit` must be defined
for a ratio like `R` or an angle in radian (for `1 rad := 1`). 

## tasks

- What does canSAS say?
- Are there other established notations?

## decissions

- Do we define the units once for all the document, or are they given with 
  every physical quantity?
- Do we accept the selection (bold notations) in the table above?

# Internal links

In the exampes in section [Multiplicity of hirarchies] there is a link
of the type `column: <value>` in case there is an extra column in the
table representing the respective quantity.

This link can be used by the analysis software to use the content of this column.

It is also a way to give extra information to the data in the column by
providing the resolution type and value.

## tasks

- This looks good in the YAML representation, but is it compatible
  with hdf?
- What is the programmers view?
- Are there other, more intuitive or compatible ways to realise this?

## decission

- are docuement-internal links allowed?
- How are they realised?
   
# Non-standard entries and comments

## non-standard entries

It might be helpful for the user to add extra, non-standard information
to facilitate reading the docuemnt and e.g. identifying the raw file
and settings.

An example is the sample alignment given together with the raw file
name (which often is just a non-descriptive number).

The problem is that the key-words introduced for this purpose might 
conflict with future standardised key words.

Possible solution: non-standard key words are marked as comment by
a prefix. Any ideas?

## comments

Is there a way to comment out lines?

What about a free-form text section of the form

```YAML
comment: |
    Attention! These data have been collected without the frame overlap mirror
    and contain thus some highly structured background.

    Still the peak positions can be analysed.
```

# misc.

## string vs. float

When using an x-ray reflectometer, it is more exact to mention the cathode and line
rather than the wavelength. Eg. `CuKa` or `CuKa1`. For this also includes the resolution.

Is it possible to use

```YAML
    wavelength: {value: CuKa}
```

or 

```YAML
    wavelength: {value: 1.54184, unit: angstrom}
```

without specifying the data type?
Or is it better to create an own key like

```YAML
    wavelength: {cathode: CuKa}
```

## tasks

## decission
