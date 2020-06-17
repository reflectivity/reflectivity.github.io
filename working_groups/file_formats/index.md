---
layout: page
title: "File Formats"
permalink: /working_groups/file_formats/
---

This working group aims to draft a spec for a standard file format to be used across X-ray and neutron reflectivity. 
This is an important goal to move towards interoperability across facilities. 

### Summary of the May 2020 workshop
The outcome of the discussions during the workshop can be
summed up as
> We want to stick to the principles whenever possible -
> and at the same time we are flexible and pragmatic and pay respect to
> traditions.

This attitude shows up in the agreement to use SI units,
but to also allow Angstrom.

Acting in this spirit a broad support for developing
**two file formats** could be observed:

* The **strict format** who's aim is to fulfil the principles of 
  *inter-operability* and *reusability*,
  most likely using a NeXus-based the hierarchical data format (HDF).   
  This format should fulfil high scientific standards, it will allow
  future analysis software to do more than just fit R(q) curves and
  it should match the national data policies.

* The **pragmatic format**, which should be easily human-readable
  (i.e. ASCII format with a YAML like structure) and contain the
  essential information to trace back its origin and some
  basics about the state of the data reduction. The standardised
  minimum content of this format can be expanded as seen appropriate
  by the user.   
  This format is compatible with most actual analysis software.
  It is easy to understand and read by new users and thus
  unburdens beam line scientists. And it allows fast and easy comparison
  of results. After all, a *reflectivity curve* is expected as the
  outcome of a *reflectivity measurement* - however spoilt by
  experimental influences (e.g. resolution) it might be.

It is in the nature of the *pragmatic* approach that a lot of input
has been given and [example files](examples.md) were created as a basis for
further discussions.

The main work for the (near) future will be to create a series of
**dictionaries** describing the vocabulary used in either of the
two file formats:
* list of **symbols** and their definitions (e.g. *alpha_i := angle of
  incidence on the sample surface ...*)
* dictionary of **key words** (e.g. what is the meaning of *reflectivity*
  in the context of the *reflectivity data file*)
* dictionary of standard(ised) **data reduction steps** (e.g.
  *geometrical footprint correction*)

It was recommended to use existing definitions from NeXus or
canSAS whenever they fit. If theses are not in agreement with
the common sense used in reflectometry experiments (e.g. the
sample coordinate system) new, specialised definitions should be
written in a similar style.
It was also suggested to start with the essential quantities
(again coordinate system, angles, momentum transfer, ...) and
to expand the dictionaries later when needed. These dictionaries
are to be used for the reduced data. The very technical and highly
instrument specific *language* describing the raw data should
be used in the raw data file.

A series of **tasks** were defined and everyone is welcome to contribute
there (as well as to the general discussions):
- [ ] Contribute to the dictionaries.
- [ ] Create an overview over the national data policies.
- [ ] Try to implement the *pragmatic* file format in existing
      data reduction software and report on problems and
      successes.
- [x] [Draft a header for an ASCII file.](examples.md) (J. Stahn)
- [x] Draft a *strict* reflectivity file in NeXus format. (A. Nelson) 

---

### [Message board](https://gitter.im/reflectivity/file_formats) 

Open to all to read, however, an account is required to post.

### Members

- Ruipeng Li (NSLS-II)
- Brian Maranville (NIST) 
- Tim Snow (Diamond)
- Jochen Stahn (PSI) -- **Chair**
