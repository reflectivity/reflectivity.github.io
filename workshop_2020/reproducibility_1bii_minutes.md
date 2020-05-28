---
layout: page
title: "Reproducibility 1b(ii) Minutes"
permalink: /workshop_2020/reproducibility_1bii_minutes
---

ORSO Reproducibility Session 1
Present at the session were.
Andrew Nelson, Tim Snow, Jean-Francois Moulin, Gaetano Mangiapia, Max Skoda, Norifumi Yamada, Adrian Rennie and Robert Dalgliesh.
The session was tasked with discussing and providing answers to a couple of very specific questions which it largely turned out were fairly generically answered 
Define a list of requirements for a reproducible measurement standard and how it would be documented.

The reproducibility of measurements was largely discussed through the method of using round robin samples and then the suggestion of developing certified standards
The current round robin uses a sample manufactured at NIST consisting of a 2” silicon block coated in an inorganic nitride chosen for its high SLD, scratch resistance and lack of oxidation potential making it chemically stable and well suited as a standard. The round robin has been going on for a number of  years and has been extremely instructive to a number of instruments revealing subtle details of the instrument performance. The samples are currently at NIST and are then due to travel to France. Eventually the data will be taken and used to produce a publication but it is proving difficult to get the data back from people. A renewed effort needs to be made to complete the job.
Beyond the round robing the question of enabling each facility to have a “certified” sample was discussed and compared to the NIST certified SAXS and SANS standards. Adrian Rennie discussed the way that these standards came about driven by the needs of the SAXS community and coordinated/influenced by the work of CANSAS. It was pointed out the workload is considerable  and uniformity must be checked leading to a considerable amount of work which will make things expensive. Max Skoda has already  enquired about obtaining a sample similar to the round robin one for ISIS which is already looking a multiple standards to be run as often as possible to ensure consistent instrument performance but has had no luck to data.
In terms of the documentation required  the NIST standard examples give a clear idea as to what would be needed to give confidence that a sample has been checked but if we are to undertake a process of developing a standard this work will need to be stored centrally on the github site along with a  suitable publication.
It was also noted that members of the BioSAS community have written a paper of what they think is required for the minimal specification of reproducible data. However, after brief subsequent discussion it should be noted that this paper was not widely discussed through e.g. CANSAS.
  

Define a list of requirement for the reproducible analysis of data

The reproducibility of data analysis was discussed based around the work already deposited into the Github repository by Andrew Nelson with respect to his recent publication on RefNX.
It is expected that journals will soon be demanding documentary evidence of analysis workflows that would enable another scientist to work their way through the analysis pathway, to be able to repeat the analysis using the same programs or to look at alternative.
The work deposited on Github makes use of iPython Notebooks and the scriptability of RefNX to ensure reprocibility. It was noted that this is very important because GUI interfaces are inherently more prone to reproducibility errors. i.e. A gui interface should be able to output an exact script that would reproduce an analysis with all the steps and settings used.
The demands of reproducibility also make the need for clear and accurate documentation of algorithms more necessary when different pieces of software might be used for cross comparison of analysis.  
Identify individuals who will continue to work on these projects 
1.	Move towards a definition of this measurement standard
Adrian Rennie, Tim Snow and a representative from ISIS will investigate standard samples further. Swiss Neutronics ir the German standards agency were suggested as possible route.
2.	Develop a standardised protocol for producing such a ”workflow”
Andrew Nelson will set up a living document on the Github repository to enable the ongoing discussion of the steps required for a reproducible data workflow. All those involved in ORSO will be encouraged to feed into the document.


