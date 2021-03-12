---
layout: page
title: ORSO file format - tasks - misc
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-08
---

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
    wavelength: {line: CuKa}
```

## tasks

## decision
