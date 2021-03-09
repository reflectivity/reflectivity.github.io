---
layout: page
title: ORSO file format - tasks - Units
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-08
---

## How do we write reciprocal and non-SI units?

The following table shows the physical dimensions typically used in reflectometry 
with the *accepted* units. The
4 last columns contain suggested shortcuts for the units to be used by ORSO: the very
short notation, the short notation, the full word and a LaTeX type notation. The 
bold notations are those more or less recommended during the September workshop.

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

## Where are units declared?

There are 2 principle approaches: 

- Units are declared once at the beginning of the header and then apply to
  all quantities with the same dimension.
- Units are given for every quantity individually.

In the first case a conflict might arise between
the slit size given in mm and the wavelength in nm.

The second approach has the advantage that there is no need to
collect the constituents of a physical quantity from various
places in the document. 

## defaults

If no unit is given it is assumed to be `1`. Thus no `unit` must be defined
for a ratio like `R` or an angle in radian (for `1 rad := 1`). 

## tasks

- What does canSAS say?
- Are there other established notations?

## decisions

- Do we define the units once for all the document, or are they given with 
  every physical quantity?
- Do we accept the selection (bold notations) in the table above?


