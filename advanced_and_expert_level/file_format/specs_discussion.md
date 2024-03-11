---
layout: page  
title: "ORSO - file formats - discussions on specifications"  
permalink: /advanced_and_expert_level/file_format/specs_discussions
author: "Jochen Stahn"  
---


## Feedback and discussions on the .ort specs

In the following you find a rather unsorted collection of feedback and ideas about the [`.ort` specs](https://www.reflectometry.org/advanced_and_expert_level/file_format/file_format_specs) and the related [`orsopy`](https://orsopy.readthedocs.io/en/latest) package.

last modified: 2024-03-11


## confusion of physical terms 

(Jochen)

We have a confusion of what we use as key words. Since the german terms are different I had probplems figuring out the correct English definitions...

### official definitions

What can be measured or calculated is a **physical quantity**.

> E.g. the *incident angle*

This has a **dimension** = dim(*physical quantity*) relating it to a set of base quantities like *length*, *time*, *charge*, *temperature* etc. The *dimension* is no unit, nor can it be used to unambigiously describe a *physical quantity* (*plane angle* does not tell between *scattering angle*, *incident angle*, *total reflection angle*, ...). 

> dim( *incident angle* ) = *plane angle*

The *physical quantity* is often refered to by using a **symbol**.

> one possible symbol for *incident angle* is $\alpha_i$ (or *alpha_i* in the orso header)

The *physical quantity* is composed of a **numerical magnitude** times **unit**. Depending on the chosen *unit*, the *numerical magnitude* changes.

> $\alpha_i = 2.3 \cdot \mathrm{deg}$


## guidelines for writing and reading

- avoid contradicting information (e.g. single incident angle in the header for angle-disperse measurement)
- The `.ort` format contains the *reduced* data. Its content should thus be limited to information useful to the scientist and to some analysis software. The *rest* is still available in the raw files..... 

## open issues for lab x-ray reflectometers
 
Which of the keys discussed below should be included in the specs to (better) incorporate lab x-ray data files?

When attempting to convert the ASCII output files of various commercial lab x-ray reflectometers (diffractometers) 
it became obvious that the present dictionary misses several entries.
 
- It is not exactely clear where to put the *brand*, *model* and probably *configuration* information.

  ``` YAML
     experiment:
         title: ...
         instrument:
             type: x-ray lab source        (neutron reflectometer, synchrotron diffractometer, ....)
             brand: Brucker
             model: Discovery
             hardware_indicator: 65519
  ```
 
- The wavelength is often defiend via the anode material, the line(s) and probably the presence of a monochromator.
- The slit sizes are reported to enable resolution calculation.
- Often a long list of hardware settings is supplied, e.g. tube current, temperature, configuration, etc. 
  These things do not really belong to a *reduced data* file, but we should at least recommend a place for these entries. In the example below I put it as a multy-line string in `instrument_settings.details`.
 
``` YAML 
     measurement: 
         instrument_settings:  
             incident_angle:           
                 min:          0.1
                 max:          6.0
                 unit:         deg
             wavelength:               
                 magnitude:    1.54184
                 unit:         angstrom
                 anode:        Cu 
                 lines:
                    - name:    K_alpha1
                      magnitude:  1.5405980
                      weight:  2/3
                    - name:    K_alpha2
                      magnitude:  1.5444260
                      weight:  1/3
             scan_type:        continuous
             details: |
                 "Configuration=Reflection-Transmission Spinner 3.0, Owner=user, Creation date=3/5/2021 8:12:09 AM"
                 "Goniometer=Theta/Theta; Minimum step size 2Theta:0.0001; Minimum step size Omega:0.0001"
                 "Sample stage=Reflection-transmission spinner 3.0; Minimum step size Phi:0.1"
```

- The `.ort` specs clearly separate data origin and data reduction. For lab reflectometers it often the same software for 
  instrument control and reduction.
- Information about the facility, the owner and the sample is often missing.
 
## new column type: `flag`
 
suggestd by Artur, draft by Jochen
 
``` YAML
# columns:
...
#    - flag_is:
#      0: electric field off
#      1: electric field on, positive
#      -1: electric field on, negative
```

or 
 
``` YAML
# columns:
...
#    - flag_is:
#      0: ignored for fitting
#      1: used for fitting
```

The flag key is to be treated as an integer (by cutting off after the point). 

## `date` vs. `datetime` for experiment date

The header entry `data_source.experiment.start_date` presently shows the date and time. The intention here was to report on the date only. The time is listed anyway for all individual measurements below.

Can we change this to **date** only?

## slit info format

Should we define a format for slits? I.e. *distance to sample*, *horizontal* or *vertical openings*, *radius*, *shape* ....

siggestion:

``` YAML
# data_source:
#   measurement:
#     instrument_settings:
#       apertures:
#         - name: D1
#           shape: rectangular
#           horizontal_opening: 20
#           vertical_opening: 0.5
#           distance_to_sample: -983
#           unit: mm
#         - name: D2
#           shape: circular
#           radius: 2
#           distance_to_sample: 30
#           unit: mm
```

with 

- `shape`: circular, rectangular, slit
- dimensions: vertical_opening, horizontal_opening, radius
- `distance_to_sample` with negative sign befre the sample (strictly speaking, it is no longer a disctance then). Maybe `position_wrt_sample`?

If this format is opened to other devices, e.g. a collimator tube, the `distance_to_sample` would need to allow for a range.
