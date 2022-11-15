---
layout: page
title: "ORSO file formats & data analysis: simplified model description"
permalink: /projects/simple_model
author: "Jochen Stahn"
---

Jochen Stahn, Artur Glavic, *Paul Scherrer Institut, Switzerland* <br>
Brian Maranville, *NIST, USA* <br>
2022-03-14

---

## simplified model description for .ort files

This is a joint project by the *file formats* and the *data analysis* working groups.

Based on slack discussions and online meetings, Artur and Jochen suggest the following 
approach for a **simple** and **flexible** way to define a first step sample model.

This document is not free of uncertainties or contradictions! We will 
correct this according to the state of the discussion.

All previous suggestions and variants are removed in order to avoid confusion.

### aims

<dl>
    <dt> experiment planning </dt>
    <dd> The model might be used to estimate counting times, statistics and experimental 
         settings already before and during the experiments.</dd>
    <dt> completeness of reflectivity file </dt>
    <dd> The reflectivity file can be related to a sample model without the *external* connection
         sample name - stack (in the best case this can be found in the log-book, else in the
         manufactorers lab journal....).</dd>
    <dt> data analysis </dt>
    <dd> The standadisation allows the analysis software to automatically create a starting model which is 
         not too far from the real one.>/dd>
    <dt> indexing of data and analysis </dt>
    <dd> A standadised model might be used for indexing and filing of the data - within the lab or on a more general 
         scale. This might be used to train AI algorithms.</dd>
</dl>

### complexity

This langage allows to provide a simple model description on only 2 lines:

> ``` YAML
>     model:
>         stack: air | Ni 100 | SiO2 0.5 | Si
> ```  

More complexity to the modle (e.g. magnetic induction) can be provided by adding a few more lines. 
The key words used in the *stack* (here `air`, `Ni`, `SiO2` and `Si`) either refere to a data base 
or to declaration within the model. This declaration is quite flexible so that an detailed 
model for a complex sample might reach up to 100 lines.
The idea is that recuring entries are defined in detail and strored in a local data base. The 
model description in the header then stays simple and easy to use.


### structure

The model description has the following structure:

``` YAML
sample:
    model:
        stack:        string    mandatory
        sub_stacks:   dict      optional
        layers:       dict      optional
        compositions: dict      optional
        materials:    dict      optional
        globals:      dict      optional
        reference:    string    optional
        schema:       string    optional
        databases:    list of strings    optional
        origin:       string    optional
```

The information is organised according to YAML rules but for the *stack* string(s). 
The reason is to make it simple and less error-prone to enter this information *by hand*. 


#### stack

The simplified model description is given in one line. This allows for a very compact notation,
but restricts the content to the absolute minimum. 

Rules:

1. According to the first axiom of optics, the beam enters from the left. I.e. the backing medium is the last entry in the `*stack*.
1. Entries in the *stack* are separated by the pipe symbol `|`.
1. Repeating *sub_stacks* are marked with `<number> ( ... )`.
1. Each entry has a *name* and probably an assosiated *thickness*. These are separated by one or more spaces.
1. The default thickness unit is `nm`.

(These rules allow to expand the *stack* string into a YAML compliant *sequence*)

> Examples:
> 
> - The standard 1000 angstrom Ni film to check the resolution:
> 
>   ``` YAML
>       stack: air | Ni 100 | SiO2 0.5 | Si
>   ```  
>   
> - A polarising multilayer with 25 repetitions of 70 angstrom Fe and 70 angstrom Si:
>   
>   ``` YAML
>       stack: air | 25 ( Si 7 | Fe 7 ) | Si
>   ```
>
>   No information about the magnetic induction is given on this level.
>   
> - A lipid multilayer in a solid-liquid cell:
> 
>   ``` YAML
>       stack: Si | SiO2 0.5 | lipid_multilayer | water
>   ```
>   
>   No details about the organic film are given on this level. 
>   To allow for automated processing, further information must be provided in the `sub_stack` section or in a data base.

<!---
> Expansion of the *stack*
>
>   ``` YAML
>       stack: air | 25 ( Si 7 | Fe 7 ) | Si
>   ```
>
> into a YAML compliant *sequence*: 
>
>   ``` YAML
>       sequence:
>         - layer:
>             name: air
>         - sub_stack:
>             repetitions: 25
>             sequence:
>               - layer: 
>                   name: Si
>                   thickness: 7
>               - layer: 
>                   name: Fe
>                   thickness: 7
>         - layer:
>             name: Si
>   ```
--->


#### sub_stacks

Each substack is made up of one or several layers. It has a unique name which is used to relate the substack to an entry in the *stack*. The purpose of this dictionary is to enable a simple *stack* for complicated models and to provide multy-layer building blocks in data bases.

``` YAML
    sub_stacks:
      <name>: 
        repititions: 
                        int
                        optional  (default: 1)
                        defines how often the substack is repeated. A negaive value means an inversion of the order.
        stack:       
                        str
                        optional
                        Same rules as for the top-level stack apply.
        sequence:         
                        list
                        optional
                        Instead of an other stack, the layers and their sequence are defined.  
```

> Examples:
>
> - expansion of the physical (chemical) unit *lipid multilayer* into a sequence of *layers*:
>
>   ``` YAML
>       sub_stacks:
>         lipid_multilayer:
>           repetitions: 4
>           stack: head | tail | tail | head    
>   ```
>
> - the same layer sequence, but assembled hirarchically stating with chemical units: 
>
>   ``` YAML
>       sub_stacks:
>         lipid:
>           sequence:
>             - material: headstuff
>               thickness: 0.5  
>             - matrial: tailstuff
>               thickness: 2.2  
>         lipid_inverse:
>           repetitions: -1
>           stack: lipid
>         lipid_bilayer:
>           stack: lipid | lipid_inverse
>         lipid_multilayer:
>           repetitions: 4
>           stack: lipid_bilayer
> ```


#### layers

If information about a layer besides its chemical composition (and thus the density form a data base) and its thickness is needed, this has to be defined here. A *layer* has homogeneous properties. I.e. no density gradients (might be a future option) or separation into sub-layers (that is covered by `sub_stack`).

``` YAML
    layers:
      <name>:
                      Each layer from the layers list has a unique name which 
                      relates it to an entry in one of the stacks. 
                      Layers defined in a sub_stack list are only used once 
                      and do not require naming.
        thickness:
                      This overwrites the thickness given in the stack
        roughness:
        material:
                      Either a name of a material or dictionary with 
                      material parameters. See below.
```

> `material` examples:
> 
> - simple case, reference to the `materials` list or a data base
>   
>   ``` YAML
>       layers:
>         iron:
>           material: Fe
>   ``` 
>  
> - a bit more detailed, therefor no internal reference
>   
>   ``` YAML
>       layers:
>         iron:
>           material: {formula: Fe, magnetic_moment: 2.4, mass_density: 6.8}
>   ```
>     
> - reduced density (voids, coverage, ...)
>   
>   ``` YAML
>       layers:
>         nickel:
>           material:
>             composition:
>                Ni: 0.95
>           thickness: 7.5     
>   ```   

#### composits

A composit behaves like a material, but is made up from (on or) several materials with respective relative densities.
It enables an easy way to define mixtures (solvents, interdiffusion, absorption).   

``` YAML
    compositis:
      <name>:
        <material 1>: <rel. density 1>
        <material 2>: <rel. density 2>
``` 
 
> Example:
>  
> ``` YAML
>     composits:
>       solvent:
>          cyclohexane: 0.6
>          toluene: 0.4
> ```

#### materials

Each material has a unique name which relates it to an `layer.composition` entry or referenced in a `stack`.

``` YAML
    materials:
      <name>:
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
        deuteration:
                            For materials containing hydrogen this parameter allows
                            to tune the deuteration level while keeping the number
                            density constant. 
                            Details have to be figured out....
```

> Examples:
>
> - magnetic moment for iron
>
>   ``` YAML
>       materials:
>         Fe
>           magnetic_moment: 2.2
>   ```
> 
> - clear names for solvents where the formula might be ambiguous:
>
>   ``` YAML
>      materials:
>        cyclohexane
>          formula: C6H12
>          mass_density: 0.778
>        toluene
>          formula: C7H8
>          mass_density: 0.87
>   ```

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


#### reference

A string defining the model language and version to be used to interpret the data.<br>

> Example:
> 
> ``` YAML
>     reference: ORSO model language | 1.0 | https://www.reflectometry.org/projects/simple_model
> ```

#### schema:

``` YAML
    schema: <URL>
```

#### databases:

This is a list of places where to look for information about materials or pre-defined substacks. These places are searched for an *unknown* string in the `stack` in the order they are listed. The search is stopped after the first hit. I.e. one can overwrite the ORSO SLD database entry by a local definition.

``` YAML
    databases:
     - './model.def'
     - '/home/amorlnsg/orso/model/'
     - 'https://slddb.esss.dk/slddb/'
```

> Example:
>
> ``` YAML 
>     stack: Si | my_bilayer | H2O
>     databases:
>      - './model.def'
>      - ....
> ```
>
> points to the entry `my_bilayer` in `model.def`:
> 
>  ``` YAML
>       substacks:
>         my_bilayer:
>           sequence:
>             - SLD: 1.23
>               thickness: 0.5  
>             - matrial: CH2
>               thickness: 2.2
>               mass_density: 0.83
>             - matrial: CH2
>               thickness: 2.2
>               mass_density: 0.83
>             - SLD: 1.23
>               thickness: 0.5  
>           origin: Best guess by J. Stahn. Not trustworthy!
> ```

where `CH2` is treated as a formula and the corresponding parameters are caught from the ORSO SLD database. The head groups are anonymous.


#### origin

A string to declare where the model parameters come from.

> Example:
> ``` YAML
>     origin: thicknesses based on XR mesurements, nominal compositions
> ```


---

<!---
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
- `composit` material mixture 
- `roghness` sigma of the errorfunction describing the density variation at an 'interface'
- `composition`
- `density` (which one?)
- `mass_density`
- `SLD`, `sld`
- `origin`
- `magnitude`
- `unit`
- `schema`
- `CAS` Chemical Abstracts Service number
--->


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
              Fe
                magnetic_moment: 2.2 
                sld: 5.02e-6
              Si
                formula: SiN0.01
                rel_density: 0.95
           globals:
              length_unit: angstrom
              m_moment_unit: muB
              roughness: 5
              sld_unit: 1/angstrom^2
           reference: ORSO model language | 1.0 | http://bla.bli
```

or a quite complicated model to illustrate what is possible:

``` YAML
sample:
    model:
        origin: guess by J. Stahn
        stack: substrate | film | water
        sub_stacks:
          substrate:
            sequence: 
               - material: Si
                 sigma: 2
               - material: SiO2
                 thickness: 5
                 sigma: 3
          film:
            repetitions: 5
            stack: head_group 4 | tail | tail | head_group 4
        layers:
          tail
            material: tailstuff
            thickness: 22.
        compoisits:
          water: 
              H2O: 0.3
              D2O: 0.7   
        materials:       
          head_group:
            sld: 0.2e6
          tailstuff:
            formula: CH2
            mass_density: 1.2
          SiO2:
            formula: SiO2
          Si:
            formula: Si
        globals:
          sigma: {magnitude: 5, unit: angstrom}
          length_units: angstrom
          mass_density_units: g/cm^3
          sld_unit: 1/angstrom^2
        reference: ORSO model language | 1.0 | https://www.reflectometry.org/projects/simple_model
```

Here `film` referes to a stack with 5 repetitions of some organic bilayer, which in turn consists of 4 sublayers. These are defined either again as layer (here for the tails) or directly with a thickness and a material. The `materials` section allows to define the materials used above. When missing, the name is taken as the chemical formula (e.g. Si or SiO2) or as a pre-defined material (water, air) and the corresponding values are taken from a data base.

These examples show how a model might be declared. There are various ways to do so for exactely the same model, and the choice depends mainly on the human readability and on *logical units* (like POPC). For automated writing (e.g. as an output from the data analysis software), we will have to find a reasonable and programmable approach.... 

### Implementation and examples
The model desciption has been implemented as a .ort header item in a Pull Request to the orsopy package with options to resolve layers for software to easily build a model system from the specification. Header examples and some automatic plotting scripts are included, too.
See the [Pull Request](https://github.com/reflectivity/orsopy/pull/83) for more details.
