---
layout: page
title: "File Formats 2a Minutes"
permalink: /workshops/workshop_2020/file_formats_2a_minutes
---

Notes on session File Formats 2A


   Discussion on the formulation and application of “FAIR” principles
   of data (Findable, Accessible, Interoperable and Reusable) for ORSO
   standard reduced data files


- agreement that historical information is needed on provenance (who was the experimenter, when, etc) even if this is not part of the analysis needs


JS: from an earlier session, there was agreement that not all data/metadata needs to be in one file, but that there can be links or references to other sources of data.


AG: completeness can refer just to the reflectometry part of the data, not necessarily to the sample information and quantities not needed for analyzing reflectometry data.


     short discussion on how facility data policy affects FAIR principles:

JS: data are currently freely available at PSI for immediate download, but in the near future will be immediately embargoed (even from access by instrument responsibles) for 3 years.

- this embargo can be opted out by the user

- leads to situation where instrument responsibles are asking for data access for every experiment from the users

- makes “accessible” part of FAIR hard


AR: while some papers are now requiring supporting or raw data for publications, they frequently cannot handle the large data sets that actually happen. So users rely on facilities to host the data.

- quite a few facilities make it difficult to lift the access barriers to data.


BM: surprised that data are being more locked down, given the spreading of FAIR principles

AG: this change in policy is because of FAIR (trying to apply rules that were never really enforced, while trying to meet FAIR requirements)


     short discussion on “correctness” of terminology used:

JS: we are wanting to define things as physical quantities in the files when they really are not, e.g. R is not the true reflectivity of a sample but is combined with resolution effects etc.

AG: other techniques use the name of the physical quantity even when it is a measured quantity with resolution etc.

BM: correctness in terminology is less important to me than explicitness


     resolution and “extended” or “derived” quantities

from previous discussion, A. Nelson and others may want lookup tables for Q resolution for each point (or in fact theta and lambda resolution tables for each point)

JS: this is too much for current analysis packages to handle


AR: software should support current capabilities and future needs.

- if Gaussian representation of resolution is one of our standard “required elements” in a data file, it could be included also (derived from the distribution)


JS: this raises a larger point about redundant information in a data file: do we need to identify which are “master” columns/quantities and which are “derived”

AR: some quantities are better-defined and more traceable, can we indicate which values are calculated and which ones are traceable (and calibrated?)


AR: being able to indicate “primary” x-coordinate and errors can be helpful


     reusable principle

JS: do we need to house all raw data in reduced file to make it “reusable”?

- this can make the files quite large and complicated

AG: in modern files this is less of a concern

BM: in most discussions they are looking for metadata, not data when referring to reusability principle

AR: need version information on reduction operations used on the data (sometimes these operations are changed by the instrument responsibles at a later date – important to know which algorithms were used to produce reduced data)


- documentation on data reduction modules is not easy to find in general (often just code, even when available)


- can we develop a stylized language and descriptors to make documentation of reduction steps more clear?


     On the question of reformulating the FAIR principles in the FF
     document

JS: should we reformulate the principles to represent what we are really going to be able to do?

AG: I would treat FAIR principles as more more high level requirements (not specific) that we try to achieve

JS: in previous discussions it was agreed that the minimum data set for reflectometry will have just Q and R – how does this meet requirements for principles?

BM: the goals of the FAIR principles and the goals of reducing data for a particular experiment can be separated

AG: the principles apply even for that case: when published, the reusable principle is important for readers of the paper, for checking results.

AR: should we identify levels of adequacy for various uses, based on levels of implementation of the standard?


     Data from lab (benchtop) equipment

AR: for lab instruments, users want the same rich data they get from the custom facility instruments. It is hard to get people building commercial hardware to include rich and interoperable data.

There are a couple of big suppliers of X-ray equipment; they don’t engage with us and we don’t engage with them.

AG: if this working group approached the manufacturers, this might be useful.

AR: it might be a task for someone to try to figure out how to make contact with the companies, and make it clear that there is a benefit to them.

AR: we need the same standards for data from lab instruments (metadata, error bars, etc.) as from custom instruments

AG: people are willing to write translators for this benchtop equipment data (can it be shared generally?)

AR: it is good practice to enter metadata into e.g. filenames (sampel information) on benchtop equipment

JS: there are many details to keep track of, Soller slits, attenuators, etc.

AR: maybe we should gather information on what is written so far.


### Tasks

- for final session: identify people who know how to make contact with
   the companies.
- reformulate the principles
  - circulate document by email for comments
  - keep circulation list small at first (4-5)
