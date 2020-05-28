---
layout: page
title: "Reproducibility 1b Minutes"
permalink: /workshop_2020/reproducibility_1b_minutes
---

people attending:

Andrew McCluskey (AM)
Alexandros Koutsioumpas (AK)
Gemma Guest (GG)
Thomas Saerbeck (TS)
Francesco Carla (FC)


MINUTES:

> AM
started the discussion listing the goals to be achieved by June 2020 as written during 1st ORSO meeting

> AK
suggests to add the following to the goals: put online some information about reproducibility: generate/agree on document describing elementary calculation for specular reflectivity along with calculations

> TS
asks what is the aim of the discussion: is this a practical discussion about the content of the goals (what can we do now from a practical point of view) or is it a more general discussion to (re)define our goals. 
TS raises another point about the list of requirements for measurement standards proposed by AK - TS agrees only in part with AK, because not every scientist can agree on the same representation of data (due to different instruments/ techniques...).
TS suggests that instead of providing general information we should think about to provide more information about the data analysis path - we should be able to go from data to analysis and analysis to data 

> TS
TS thinks that reproducibility is deeply related with the instrument:
He identifies 2 main issues:
1 analysis - always reproducible independently on the instrument
2 measurement - instrument dependent

TS notes that measurement+analysis should be reproducible on any instrument (on the same sample).

> AM asks what would be a suitable sample for standardization.

> AK
AK suggests multilayer metal without magnetic properties that could be measure with neutron and x-rays.
Multilayer will also generate significant features that will serve a general purpose for the analysis of the instrumental resolution.

> AM
asks if it would be possible to have a cross standard for x-rays and neutrons.

> TS 
thinks that there is common ground for this. TS suggests to use Quartz on Silicon as was already done during a Round Robin proposed by A. Nelson. The sample has been measured at ILL and ESRF and seems working quite well.
TS underline that the sample should be relatively easy to make (in case it gets lost/damaged).

> AK 
suggests that for the measure of the standard sample we should define some specific conditions (time, resolution...) so that we would be able to mimic the experimental conditions used by users.

> TS
shares the Round Robin document produced in 2017 by A. Nelson that describes the process for the standard sample measurement.

[note by FC: everybody agrees that this is a good starting point and the idea should be exploited for future developments.]

> AM
AM raises a new point about the requirements for the reproducible analysis of data:
define guidance of what we define a reproducible analysis

> TS
journals asks for a list of steps to reproduce the data reduction/analysis - 
description of the physics is not always clear so it's difficult to find a general mathematical description of the problems.

> AM 
suggests that maybe we should go beyond reduction and look at the data starting from reduced form.

> AK
AK raises the following point: how do you get to a model ?
(how do we decide which parameters we are going to use for the fitting ?)

> TS
TS points out that reflectometry does not always give a unique answer - a number of paramenters are commonly  defined by users using complementary knowledge/measures (e.g. composition, density...) - this information is not directly contained in the data file.

TS also raises a point about the reproducibility 
how different codes reproduce the same input slabs ?
when you start considering resolution functions the results obtained with different codes can diverge significantly - people should be aware of the lmitations of the different codes.

> GG 
thinks that the discussion has been more about validation than reproducibility...

> AM
suggests to refocus on the requirements

> AK 
is the  problem validation or optimisation ?

> AM
optimisation is a software-to-software problem, the question should be more: what kind of information should the users provide to reproduce the analysis ?

> TS
TS identifies 3 stages where information for data analysis is required:

1> take data from paper analyse them with the SAME software obtain the same results
2> take data from paper analyse them with a DIFFERENT software obtain the same results
3> take all the info, go to a new instrument obtain the same results

data sets we should provide information to allow people to reproduce the processes above.

> AM 
action point: define what we are trying to achieve in terms of reproducibility.
This could be done by adding a document to the repository so that it can be modified with the contribution of everybody.

