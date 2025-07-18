---
layout: page  
title: "ORSO - file formats - specifications for the text reflectivity file"  
permalink: /advanced_and_expert_level/file_format
author: "Jochen Stahn"  
---

This document contains the specifications and some examples for the text representation of the ORSO reflectivity file. 
It is the basis for the development of the [**orsopy** python modules](https://orsopy.readthedocs.io/en/latest) to read and write these files.

The main contributors are: 
Andrew McCluskey,
Andrew Nelson,
Artur Glavic,
Brian Maranville,
Maximilian Skoda
and
Jochen Stahn.

last modified: 2023-06-16

---

This specification file aims at describing and defining the ORSO `.ort` format. The **reference** in case of a conflict or
ambiguity is the schema of the orsopy implementation (if up-to-date). 
If you detect some inconsistency, please report it to <Jochen.Stahn@psi.ch>.

Items under discussion and changes intended for **future releases** can be found at the 
[discussion page](./specs_discussion.md) and on the [simple model specifications page](./simple_model.md).
Please have a look and contribute (critics, suggestions, hints, ...).

---

## general specifications

### data organisation

The data file consists of 3 kinds of content blocks:

- the first line containing version and language information
- a block with meta data and column description
- a data array with the reduced data and optional information

For several data sets in one file the later two are repeated.

### file extension

It is recommended to use the suffix `.ort` (**o**rso **r**eflectivity **t**extfile) for reflectivity files following this standard.

### placeholders

If there is no entry available for a keyword, the default value for both, string and numbers is the string `null` 

### language

In line with canSAS and NeXus, we use American English for the keywords. 
E.g. `polarization` rather than `polarisation`. 

### encoding

The text representation allows for UNICODE characters encoded in UTF-8. 
For the keywords only ASCII Basic Latin encoding is allowed. 

### date and time format

For time stamps we use the ISO8601 format for **date and time**: 'yyyy-mm-ddThh:mm:ss'.
This is local time.
Do not use UTC (hence no suffix Z).
If the time zone shall be specified (e.g. for disambiguation of summer/winter time),
then append the UTC time offset in the form "+09:30", i.e. 'yyyy-mm-ddThh:mm:ss+09:30'.

If just the date is of interest, only the **date** part of the same standard is used: 'yyyy-mm-dd'.

### units

In the header, physical quantities shall always be stored with magnitude and unit in the form

```
<quantity>:
     magnitude: <magnitude>
     unit: <unit>
```

Rules for using units:

- only ASCII symbols are to be used;
  - the greek letter μ is written as `mu` (e.g. `mum` for micrometer);
  - `angstrom` cannot be abbreviated (`Å` is not allowed, `A` stands for Ampere);
- composition with `*` and `/`, exponentiation with `**`
  - reciprocal units are written e.g. as `1/nm` for wavenumbers, `1/angstrom**2` for the scattering length density

Recommended units include:
- for angles, `rad`, `deg`;
- for lengths, `m`, `mm`, `nm`, `angstrom`;
- for durations, `s`;
- for energies, `eV`, `keV`;
- for temperatures, `K`.

### errors

Errors or uncertainties of a single physical quantity can be given in the form

```
<quantity>:
    magnitude: <magnitude>
    unit: <unit>
    error: 
        magnitude:    <error magnitude>
        error_type:   uncertainty (default) | resolution
        distribution: gaussian (default) | uniform | triangular | rectangular | lorentzian
        value_is:     sigma (default) | FWHM
```

The respective unit of the error is taken from the quantity the error referes to.  

Example:

```
incident_angle:
    magnitude: 2.3
    unit: deg
    error: {magnitude: 0.05}
```

### comments

There are 2 kinds of comments possible:

The key word `comment:` allows to add free text, e.g. to describe a related entry in more detail. Using YAML coding (see below) a multi-line comment might look like:

```
comment: |
   Attention! These data have been collected without the frame overlap mirror
   and contain thus some highly structured background.

   Still the peak positions can be analysed.
```

A hash (`#`) declares everything that follows on the same line to be outside the hierarchical structure and will be ignored by YAML (or JSON) based information processing. 
E.g. the first line of the text representation contains information not structured due to YAML rules and thus starts with `# # `, where the first hash means *header* and the second *non-YAML entry*.

Example:

```
sample:
   name: Ni1000 
   # there is a scratch on the surface!
```

### look-up order for physical quantities

Some information might be provided as a column in the data block, in the header or in both places. In the latter case the 
information provided in the data block has higher priority for data reduction or analysis. 

A condition for this to work is that the *key* in the header is the same as the `physical_quantity` *value* in the column description. 
E.g.  
`data_source.measurement.instrument_settings.` **`incident_angle`**   
and   
`columns.?.physical_quantity:` **`incident_angle`**

---

## the header

The header may contain more sections than presented below - and also the sections may contain user-defined `key: <value>` pairs on all levels. 
These of course should not interfere with defined content, and the rules for units and formats should be applied as stated above.

The header follows a hierarchical structure and is formatted according to YAML (see below) or JSON rules. 
In addition, each line of the header starts with a hash and a space `# ` (wrapped YAML), which is the default header marker in Python (and other languages).

The header is organised following a *chronological* structure:

- Where do the raw (=input) data come from?
- What was done to them?
- (probably) How were they analysed?
- What is the outcome?

### first line

The first line contains information about

- the general content;
- the ORSO file format version (and level of strictness) used;
- the encoding;
- a link to orso.

Since it is not part of the YAML hierarchy, a second hash is needed.

```
# # ORSO reflectivity data file | 1.0 standard | YAML encoding | https://www.reflectometry.org/
```

### second line

optional, **recommended**

This (comment) line should help the user to identify the content based on a few key words.
It is free format and no further rules apply.

```
# # <title> | <date> | <sample name> | <what>
```

e.g.
```
# # Interdiffusion in Fe | 2020-12-24 | sample fe-457-2 | R(q_z)
```

### data source

(mandatory)

This section contains information about the origin and ownership of the raw data, together with 
sample- and instrument-related details.

```
# data_source:
```

This information should be available from the raw data 
file(s). If not, one has to find ways to provide it.  
                             
```
#     owner:                 This refers to the actual owner of the data set, i.e.
                             the main proposer or the person doing the measurement
                             on a lab reflectometer.
#         name:          
#         affiliation:       If more than one affiliation is listed these can be 
                             seperated with a `;` or written on multiple lines.
#         contact:           * email address
#     experiment:  
#         title:             proposal, measurement or project title
#         instrument:    
#         start_date:        yyyy-mm-dd (for series of measurements) or yyyy-mm-ddThh:mm:ss (e.g. for lab x-ray reflectometers)
#         probe:             'neutron' or 'x-ray' (see nxsource)
#         facility:          *
#         proposalID:        *
#         doi:               * might be provided by the facility
#     sample:  
#         name:              string identifying the individual sample or the subject and state being measured
#         category:          * front (beam side) / back, each side should be one of solid, liquid or gas (i.e. solid/liquid)
#         composition:       * free text notes on the nominal composition of the sample  
                              e.g. Si | SiO2 (20 A) | Fe (200 A) | air (beam side)
                              this line/section might contain information to be understood by analysis software
#         description:       * free text, further details of the sample, e.g. size
#         environment:       * list of free text name of the sample environment device(s) e.g. [magnet, cryostat] or [sample changer]
#         sample_parameters: * sub-items for sample parameters with currently undefined keys and associated values, e.g. T: {magnitude: 300, unit: K}
```

The following list of sample parameters is incomplete and expandable.
All these entries are optional.

```
#         temperature:
#             magnitude:        2
#             unit:             C
```

In case there are several temperatures:

```
#         temperature:
#             min:            50
#             max:            150
#             unit:           K
#             all_values:     [v1, v2, v3, v4, ...]   # (a user-defined keyword in this example)

#         magnetic_field:     #  (if only value is needed use "magnitude" instead of x/y/z
#             x:              1.5
#             y:              0.2
#             z:              -0.2
#             unit:           T
#         electric_potential:
#             magnitude:      25
#             unit:           V
#         electric_current:
#             magnitude:      2
#             unit:           A
#         electric_ac_field: 
#             amplitude:
#                   magnitude:      2
#                   unit:           A
#             frequency: 
#                   magnitude:      50
#                   unit:           Hz
```
    
and so on for `pressure`, `surface_pressure`, `pH`, ....

#### not sample-related details about the set-up

```
#    measurement:
```

This section contains the general settings of the instrument and a list of raw data files and supporting files.


```
#        instrument_settings:
```

(mandatory) general instrument settings not closely related to the sample

```
#            incident_angle:   
```

(mandatory)

- one of
  - `magnitude`
  - `range`
      one of
      - `min` and `max`
      - `individual_magnitudes`
- `unit`
- `offset` (optional) to indicate a discrepancy between 'target' positions and actual (fitted) values
- `movement` (optional) one of `steps` or `continuous`

```
#            wavelength:
```

(mandatory)

- one of
  - `magnitude`
  - `range`
     - `min` and `max`

```
#            polarization:
```

(mandatory)
Qualitative description of the *intended* instrument setting.

- for neutrons:
    
  A two-letter code where the first letter describes the preparation of the beam before the sample
  and the second the manipulation of the reflected beam.
  - *p* (plus polarization): neutrons with **magnetic moment parallel** to the external field arrive at the 
    sample / detector.
  - *m* (minus polarization): neutrons with **magnetic moment antiparallel** to the external field arrive at the 
    sample / detector. 
  - *o*: no polarization or polarization analysis is performed.
      
  This leads to the combinations:
  `pp`, `mp`, `op`, `pm`, `mm`, `om`, `po`, `mo` and `oo`,
  where the last can also be written as `unpolarized`. 

  > This definition of *plus* and *minus*
  > - has the experimentally verificable consequence that
  >   the measured critical momentum transfer of a saturated Fe film on silicon is higher
  >   in the `pp` configuration of the
  >   instrument than in the `mm` configuration. 
  >   ($q_z^{c\ \mathrm{pp}} > q_z^{c\ \mathrm{mm}}$);
  > - means that for the *plus*/*minus* configuration one gets
  >   $V = V_\mathrm{nuc} +/- \mu_\mathrm{n} B$;
  > - is opposite to the one used in CANSAS, where they
  >   refere to the **spin** alignment relative to the external field.



- for x-rays:

  One of   
  `pi` (linear with E-field in scattering plane),   
  `sigma` (linear perpendicular to scattering plane),   
  `right` (circular right),   
  `left` (circular left) or   
  `unpolarized`    
  or combinations of two linear polarizations concatenated by `_` (e.g. `pi_sigma`).

  > This definition of linear polarizations with respect to the scattering plane affects
  > components measureble in resonant scattering. In the case of pure charge scattering
  > it also affects the intensity correction by a polarization factor $P$ for larger scattering
  > angles. For `sigma` polarization this factor is $P=1$, for `pi` $P=\cos^2 (2\theta)$ and for
  > `unpolarized` $P = 1/2 (1+\cos^2(2\theta))$.      
  > See, for example, J Fink *et al.* 2013 Rep. Prog. Phys. **76** 056502 (DOI: 10.1088/0034-4885/76/5/056502)


```
#            configuration:
```

(optional)
free text, e.g. 'liquid surface set-up' or 'theta-2theta mode'





```
#    measurement: 
#         instrument_settings:  
#             incident_angle:   various options: one value = `magnitude`, range = `range.min`...`range.max` 
#                 magnitude:    
#                 range:
#                    min:
#                    max:
#                    individual_magnitudes:     for stitched data, can most likely not be used for analysis
#                 unit:         one of 'rad' or 'deg'
#                 offset:       * to indicate a discrepancy between 'target' positions and actual (fitted) values
#                 movement:     * one of 'steps' or 'continuous' 
#             wavelength:
#                 magnitude:    # or  min/max
#                 unit:         one of 'nm' or 'angstrom'
#             polarization:     for neutrons one of  unpolarized / po / mo / op / om / pp / pm / mp / mm  / vector
#                               for x-rays one of ...  (to be defined in later specification)
#             configuration:    * half / full polarized | liquid_surface | ....   free text
#         data_files:           raw data from sample
#             - file:           file name or identifier doi
#               timestamp:      yyyy-mm-ddThh:mm:ss
#               incident_angle: * user-defined in case of stitched data
#             - file:       
#               timestamp:  
#         additional_files:     (extra) measurements used in for data reduction like normalization, background, etc.
#             - file:   
#               timestamp: 
#         scheme:               * one of angle-dispersive / energy-dispersive / angle- and energy-dispersive 
```

The idea here is to list all files used for the data reduction. The actual corrections and probably the used algorithem are mentioned in the section `reduction.corrections`.


### data reduction

This section is **mandatory** whenever some kind of data reduction was performed. 

An example where it is not required is the output of an x-ray lab source, as long as no normalization or absorber correction has been performed.

The content of this section should contain enough information to rerun the reduction, either by explicitly hosting all the required information, or by referring to a Nexus representation, a notebook or a log file. 

```
# reduction:  
#      software:
#          name:         name of the reduction software
#          version:      *
#          platform:     * operating system
#      timestamp:        date and time of reduction
#      computer:         * computer name
#      call:             * if applicable, command line call or similar 
#      script:           * path to e.g. notebook
#      binary:           * path to full information file
```

The following subsection identifies the person or routine who created this file and is responsible for the content.

```
#      creator:           
#          name:         
#          affiliation:   
#          contact:       *
```

Optional, but **recommended** is a list of corrections performed in free text.
This helps the user to set the respective parameters for the data analysis.

In case a correction step follows a certain published algorithm, a link to the publication or homepage might be given.

This part might be expanded by defined entries, which are understood by data analysis software.

```
#      corrections:          list of free text to inform user about the performed steps (in order of application)
#         - footprint
#         - background
#         - polarisation
#         - ballistic correction
#         - incident intensity
#         - detector efficiency 
#         - scaling / normalisation
#      comment: |
#         Normalisation performed with a reference sample 
```

The `comment` is used to give some more information. 

### column description

This data representation is meant to store the physical quantity *reflectivity* (*R*) as a function of the
*normal momentum transfer* (*Qz*). 
Together with the related information about the error of *R* and the resolution of *Qz* this leads to the defined leading 4 columns of the data set. 
I.e.

1. `normal_momentum_transfer` with unit `1/angstrom` or `1/nm` and label `Qz`
2. `reflectivity` with unit 1 and label `R`  
   (fuzzy use of the term *reflectivity* since the data might still be affected by resolution, background, etc, and might not be normalized)
3. error of the `reflectivity`
4. resolution of the `normal_momentum_transfer` 

for columns 3 and 4 the default is *sigma*, the standard deviation of a Gaussian distribution. 
(While the specification allows for error columns of different type (FWHM or non-gaussian), this description is to be preferred.)

It's **strongly advised** that the third and fourth columns are provided. 
If these are unknown then a value of `nan` can be used in the data array. 
The error columns always have the same units as the corresponding data columns.

```
# columns:
#      - name:               Qz
#        unit:               1/angstrom 
#        physical_quantity:  normal_wavevector_transfer
#      - name:               R
#        physical_quantity:  reflectivity
#      - error_of:           R
#        error_type:         * uncertainty               
#        distribution:       * gaussian   
#        value_is:           * sigma   
#      - error_of:           Qz
#        error_type:         * resolution
#        distribution:       * rectangular
#        value_is:           * FWHM
```

with

- `name:` a recognisible, short and commonly used name of the physical quantity, most probably a common symbol
- `physical_quantity:` the plain name of the physical quantity
- `errortype:` one of `uncertainty` (default) [one random value chosen from distribution] or `resolution` [spread over distribution]
- `distribution:` one of `gaussian` (default), `uniform`, `triangular`, `rectangular` or `lorentzian` 
- `value_is`: one of `sigma` (default) or `FWHM`
- the respective unit of the error is taken from the quantity the error referes to

Further columns can be of any type, content or order,
but **always** with description and unit. 
These further columns correspond to the fifth column onwards, meaning that the third and fourth columns must be specified 
(in the worst case filled with `nan`).

```
#      - name:               alpha_i
#        unit:               deg  
#        physical_quantity:  incident_angle
#      - error_of:           alpha_i
#        error_type:         resolution
#        distribution:       rectangular
#        value_is:           FWHM
#      - name:               lambda
#        unit:               angstrom 
#        physical_quantity:  wavelength
```

It is possible to use a column for setting **flags**, e.g. to indicate the spin state
or to mark a certain Qz interval. The corresponding column description is

```
#      - name:                <descriptive name>
#        flag_is:             <list of 'integer key: string' pairs>
```

When processed, the values in this column are converted to integers (by truncation towards zero) in order to match the description.

Example:

```
#      - name:                spin_polarisation
#        flag_is:             {1: 'plus', -1: 'minus'}
```

or

```
#      - name:                attenuator
#        flag_is:             {0: 'none', 1: '0.1 mm Cu', 2: '0.2 mm Cu', 3: '0.4 mm Cu'}
```

#### canonical names for physical quantities

In addition to the mentioned 2 essential physical quantities `normal_momentum_transfer` and 
`reflectivity` and their respective errors it is recommended to 
use the following *self-explanatory keys* if applicable (the *labels* are just suggestions):

| *physical quantity*  | *self-explanatory key* | *label* | *comment* |
|:---|:---|:---|:---|
| *incident angle*     | `incident_angle`       | `alpha_i`   | |
| *final angle*        | `final_angle`          | `alpha_f`   | |
| *scattering angle*   | `scattering_angle`     | `two_theta` | |
| *in-plane angle*     | `in_plane_angle`       | `phi_f`     | |
| | | |
| *photon energy*      | `photon_energy`        | `E`         | |
| *wavelength*         | `wavelength`           | `lambda`    | |
| | | |
| *absolute counts*    | `counts`               | `cnts`      | |
| *attenuation factor* | `attenuation_factor`   | `a`         | $a \le 1$ |
| *scale factor*       | `scale_factor`         | `s`         | $R = s I$ |
| *counting time*      | `counting_time`        | `tme`       | |
| | | |
| *beam divergence*    | `beam_divergence`      | `D_alpha_i` | |
| | | |
| *intensity*          | `intensity`            | `I`         | |

 
### multiple data sets

If there are multiple data sets in one file (see below), each starts with an identifier and a line looking like:

```
# data_set:   * <identifier>
```

This line is optional for the first (or only) dataset.

Also optionally there might be a short-notation column description (preceded with a hash, since this line is outside the YAML structure):

```
# #                 Qz                      R                     sR                    sQz
```

---

## data set

The data set is organised as a rectangular array, where all entries in a column have the same physical meaning. 
The leading 4 columns strictly have to follow the rules stated in the *column description* section.

- All entries have to be of the same data type, preferably `float`.
- There is no leading space.
- Separators are spaces, tabs are not allowed.
- Whilst it's strongly advised to provide values for the third and fourth columns, if these are unknown then use `nan` for the values.
- It is recommended to use the same format for all columns, preferably `'%-22.16e'`, and align it to the column comment line.

```
1.0356329600000000e-02 3.8810006800000001e+00 4.3390906800000000e+00 5.1781647800000000e-05
1.0671729400000000e-02 1.1643051099999999e+01 8.8925271899999991e+00 5.3358647100000001e-05
...
```

## multiple data sets

In case there are several data sets in one file, e.g. for different spin states or temperatures, the following rules apply:

### separator

Optionally, the beginning of a new data set is marked by an empty line.
This is recognised by gnuplot as a separator for 3 dimensional data sets.

The mandatory separator between data sets is the string

```
# data_set: <identifier>
```

where `<identifier>` is either an unique name or a number. 
The default numbering of data sets starts with 0, the first additional one thus gets number 1 and so on.

### overwrite meta data

Below the separator line, metadata might be added. 
These overwrite the metadata supplied in the initial main header (i.e. data set 2 does not know anything 
about the changes made for data set 1 but keeps any values from data set 0 (the header) which is not overwritten.

For the case of additional input data (from an other raw file) with different spin state this might look like:

```
#     data_source:
#         measurement:
#             polarization: mo
#     reduction:
#         input_files:
#             data_files:
#                 - file:      amor2020n001930.hdf
#                   timestamp: 2020-02-03T15:27:45
```

### repetition of short-version column description

**optional**

```
# #                 Qz                      R                     sR                    sQz                  lambda
```

### next data set

The following data set has to be of the same format (number, format and description of columns) as data set 0,
probably with a different number of rows.

```
1.0356329600000000e-02 3.8810006800000001e+00 4.3390906800000000e+00 5.1781647800000000e-05 3.23390906800000000e+00
1.0671729400000000e-02 1.1643051099999999e+01 8.8925271899999991e+00 5.3358647100000001e-05 3.54342000000000000e+00
...
```

---

## the end

There are no rules yet for a footer. Thus creating one might collide with future versions of the ORSO (`.ort`) format.

## suggestions, discussion \& future

see also the [discussion page](https://www.reflectometry.org/file_format/specs_discussion)

- Prepare an  example .ort file for a lab x-ray source as basis for negotiantions with manufacturers. 
- Add structured information about the sample history. 

