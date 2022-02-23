---
layout: page
title: "ORSO file formats & data analysis: simplified model description"
permalink: /projects/simple_model
author: "Jochen Stahn"
---

Jochen Stahn, Artur Glavic, *Paul Scherrer Institut, Switzerland* <br>
Brian Maranville, *NIST, USA* <br>
2022-02-22

---

## simplified model description for .ort files

A joint project by the *file formats* and the *data analysis* working groups.

This is a collection of ideas for how to describe a reflectometry sample in a **simple**
and **flexible** way.
This might be a first step towards a general and comprehensive sample description language.

### aims

##### experiment planning

The model might be used to estimate counting times, statistics and experimental 
settings already before and during the experiments.

##### completeness of reflectivity file

The reflectivity file can be related to a sample model without the *external* connection
sample name - stack (in the best case this can be found in the log-book, else in the
manufactorers lab journal....)

##### data analysis

The standadisation allows the analysis software to iautomaticlly create a starting model which is 
not too far from the real one.

##### indexing of data and analysis

A standadised model might be used for indexing and filing of the data - within the lab or on a more general 
scale. This might be used to train AI algorithms.



### structure

This section is about *compactness vs. flexibility* of the model description. 

#### the 1-liner

The complete model description is given in one line. This allows for a very compact notation,
but restricts the content to the absolute minimum. 

For each layer the name or composition, its thickness and probably some additional information is provided. 
Tor keep this readable, roughnesses and magnetic information can hardly be provided.

The structure will look like

``` YAML
    sample:
        stack: <model string>
```

Where the `<model string>` might look like one of the following suggestions for the
example

|               | name | SLD  | thickness | **M** | repetitons |
| :------------ | :--- | ---: | --------: | ----: | :--------: |
|  outer medium | water |      |           |       |            |
|  layer 4      | POPC |  0.4 |       2.0 |       |            |
|  layer 3      | Ni   |      |      70.0 |       | 5          |
|  layer 2      | Fe   |      |      30.0 |   0.4 | "          | 
|  layer 1      | SiO2 |      |       0.5 |       |            |
|  substrate    | Si   |      |           |       |            |

```
        stack: Si | SiO2 0.5 | 5 (Fe 30 | Ni 70) | POPC 2 | water     
        stack: [Si, SiO2 0.5, [Fe 30, Ni 70]*5, POPC Bilayer, water]                      
        stack: [Si, [SiO2, 0.5], [5, [Fe, 30], [Ni, 70]], [POPC, 0.3], water]             
        stack: [Si, SiO2: 0.5, ml: [Fe: 30, Ni: 70], 5, POPC: 0.3, water]                     
        stack: [Si, {SiO2: 0.5}, {ml: 5}, {Fe: 30}, {Ni: 70}, {POPC: 0.3}, water]    
```

pro:

- in most cases, there is not much more information available prior the the data analysis;
- this is close to the notation chosen for log-book entries (at least to Jochen's experience).

contra:

- SLD, magnetic moment or roughness can not be provided without spoiling the readability even more. 
- the handling of units is not nice (there is none);
- pre-defined units might lead to confusion (anstrom in the orso header, nm in the model);
- more than about 6 layers leads to too long lines.
- It is not clear what the numbers mean unless one reads the manual....

---

#### a listing

Each line represents one layer. 

``` YAML
    sample:
        stack:
         - water , inf nm , 0.0 /fm^2 , 0.0 muB
         - POCP  , 0.3 nm , 0.4 /fm^2 , 0.0 muB
         - 5
           - Ni  , 3.5 nm , calc      , 0.0 muB
           - Ti  , 7.0 nm , calc      , 0.0 muB
         - SiO2  , 0.5 nm , calc      , 0.0 muB
         - Si    , inf nm , calc      , 0.0 muB
```

where `calc` means that the value is calculated from the formula = name.

Of cause also here one can think of various ways to structure the layer lines....

pro: 

- the table-like appearance is easy to read
- more information can be provided
- a large number of lyers does not affect readability

contra:

- entering this information *by hand* is quite susceptible to errors;
- there is redundand information (the chemical composition can already contain the info about the SLD).

---


 
#### hirarchical structure

A hirarchical approach, where a one-line string tells the layer sequence with 
name or formula and thickness of each layer in a 
very compressed way. 
All extra information can be provided in a following layer description:

Examples (Jochen):

``` YAML
    sample:
        stack: Si | SiO2 0.5 | 5 (Fe 30 | Ni 70) | POCP 2 | water
        layer_description:
         - layer: SiO2
           relative_density: 0.95
           lower_roughness: 5.0
           upper_roughness: 3.0 
         - layer: POCP
           SLD: 
                magnitude: 0.3e-6
                unit: fm
         - layer: Fe
           magnetic_moment:
                magnitude: 0.4
                unit: muB
           magnetisation_axis: 0.9, 0.1, 0.0
```

or (Artur)
`layers` describes the sequence of names and thicknesses in brackets, 
for `Rep 1` the brackets give the number repetitions

``` YAML
     sample:
         structure:
             layers: Air | Layer 1 (135) | Layer 2 (400) | Rep 1 (5) | Layer 3 (25) | Substrate
```

`materials` desribe the physical parameters in various forms given by the name used in layers
the name has to be unique but can be used at different locations in the sequence        

``` YAML
         materials:
           - name: Air
             sld: 
               [0., 0.]
           - name: Layer 1
             sld: 
               magnitude: [5.6e-6, 0.]
               unit: angstrom^-2
           - name: Layer 2
             sld:
               magnitude: [3.5e-6, 0.]
               unit: angstrom^-2
           - name: Layer 3
             composition: Fe2O3
             mass_density: 
               magnitude: 5.24
               unit: g/cm^3
           - name: Substrate
             composition: Si
             fu_density: 
               magnitude: 0.04996026
               unit: 1/angstrom^3
           - name: Rep 1
             layers: Layer 1 (50) | Layer 2 (40)
         thicknesses_unit: angstrom
```

pro:

- the first entry gives an overview and contains most of the information, only special
information has to be provided in a more complicated form;
- one can add more and more complex information up to a full description (e.g. after analysis)

contra:

- extra information is again difficult to enter *by hand*;
- complicated samples lead to a too long first line.

---


#### various formats

A grammar for the model description might be chosen by an additional variable:

``` YAML
    sample:
        model:
            schema:     orso-short-notation 
                        Brian's-JSON-nightmare
                        etc.
            stack:      ....   
```

pro:

- maximum flexibility

contra:

- maximum confusion

---

### open questions

- Should the model be formatted according to YAML or JSON rules? (The one-liner then will consist essentially of brackets, colons and comas.)
- Should the simplified model description we aim for here be 100% compatible with a unversal model language? Probably using the same grammar? 
- Where is the model analysed? Within orsopy or only in the analysis programs?
- One or several languages?

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
- the beam enters from the top of the listing, or the right side of a one-line description

---

### Jochen's suggestion

minimal version (3 lines for the model) to estimate the outcome of the measurement as a starting point for a detailed model for analysis. Magnetisation is missing:

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
                  unit:      angstrom 
            reference: ORSO model language | 1.0 | http://bla.bli
```

or (quite complicated)

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

With alternative notation using list instead of sub-elements:
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

Here `film` referes to a stack with 5 repetitions of some organic bilayer, which in turn consists of 4 sublayers. These are defined either again as layer (here for the tails) or directly with a thickness and a material. The `materials` section allows to define the materials used above. When missing, the name is taken as the chemical formula (e.g. Si or SiO2) or as a pre-defined material (water, air) and the corresponding values are taken from a data base.

This last example is loosely based on the model description language created by Petr Mikulik for his x-ray fitting program EDXR.
