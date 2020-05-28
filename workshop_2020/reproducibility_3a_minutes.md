---
layout: page
title: "Reproducibility 3a Minutes"
permalink: /workshop_2020/reproducibility_3a_minutes
---

introduction of everyone

Chair: Andrew Nelson (AN)
Scribe: Andrew McCluskey (ARM)

Members:
Thomas Saerbeck (TS)
Gemma Guest (GG)
Gaetano Mangiapia (GM)
Jean Francois Moulin (JFM)
Alex Koutsioumpas (AK)

AN is outlining direction
- way forward that is implementable 
- how to evangelise?
- how to help users?
- what do we expect wrt peer review?

In previous meeting there was a discussion of reproducible samples before going onto reproducible of a journal article.

Reproducibility of analysis is becoming a requirement for publication. FAIR data etc.

What is necessary for "reproducible" analysis:
- raw data?
- history of reduction?
- reduced data?
- analysis mechanism (script/notebook)?
- documentation of analysis?
- how is resolution smearing implemented?
- better reduction algorithm documentation

AN has presented the bioSAXS publication guidelines in Acta Cryst D. 
- more important for biological scattering
- but does have requirements for data analysis (including data should be submitted)

What are the steps for our community:
- our users might not be as computationally literate as we would like

TS 
- list is good (but exhaustive)
- touches on "proper data analysis"
- we should agree to start on a list of items that a user/author should include to "reproduce" the data
- from sld to reflectivity, including formalism
- institute should provide reduction information
- user/author should provide analytical information

AN 
- analysis with a notebook/script computers are good at reproduction
- but this is more effort 
- should exact steps be the BASE?

TS
- not EXACT steps but referenced
- methods include instrument/reduction/analysis package
- the references give this information
- you need exact parameters to reproduce analysis 
- we should give guideline (document) for the information the users will include in paper 

GM 
- version of software should be included

GG 
- mantid versions algorithms and software
- this is stored in the nexus analysis 
- difficult to do in a single project
- saving to ASCII obviously does not do this, becomes instrument scientists problem

TS
- journals are moving to force versioning from users and better documentation
- a standard list of parameters for users would be useful for publication and dealing with this from journals. 

AN
- are reduced data files a minimum?

TS
- disagreed
- what does that help?

AN
- if I don't trust the analysis, I can redo it myself
- test their conclusions 

TS
- your don't need the reduced data
- the referee should be checking this 
- how do you interpreate data?
- is chasing up false interpretations is a dodgy aspect
- the interpretation is the SLD profile
- fit doesn't match data OR SLD doesn't match interpretation

AK
- in the bioSAXS community
- users need to upload data to publish
- to upload data some requirements are necessary
- we can create a repository to store data
- we need a format for reporting a result SLD?
- a pdb for reflectivity 

ARM
- For analysis to be reproducible you need reduced data

TS
- yes

JFM
- reduced data is pretty small
- this is easy and cheap (we should do it!)

GG 
- what do we mean by including data?

AN 
- "the data is available @ x"

GG
- data is on a institutional repo  

JFM
- every facility has storage to x years
- when you publish data for beamline, the institute should provide data access
- but we need reduced data, cause information from raw to reduced is very hard

AN
- file format could have reduction info

TS
- file formats discussed data policy and europe is consistent 
- not sure about US
- include raw, meta and reduced data
- handled carefully 
- restricted access
- do we want to rewrite data policy 

ARM
- not just institute data also journal policy

AN
- what is the lowest bar we would accept for reproducibility?
- mechanistic analysis?

TS
- sld graph is not easily digitised
- a table is better at least in SI
- data should be shown with a fit
- general description should include assumptions and formalisms

AN 
- not dissimilar to bioSAXS
- is script too high a bar?

JFM
- nice option but software dependent?

ARM
- if the best practice is possible they should do it

AN
- important to have concrete steps
- start working on a set of principles/document which outlines this
- in future this can become a paper
- needs the whole community

GG
- we need better software tools, capture the information in a gui
- the gui should be the front end to a script

AN
- mantid is a bit hard to descipher, documentation is vital

AN 
- start a MD file to work towards a "paper"
- encourage all to contribute
- how else can we move this forward?

TS
- document on github is one thing
- teaching starts with users
- there are implementations where mantid produces a gui script
- this is given to the users 
- the analysis software does not strictly do that
- lets tell the analysis code people to do this

AN 
- it is possible to do, refnx has this
- refl1d possibly does something similar

GG 
- we need to educate software engineers too
- is there propriatory software that can mess with things

AN 
- if it is not open it is not worthwhile

JFM 
- agrees

AN
- rascal uses MATLAB which is questionable but passable

TS
- I use closed code
- include calculations in the paper
