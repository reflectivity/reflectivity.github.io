---
layout: page
title: "ORSO file formats & data analysis: simplified model description"
permalink: /projects/simple_model
author: "Jochen Stahn"
---

Jochen Stahn, Artur Glavic, *Paul Scherrer Institut, Switzerland* <br>
Brian Maranville, *NIST, USA* <br>
2022-03-09

---

## simplified model description for .ort files

A joint project by the *file formats* and the *data analysis* working groups.

Based on slack discussions and online meetings, Artur and Jochen suggest the following 
approach for a **simple** and **flexible** way to define a first step sample model.

All previous suggestions and variants are removed in order to avoid confusion.

### aims

##### experiment planning

The model might be used to estimate counting times, statistics and experimental 
settings already before and during the experiments.

##### completeness of reflectivity file

The reflectivity file can be related to a sample model without the *external* connection
sample name - stack (in the best case this can be found in the log-book, else in the
manufactorers lab journal....)

##### data analysis

The standadisation allows the analysis software to automatically create a starting model which is 
not too far from the real one.

##### indexing of data and analysis

A standadised model might be used for indexing and filing of the data - within the lab or on a more general 
scale. This might be used to train AI algorithms.



### structure

The sample description consists of 1 to ??? entries in the ???:

``` YAML
    model:
        stack:        string
        origin:       string
        global:       list
        sub_stacks:   list
        materials:    list
        reference:    string
```

#### stack

The simplified model description is given in one line. This allows for a very compact notation,
but restricts the content to the absolute minimum. 

rules:

- entries in the `stack` are separated by the pipe symbol `|`
- according to the first axiom of optics, the beam enters from the left. I.e. the backing medium is the last entry in the `stack`
- repeating substacks are marked with `<number> ( ... )`
- each entry has a *name* and probably an assosiated thickness. These are separated buy one or more spaces.
- the default thickness unit is `nm`

examples:

- `stack: air | Ni 100 | SiO2 0.5 | Si`
  The standard 1000 angstrom Ni film to test the resolution
- `stack: air | 25 ( Si 7 | Fe 7 ) | Si`
  A polarising multilayer with 25 repetitions of 70 angstrom Fe and 70 angstrom Si. No information about the magnetic induction is given on this level.
- `stack: Si | SiO2 0.5 | lipid_multilayer | water`
  A lipid multilayer in a solid-liquid cell. No details about the organic film are given on this level. To allow for automated processing, further information must be provided in the `sub_stack` section or in a data base.

#### global

**wrong key word since it is alreade used in python and might cause confusion**

The `global` section allows to (re-)define model parameters or units which apply to the full `stack` and if applicable also to the following sections `sub_stacks`, `layers` and `materials`.

Unless overwritten, the following default values are used:

``` YAML
    global:
        length_unit: nm
        roughness: 0.5
        mass_density_unit: g/cm^3
        number_density_unit: 1/angstrom^3
        sld_unit: 1/angstrom^2
        m_moment_unit: muB
```

#### sub_stacks

Each sub_stack is made up of one or several layers. It has a unique name which is used to relate the substack to an entry in the `stack`.

``` YAML
    sub_stacks:
    - name: <>
    - stack: <>
```

#### layers

Each layer has a unique name which relates it to an entry in one of the `stack` s. 
Optional entries are:

- `thickness`<br>
  this overwrites the thickness given in the `stack`.
- `roughness`
- `magnetic_induction`
- `sld`
- `composition`
  A list of `materials` and probably the relative density.
  e.g.
  
  - solvent mixture:<br>
  
        composition:
        - {H2O, 0.4}
        - {D2O, 0.6} 
   
  - reduced density (voids, coverage, ...)
  
        composition:
        - {Ni, 0.95} 
        
- ...


#### materials

Each material has a unique name which relates it to an `layer.composition` entry.

- `name`
- `formula`
- `sld`
- `mass_density`
- `formula_number_density`
- `magnetic_induction`
- `rel_density`


#### origin

#### reference

#### data_base


### arguments

- This structure allows to enter the same (or contradicting) information at various levels. E.g. the `thickness` can be defined in the `stack` and the `layer`. This is not nice for programming and might be a source of errors, but on the oter side it allows for a very compact and human readable notation.
- The `composition` enables an easy way to define mixtures (solvents, interdiffusion, absorption).   

### conflicts

- in `stack air | Fe 5 | Si` the Fe can mean 

  - name of a sub_stack 
  - name of the layer
  - name of the material
  - chemical composition
  
  Naturaly on might call the material iron `Fe` in order to simplify searching the data base. 
  But the material has no `thickness`, thus also the `layer` should be named Fe to avoid a non-intuitive name in the `stack`.

### vocabulary

There is a wide variety of meanings associated with key words such as *layer* or *material*. 
It will be quite complicated to find an agreement here, because most of us will have to 
accept an *absurd* choice at some point.

- `sample` already defined
- `model`
- `origin` provides some informaton about how trustworthy the model is (*guess* vs. *based on XRD*) 
- `stack`
- `layer`
- `material`
- `roghness`
- `composition`
- `density` (which one?)
- `mass_density`
- `SLD`, `sld`
- `origin`
- `magnitude`
- `unit`
- `schema`




### definitions & rules

- local coordinate system



rules e.g. 

- both, 'lower' and 'upper' medium have to be defined

### examples



``` YAML
    sample:
        model:
            origin:     guess based on preparation
            stack:      Si | 10 ( Fe 7 | Si 7 ) | air
```

extended version (with more information) on the level of the starting model for data analysis:

``` YAML
    sample:
        model:
            origin:     guess based on preparation / XRR
            stack:      Si | 10 ( Fe 7 | Si 7 ) | air
            materials:          
            - name:         Fe
              moment: 
                  magnitude:    2.2 
                  unit:     muB
              sld: 
                  magnitude:    5.02e-6
                  unit:     1/angstrom^2
            - name:         Si
              composition:  SiN0.01
              rel_density:  0.95
              ...
           global:
              roughness:
                  magnitude:     5
                  unit:      amgstrom 
           reference: ORSO model language | 1.0 | http://bla.bli
```

or a quite complicated model to illustrate what is possible:

``` YAML
sample:
    model:
        origin: guess by J. Stahn
        stack:  sub | film | water
        sub_stacks:
        - name: sub
          layers: 
             - material: SiO2
               thickness: {magnitude:5, unit: angstrom}
               sigma: {magnitude: 3, unit: angstrom}
             - material: Si
               sigma: {magnitude:2, unit: angstrom}
        - name: film
          repetitions: 5
          stack: head_group 4 | tail | tail | head_group 4
        layers:
        - name: tail
          material: tailstuff
          thickness: 22.
        materials: 
        - name: water
          composition: H2O 0.3, D2O 0.7          
        - name: head_group
          sld: {magnitude: 0.2e6, unit: angstrom^-2}
        - name: tailstuff
          formula: CH2
          mass_density: {magnitude: 1.2, unit: g/cm^3}
        - name: SiO2
          formula: SiO2
        - name: Si
          formula: Si
        global:
          sigma: {magnitude: 5, unit: angstrom}
          length_units: angstrom
          mass_density_units: g/cm^3
        reference: ORSO model language | 1.0 | http://bla.bli
```

Here `film` referes to a stack with 5 repetitions of some organic bilayer, which in turn consists of 4 sublayers. These are defined either again as layer (here for the tails) or directly with a thickness and a material. The `materials` section allows to define the materials used above. When missing, the name is taken as the chemical formula (e.g. Si or SiO2) or as a pre-defined material (water, air) and the corresponding values are taken from a data base.

This structure is loosely based on the model description language created by Petr Mikulik for his x-ray fitting program EDXR.

An alternative notation using list instead of sub-elements:

``` YAML
sample:
    model:
        origin: guess by J. Stahn
        stack:  sub | film | water
        items:
        - type: sub_stack 
          name: sub
          layers: 
             - material: SiO2
               thickness: {magnitude:5, unit: angstrom}
               sigma: {magnitude: 3, unit: angstrom}
             - material: Si
               sigma: {magnitude:2, unit: angstrom}
        - type: sub_stack
          name: film
          repetitions: 5
          stack: head_group 4 | tail | tail | head_group 4
        - type: material
          name: water
          composition: H2O 0.3, D2O 0.7          
        - type: layer
          name: tail
          material: tailstuff
          thickness: 22.
        - type: material
          name: head_group
          sld: {magnitude: 0.2e6, unit: angstrom^-2}
        - type: material
          name: tailstuff
          formula: CH2
          mass_density: {magnitude: 1.2, unit: g/cm^3}
        - type: material
          name: SiO2
          formula: SiO2
        - type: material
          name: Si
          formula: Si
        reference: ORSO model language | 1.0 | http://bla.bli
```

These examples show how a model might be declared. There are various ways to do so for exactely the same model, and the choice depends mainly on the human readability and on *logical units* (like POPC). For automated writing (e.g. as an output from the data analysis software), we will have to find a reasonable and programmable approach.... 
