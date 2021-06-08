---
layout: page
title: "File Formats"
permalink: /working_groups/file_formats/
---

This working group aims to draft a spec for a standard file format to be used across X-ray and neutron reflectivity.
This is an important goal to move towards interoperability across facilities.

## the principles

- **inter-operability**: The format allows the data to be processed by a wide variety of software.
  This of course requires the software developers to use the same standard.
- **reusability**: The file contains sufficient information to process (or interprete) its content.
- **correctness**: All quantities in the data file are well defined, labeled and come with an appropriate unit.
- **ownership**: It must be clear from the content who the owner of the original data is and who
  processed it.
- **reproducability**: The data set contains enough information to recreate it from the raw data.

## target audience

In general this is everyone dealing with reflectometry data. This includes the scientists using 
reflectometry as a tool, beamline scientists who see it as their profession and programmers writing 
reduction and analysis software. It is thus not trivial to cover all needs and preferences with
just one format representation.

During various discussions we came to the conclusion to define *one format* which might be
expressed in (at least) *two representations*.
The format is defined by some hirarchy, a dictionary and rules for mandatory, recommended and optional information. 

The two representations are:

- An ASCII formatted documet with the common header - data set structure. 
  The essential feature of this document is, that it is easily human readable.
  This sets some limits to the content, since this readability gets lost if the header gets too long,
  if it contains too much information not needed by humans, or if various data sets are combined.
  
  The target group of this representation are the scientists who want a *reflectivity file* containing the
  *R(q)* curve with errors, resolution and some information about its history. 
  
  Also this representation is completely sufficient for most of nowaday's data analysis or visualisation programs.
- A binary document in hdf format: this contains as much information as (reasonably) possible - and needed for further
  processing. E.g. the data file of the reduced data should not contain all the raw data file information, but just
  enough to trace it back and to allow for analysis.

  The hdf format allows for more precise (and complicated) data treatment. E.g. the real resolution of the measurement
  can be used, rather than the averaged one, pressed into a Gaussian distribution.

## previous workshops:

### Workshop on the text representation, 2021-03-22

Essentially formal questions about the text representation of the data have been discussed 
([minutes](../../projects/file_formats/tasks/meeting_2021-03-22.md)) and 
a [draft data file](../../projects/file_formats/tasks/text_representation.md) has been created.

### Workshop on September 30. 2020

[Task list based on the outcome of the workshop](../../projects/file_formats/tasks)

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
  most likely using a NeXus-based hierarchical data format (HDF).   
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
has been given and [example files](../../projects/file_formats/examples.md) were created as a basis for
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

A series of [**tasks**](https://github.com/reflectivity/file_format/issues)
were defined and everyone is welcome to contribute
there (as well as to the general discussions):

- [x] Draft a *strict* reflectivity file in NeXus format. (A. Nelson)
- [ ] Contribute to the [dictionaries](../../projects/file_formats/dictionaries.pdf)
- [ ] Create an overview over the national data policies
- [ ] Define an identification scheme
- [ ] Try to implement the *pragmatic* file format in existing
      data reduction software and report on problems and
      successes

---

## Co-chairs

- [Jochen Stahn (PSI)](mailto:jochen.stahn@psi.ch)
- [Max Skoda (ISIS)](mailto:Maximilian.Skoda@stfc.ac.uk)
