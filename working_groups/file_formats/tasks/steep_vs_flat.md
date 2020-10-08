---
layout: page
title: ORSO file format - tasks - Steep vs. flat hierarchical structure
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-08
---

# Steep vs. flat hierarchical structure

There are various ways to represent physical quantities and related parameters. 
As an example the wavelength of the incident radiation was discussed. 

(Suggestions not compatible with YAML or HDF are not shown here.)

The hierarchical approach:

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

The flat approach:

    wavelength:                         for a monochromatic beam
    wavelength_min:                     minimum for a wavelength range
    wavelength_max:                     maximum for a wavelength range
    wavelength_resolution_constant:     Delta lambda
    wavelength_resolution_proportional: Delta lambda / lambda
    wavelength_unit:                    either nm or angstrom

## tasks

- Collect arguments for one or the other approach.
  (Jochen Stahn)

## decision

- Steep or flat hierarchy?


