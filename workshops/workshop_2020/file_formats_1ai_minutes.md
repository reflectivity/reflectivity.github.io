---
layout: page
title: "File Formats 1a(i) Minutes"
permalink: /workshops/workshop_2020/file_formats_1ai_minutes
---

### 2020-05-26 at 15:30 UTC

### Participants:

-	Tim Snow (Chair; Diamond)
-	Jochen Stahn (PSI)
-	Chen Shen (DESY)
-	Paul Kienzle (NIST)
-	Alexandros Kotsioumpas (JCNS/MLZ)
-	Juan M. Carmona L. (JCNS/MLZ)

### Minutes

**Tim:**

The discussion starts by taking a look at  https://www.reflectometry.org/ and by having a quick overview of the presentation by Andrew Nelson of 2019 (https://www.reflectometry.org/assets/nelson_reflectometry_standards.pdf).

Discussion 1: What do we want or we don't want to store?

Discussion 2: Precise definitions of what is being stored - Specify terminology, be consistent
Sometimes there are more than 1 different meanings for the same word!

**Jochen:**

Should we define a data file format and that's it?
Which contents are relevant? Should each Q point have a matrix of resolution?
Including too much information would cause a "data explosion" rather than a reduction.
Everyone has a clear picture of a word and they are sometimes inconsistent.

**Tim:**

https://gitter.im/reflectivity/file_formats

**Chen:**

What about the definition of coordinate systems?
What about Diffraction? Should reflectometry be treated as a special case of diffraction?
Should we be consistent about definitions in these two areas?

**Tim / Jochen:**

We can do whatever we want to maintain consistency between diffraction and reflectometry,
the question is if people WANT to use the same terms... because some terms mean different
things in the two areas. Coordinate systems "sample" are different between reflectometry
and diffraction.

Show "The Nexus Coordinate systems" (https://manual.nexusformat.org/design.html#design-coordinate system)

**Chen:**

Every instrument has its coordinate system. Where to put x y z?
Relative to the beam? Relative to the sample?

**Jochen:**

This is a never ending discussion. For reflectometry, Z is always normal to the sample surface.
For Nexus, Z points into the plane of the detector.

**Alexandros:**

It is worth noting that this is not a matter of people being stubborn.
Changing the coordinate system will cause big trouble when reading already existing literature.

**Chen:**

We could define coordinate systems maybe with a suffix, so we know Which
coordinate systems we are speaking about.

**Tim:**

The logic of this system (NeXus) came from McStas. There they are explicitly given names.
If we deviate from these two different styles, at least 50% of the community will be unhappy.
Many of the terms are already defined, though.

PanOSC promise to deliver interoperability between facilities and instruments; adopting a
standard is important to reach that goal. Take the analysis that you already made and
compare them to future experiments that could provide complimentary data. Is this relevant?

**Chen:**

Documentation is quite important also. Many students tend to forget doing documentation, so it
is very hard to follow what they did afterwards; also scientists, they don't write notes even
when they are reminded that data won't be able to be analyzed if some information is not provided.

**Tim:**

At Diamond, we write metadata "for free" about the sample and the environment. If scientists provide us with some information, we offer to analyze their data... If they have these incentives, they are usually happy to provide the info (e.g. expected SLD profiles). "We can scan 10K samples an hour, we could ease your analysis if you provide us with some info".

**Jochen:**

If it is only the reflectometry file, it is hard to understand what I did, I have to re-run the analysis.
If I had already some info there, I would save time.

**Tim:**

The ML community will be happy to have as much data as possible.

**Chen:**

It is important that all info is stored. From ILL I get much data and a standardized routine that
the instrument scientist provides in order to reduce it. Then, there are all the links that
tell me which data I used and the steps I have followed to reach a result. The amount of data is
not that large. Maybe we should have a raw data file and a standard procedure also for reflectometry.

**Tim:**

We also have those "linked steps" in SAS, GISAS and Reflectometry. There's DP-DAK, which does SAS and GISAS; maybe modifying it, it would also do Reflectometry. Raw reflectometry data is divided in ToF and Monocromatic.
Taking a look at those, it seems that there would be difficulties synchronizing them. Taking all the packages that reduce raw data, we should ask how is the agreement between them and, if possible, choose a common solution.

**Paul:**

It is very hard to capture everything that happens in an experiment within a single file.
Even if two measurements occur in the same beamline, they may have different q points or resolutions.
"We need a new feature, let's add it to the reduction" - And suddenly the loading of a file is broken for some data analysis software.
How well does it work when you re-bin the data? We try to keep everything relatively simple, reduce the format to not put much information in there.

**Tim:**

The easiest thing that people do is to record the energy at which you take a measurement, so you
can refer to it later on. If there are a small number of energy / broad spectra of energy, that is
more challenging... You have to scan a single energy and repeat the process for a different number
of energies and be sure that they are not near an absorption edge.

**Chen:**

But sometimes users request energies near an absorption edge to tune the contrast between different materials; since the energy is included in the data file... [...] ... it is not by accident that they want to measure near the absorption edge.

**Tim:**

Sometimes we don't have control over these.

**Paul:**

Coming back to the main topic… We hav a .json string that we feed to a program and it can re-do the reduction.

**Tim:**

Yes, Andrew McCluskey has been working on a python program that somehow accomplishes this (https://github.com/arm61/islatu).
Also in Mantid, you can reference the pipeline that was used to recover tha analysis.
It is true that between neutrons and X-ray data reduction pipelines there are some differences,
e.g. X-ray does not need gravity correction, however there is common ground -e.g. background correction.

**Paul:**

Measuring background curves is also tricky some times, because of the sample. Even obvious things as background correction are not always obvious.

**Jochen:**

Regarding higher quality data, I don't think that we can actually create a reduced data file containing all the information.
If you collapse the Q matrix to a Q vector, you get a strange resolution function. I don't think you
can have a q vector with a "real" resolution. Still, one has to do it to be compatible.

**Paul:**

*There is some discussion about storing theta-lambda vs. Q*
Theta-lambda can be reconstructed given q. However, to do this, there are some differences between neutrons and X-Rays.
Changing the SLD from the Silicon... One can be accurate to 1% ... (?)
We can get away with binning the different data together and acknowledging there is a resolution curve. Many people do not need it because the resolution is very narrow.
This may not be useful for X-ray, which have systematic uncertainties as opposed to statistical uncertainties.

There is this notion of errors in variables in linear regression -- dx and dy; possibility of fitting data by allowing dq, one could classify the systematic uncertainties.

**Chen:**

One can consider that resolution has no impact for narrow systems --sometimes it is not even relevant to include dq in the data, it makes no difference.

**Tim:**

The resolution is given by the properties of the beam.

**Paul:**

Should we have a common software for reducing data?

**Tim:**

We're missing the reduce data application definition.
We need interoperability. Intensity vs Q... sounds good, but still...
The price you pay with co-refine datasets in two different instruments...
What do all quantities mean in each instrument and for each person?

**Chen:**

True. For instance, in our instrument, we have a problem defining the coordinate system.
samples with liquid surface vs. solid surface, they have conflicting definition of coordinates.
We have to tell our users which is the coordinate system.

**Tim:**

In reflectometry we don't have this problem. Everyone agrees that the z
direction is perpendicular to the sample surface.

**Chen:**

Each Q has an actual associated intrinsic dq or is it computed by error propagation?

**Tim:**

You should actually measure dq, but in reality it is more complicated... you should
observe reproducibility to assess the reliability of your instrument.

**Chen:**

Users measuring thin films ... resolution… Do your users make a convolution of the error of not?

**Tim:**

It has been developed over time... We are off-topic
People looking at thicker films approaching the size of the beam will need a good description
of the resolution, we cannot hide it under the carpet anymore.

**Tim:**

Sometimes a decision taken about that something is correct, is not true six months
later. In order to check that what you said was correct, you will need reusable data.
FAIR data principles.

**Juan:**

A very high-level solution of the problem would be to have standard ways of reducing the data to in the end have a file with Q, R(Q), dQ, dR. Then one can compare measurements in one instrument with measurements of another instrument. Yet, I see that the problem is much more complicated than this "high level" ideal solution.

**Paul:**

If you have an error on your measurement, that can spoil all your subsequent analysis. You certainly want to have that error logged somehow. One very important related aspect is the question about the background. How to treat the background properly?

**Tim:**

This is a going on discussion which is important, we can come back to this later.
Raise it again later.
