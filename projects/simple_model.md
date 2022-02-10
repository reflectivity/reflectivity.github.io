---
layout: page
title: ORSO file formats & data analysis: simplified model description
permalink: /projects/simple_model/
---

Jochen Stahn, Artur Glavic, *Paul Scherrer Institut, Switzerland* ,br>
Brian Maranville, *NIST, USA* <br>
2022-02-10

---

## simplified model description for .ort files

This is a collection of ideas for how to describe a reflectometry sample in a **simple**
and **flexible** way.
This might be a first step towards a general and comprehensive sample description language.

### aims

- **experiment planning** The model might be used to estimate counting times, statistics and experimental 
settings already before and during the experiments.
- **completeness of reflectivity file** The reflectivity file can be related to a sample model without the *external* connection
sample name - stack (in the best case this can be found in the log-book, else in the
manufactorers lab journal....)
- **data analysis** The standadisation allows the analysis software to create a starting model which is 
not too far from the real one.
- **indexing of data and analysis** A standadised model might be used for indexing and filing of the data - within the lab or on a more general 
scale. This might be used to train AI algorithms.

### structure

---

#### the 1-liner

The complete model description is given in one line. This allows for a very compact notation,
but restricts the content to the absolute minimum.

Example (Jochen):

``` YAML
    sample:
        stack: Si | SiO2 0.5 | Fe 30 0.4 | Ni 70 | pep 0.3 2 | air
```

which translates to (units undefined here!)

|               | name  | SLD | thickness | **M** |
| :------------ | :--- | ---: | ---: | ---: |
|  outer medium | air  |     |      |     |
|  layer 4      | pep  | 0.4 |  2.0 |     |
|  layer 3      | Ni   |     | 70.0 |     |
|  layer 2      | Fe   |     | 30.0 | 0.4 |
|  layer 1      | SiO2 |     |  0.5 |     |
|  substrate    | Si   |     |      |     |

The SLD is calculated from the *name* if that starts with a capital letter
else the 1st argument is taken as a SLD.
 
No roughnesses or density gradient films can be defined at this level. 
 
Repetitions can be marked as 

``` YAML
        stack: Si | 10 ( Ni 7 | Ti 7 ) | air
```

Alternative formats are

``` YAML
    sample:
        stack: Si | SiO2 5a | Fe 30a 0.4mub | Ni 70nm | pep 0.3/aa 2nm | air
```

pro:

- in most cases, there is not much more information available prior the the data analysis;
- this is close to the notation chosen for log-book entries (at least to Jochen's experience).

contra:

- the handling of units is not nice;
- pre-defined units might lead to confusion (anstrom in the orso header, nm in the model);
- more than about 6 layers leads to too long lines.

---

#### a listing

Each line represents one layer. 

Examples (Jochen)

``` YAML
    sample:
        model:
            origin: guess from preparation 
            stack:
             - air  , inf nm , 0.0E-6 /fm^2 , 0.0 muB
             - pep  , 2.0 nm , 0.4E-6 /fm^2 , 0.0 muB
             - Ni   ,70.0 nm , calc         , 0.0 muB
             - Fe   ,30.0 nm , calc         , 0.4 muB
             - SiO2 , 0.5 nm , calc         , 0.0 muB
             - Si   , inf nm , calc         , 0.0 muB
```

where `calc` means that the value is calculated from the formula = name.

``` YAML
    sample:
        model:
            origin: guess from preparation 
            stack:
             - air   , inf nm , 0.0 /fm^2 , 0.0 muB
             - 5
               - Ni  , 3.5 nm , calc      , 0.0 muB
               - Ti  , 7.0 nm , calc      , 0.0 muB
             - SiO2  , 0.5 nm , calc      , 0.0 muB
             - Si    , inf nm , calc      , 0.0 muB
```
 
(Brian)
First, to use the refl1d version with the model definition file, check out the 'dataclass_overlay' branch from both bumps and refl1d and install locally, installing bumps before refl1d in your virtual environment (because refl1d will try to grab bumps from pypi during install)

For adding a simple model to the header, I suggest that we not try to overload the description field.  We can be clear about what the new field is: a representation of the model (or the structure of the sample, which is sort of the same thing).  I think many users will want to use the 'description' field to put in all sorts of other important information, which is not relevant to the reflectometry information domain (where we care about layer structure)

We could call it sample.structure or sample.model or something like that - 
I would also recommend requiring a subfield that describes the model language being used, e.g.
(we could also link to a language definition specification instead of a schema)

``` YAML
    sample:
      model:
        schema: https://www.reflectometry.org/schemas/model/simple/1.0.0/base.json
        value:
        - - Si
          - 0.0
          - 5.0
        - - SiO2
          - 25.0
          - 10.0
        - - Vacuum
          - 0.0
          - 0.0
```

pro: 

- the table-like appearance is easy to read
- more information can be provided
- a large number of lyers does not affect readability

contra:

- entering this information *by hand* is quite susceptible to errors;
- there is redundand information (the chemical composition can already contain the info about the SLD).

---
 
#### combination

A hirarchical approach, where a one-line string tells the layer sequence with 
name or formula, probably SLD, thickness and probably magnetisation of each layer in a 
very compressed way. 
In case this is not precise enough, additional or more precise information is given in the
*listing* format, where the entries of the 1-liner are used as identifiers.

Examples (Jochen):

``` YAML
    sample:
        stack: Si | SiO2 0.5 | Fe 30 0.4 | Ni 70 | pep 0.3 2 | air
        layer_description:
         - layer: 'SiO2 0.5'
           relative_density: 0.95
           lower_roughness: 5.0
           upper_roughness: 3.0 
         - layer: 'pep 0.3 2'
           SLD: 
           lateral_structure:
           ...
         - layer: 'Fe 30 0.4'
           magnetisation_axis: 0.9, 0.1, 0.0
```

or 

``` YAML
        layers:
         - pep
           material: peptide123
           density: 0.95
        materials:
         - material: peptide123
           formula: A3B5C6
           density: 
               value: 34.6
               unit:  1/angstrom^3
           reference: doi:1234AB678
```

(Artur)
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
information has to be provided in a maore complicated form;
- one can add more and more complex information up to a full description (e.g. after analysis)

contra:

- extra information is again difficult to enter *by hand*;
- complicated samples lead to a too long first line.

---

### vocabulary

There is a wide variety of meanings associated with key words such as *layer* or *material*. 
It will be quite complicated to find an agreement here, because most of us will have to 
accept an *absurd* choice at some point.

- `sample` already defined
- `model`
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

``` YAML
    sample:
        model:
            origin:     * guess based on preparation / XRR
            schema:     * orso-short-notation (some default)
            stack:      Si | 10 ( Fe 7 | Si 7 ) | air
            layers:          *
             - layer:        Fe 7
               moment: 
                   value:    2.2 
                   unit:     muB
               sld: 
                   value:    5.02e-6
                   unit:     1/angstrom^2
             - layer:        Si 7
               composition:  SiN0.01
               rel_density:  0.95
               ...
            roughness:       *
                value:       5
                unit:        angstrom 
```
