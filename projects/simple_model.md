---
layout: page
title: "ORSO file formats & data analysis: simplified model description"
permalink: /projects/simple_model
author: "Jochen Stahn"
---

Jochen Stahn, Artur Glavic, *Paul Scherrer Institut, Switzerland* <br>
Brian Maranville, *NIST, USA* <br>
2022-03-10

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

The model description has the following structure:

``` YAML
sample:
    model:
        origin:       string    optional
        stack:        string    mandatory
        sub_stacks:   list      optional
        layers:       list      optional
        materials:    list      optional
        globals:      dict      optional
        reference:    string    optional
```

#### origin

A string to declare where the model parameters come from.

#### stack

The simplified model description is given in one line. This allows for a very compact notation,
but restricts the content to the absolute minimum. 

Rules:

- Entries in the `stack` are separated by the pipe symbol `|`.
- According to the first axiom of optics, the beam enters from the left. I.e. the backing medium is the last entry in the `stack`.
- Repeating substacks are marked with `<number> ( ... )`.
- Each entry has a *name* and probably an assosiated thickness. These are separated buy one or more spaces.
- The default thickness unit is `nm`.

Examples:

- The standard 1000 angstrom Ni film to check the resolution:

  ``` YAML
      stack: air | Ni 100 | SiO2 0.5 | Si
  ```
  
- A polarising multilayer with 25 repetitions of 70 angstrom Fe and 70 angstrom Si:
  
  ``` YAML
      stack: air | 25 ( Si 7 | Fe 7 ) | Si
  ```
  
  No information about the magnetic induction is given on this level.
  
- A lipid multilayer in a solid-liquid cell:

  ``` YAML
      stack: Si | SiO2 0.5 | lipid_multilayer | water
  ```
  
  No details about the organic film are given on this level. 
  To allow for automated processing, further information must be provided in the `sub_stack` section or in a data base.

#### globals

The `globals` section allows to (re-)define model parameters or units which apply to the full `stack` and if applicable also to the following sections `sub_stacks`, `layers` and `materials`.

Unless overwritten, the following default values are used:

``` YAML
    globals:
        length_unit: nm
        mass_density_unit: g/cm^3
        number_density_unit: 1/nm^3
        sld_unit: 1/angstrom^2
        magnetic_moment_unit: muB
        roughness: 0.5
```

#### sub_stacks

Each sub_stack is made up of one or several layers. It has a unique name which is used to relate the substack to an entry in the `stack`.

``` YAML
    sub_stacks:
      - name:        str    mandatory
        repititions: int    optional      default: 1
        stack:       str
        layers:      list
```

#### layers

``` YAML
    layers:
      - name:
                      Each layer from the layers list has a unique name which 
                      relates it to an entry in one of the stacks. 
                      Layers defined in a sub_stack list are only used once 
                      and do not require naming.
        thickness:
                      This overwrites the thickness given in the stack
        roughness:
        material:
                      Either a name of a material or dictionary with 
                      material parameters. See below
        composition:
                      A dictionary of `materials` names and their relative 
                      density. See below
```

`material` examples:

- simple case, reference to the `materials` list or a data base
  
  ``` YAML
      material: Fe
  ``` 
 
- a bit more detailed, therefor no internal reference
  
  ``` YAML
      material: {formula: Fe, magnetic_moment: 2.4, mass_density: 6.8}
  ```
  
`composition` example:
  
- solvent mixture:
  
  ``` YAML
      composition:
        H2O: 0.4
        D2O: 0.6
  ```
    
- reduced density (voids, coverage, ...)
  
  ``` YAML
      composition:
        Ni: 0.95
  ```   



#### materials

Each material has a unique name which relates it to an `layer.composition` entry or referenced in a `stack`.

``` YAML
    materials:
      - name:
        formula:
        sld: 
                            The scattering length density for the radiation that was 
                            used in this experiment. 
                            (neutron or x-ray for given energy)
        mass_density:
        number_density:
        magnetic_moment:
        rel_density: 
                            The density is taken from tabulated bulk values and 
                            multiplied with this parameter.
```

Example:

``` YAML
    materials:
      - name: Fe
        magnetic_moment: 2.2
        mass_density: 7.87
        formula: Fe
```

Is the following recursion possible?

``` YAML
    materials:
      - name: cyclohexane
        formula: C6H12
        mass_density: 0.778
      - name: toluene
        formula: C7H8
        mass_sensity: 0.87
      - name: solvent
        composition:
          cyclohexane: 0.6
          toluene: 0.4
```




#### reference

A string defining the model language and version to be used to interpret the data.<br>
e.g.

``` YAML
    reference: ORSO model language | 1.0 | https://www.reflectometry.org/projects/simple_model
```

---

### arguments

- This structure allows to enter the same (or contradicting) information at various levels. E.g. the `thickness` can be defined in the `stack` and the `layer`. This is not nice for programming and might be a source of errors, but on the oter side it allows for a very compact and human readable notation.
- The `composition` enables an easy way to define mixtures (solvents, interdiffusion, absorption).   


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



### examples

``` YAML
    sample:
        model:
            origin: guess based on preparation
            stack: air | 10 ( Si 7 | Fe 7 ) | Si
```

extended version (with more information) on the level of the starting model for data analysis:

``` YAML
    sample:
        model:
            origin: guess based on preparation / XRR
            stack: air | 10 ( Si 70 | Fe 70 ) | Si
            materials:          
            - name: Fe
              magnetic_moment: 2.2 
              sld: 5.02e-6
            - name: Si
              formula: SiN0.01
              rel_density: 0.95
           globals:
              length_unit: angstrom
              m_moment_unit: muB
              roughness: 5
              sld_unit: 1/angstrom^-2
           reference: ORSO model language | 1.0 | http://bla.bli
```

or a quite complicated model to illustrate what is possible:

``` YAML
sample:
    model:
        origin: guess by J. Stahn
        stack: substrate | film | water
        sub_stacks:
          - name: substrate
            layers: 
               - material: Si
                 sigma: 2
               - material: SiO2
                 thickness: 5
                 sigma: 3
          - name: film
            repetitions: 5
            stack: head_group 4 | tail | tail | head_group 4
        layers:
          - name: tail
            material: tailstuff
            thickness: 22.
        materials: 
          - name: water
            composition: 
                H2O: 0.3
                D2O: 0.7          
          - name: head_group
            sld: 0.2e6
          - name: tailstuff
            formula: CH2
            mass_density: 1.2
          - name: SiO2
            formula: SiO2
          - name: Si
            formula: Si
        globals:
          sigma: {magnitude: 5, unit: angstrom}
          length_units: angstrom
          mass_density_units: g/cm^3
          sld_unit: 1/angstrom^2
        reference: ORSO model language | 1.0 | https://www.reflectometry.org/projects/simple_model
```

Here `film` referes to a stack with 5 repetitions of some organic bilayer, which in turn consists of 4 sublayers. These are defined either again as layer (here for the tails) or directly with a thickness and a material. The `materials` section allows to define the materials used above. When missing, the name is taken as the chemical formula (e.g. Si or SiO2) or as a pre-defined material (water, air) and the corresponding values are taken from a data base.

This structure is loosely based on the model description language created by Petr Mikulik for his x-ray fitting program EDXR.

These examples show how a model might be declared. There are various ways to do so for exactely the same model, and the choice depends mainly on the human readability and on *logical units* (like POPC). For automated writing (e.g. as an output from the data analysis software), we will have to find a reasonable and programmable approach.... 
