---
layout: page
title: "Data Analysis 1b Minutes"
permalink: /workshops/workshop_2020/data_analysis_1b_minutes
---

Participants:
- Arwel Hughes (AH)
- Andrew McCluskey (AM; Chair)
- Andrew Nelson (AN)
- Christy Kinana (CK)
- Tim Snow (TS; Scribe)

### Minutes

AH – Validation of analysis is key. Confidence is needed in calculations and the best way to do this is cross checking of algorithms against each other

AN – Agreed, however, is this possible without first having an agreed vocabulary as some terms, such as resolution smearing can mean different things to different people

AM – Don’t we already have a shared vocabulary for slabs?

AN – We might have for soft matter / non-magnetic systems but different disciplines of reflectivity have different nomenclatures

CK/AH – This is true as there are more degrees of freedom and a more complex system to describe however, to date, there has been little progress in unifying a common language for these systems (although discussions on this have started)

AN – Have looked at other packages and have set up a cron job in GitHub to perform comparisons on ‘simple’ slab systems for some packages, however, advanced features (e.g. resolution smearing) have yet to be tested as there are different approaches in the code to these analyses – could this provide a springboard towards a conversation for unifying on the terms and approaches used

AM – Does this hint towards that vocabulary, dialects and verification are all needed and that there is a natural order, as alluded to, in which this should be worked upon

AN – Whilst all of this is good, we also need community buy in to look at, review and extend the code present by other members of ORSO so that it is owned by the community and not just one person

AM – A next logical step could well be to investigate resolution smearing

AH – Whilst maybe true, everyone has a different approach and everyone is making slightly different assumptions and therefore this presents itself as the area of greatest potential deviation

AN – The resolution function of my beamline is not a Gaussian it’s more of a trapezoidal function (typically), however, with higher resolution choppers it starts to approximate a Gaussian more closely

AH – How are you convoluting this with your data? Are you convoluting on a an inverse-Fourier transform?

AN – Yes and no; there’s a problem that although such functions can be obtained problems arise as there are many variables that are q-point/q-space specific

All – This points towards the need to measure and publish beamline resolution functions so that it is possible to perform validation of datasets against each other by having these functions present to work against

AM – For a number of systems the adoption of the slab model has made it easier to agree on a vocabulary, however, for magnetic measurements this might not play so well as such systems aren’t described by this so well

AN/AH – Whilst AM’s comment is true, it should also be noted that there’s no reason as to why such a model/description should be able to be extended to facilitate a full vocabulary (in time)

AM – Are we falling too far into dialects here? If we’re describing a ‘simple’ system (such as a lipid monolayer) can’t we use an appropriate dialect?

AH – Doesn’t micro-splining destroy this approach, if we go from a simple 2/3 layer system and add roughness won’t the code create a series of thin layers to deal with the interface between layers

AM – Isn’t this just a rehash of saying I put this ‘simple’ system into this analysis code and here’s the procedure it followed?

AM – Following on from this, would it be good to get two distinct groups of people together to discuss vocabulary; one representing magnetism and another not

AH/AN – Perhaps, however, this only works for ‘simple’ systems – as soon as complexity is added, such as partial surface coverage, a number of assumptions and linguistic terms might well (or certainly do) fall apart

AM – Doesn’t this fall back into the dialects argument again? Should we be bringing relevant people together and then put this to some form of vote, or consensus driven approach?

AH – Single component vs. mixed monolayers might be the downfall of this approach as this are commonly analyses systems but now requires two dialects to understand them, however, this can provide downstream problems for publication as this makes verification of the results difficult

AM – Perhaps a different approach might be to ask people to translate from a specific dialect and into our general vocabulary for publication

AH/AN – At which point any analysis software could interpret the data, deduce an appropriate dialect which may well provide a different result

AH – When the SLD profile is constructed, regardless of inputs for a moment, this is what the simulation is run against. However, how we end up at this point, and the number of degrees of freedom in translating from a to b, is where the complexity lies and is the most basic route to obtaining different results

AM – So, are we saying that defining dialects is not worth pursuing as this problem is too poorly posed and, instead, should we provide a base vocabulary and allow researchers to work on their own dialects

AN – Devil’s Advocate, what about ML?

TS – Tread carefully, this is full of caveats

AH – Why would you do an experiment if you don’t know what you’re trying to analyse?

AN – If, for a given model, inputs are given, would it be possible to allow an ML algorithm to bridge the gap between a more simple model to a complex model as it will have access to a ‘higher’ level of visualisation of the overall system landscape

All – There’s a lot of benefits and drawbacks into being able to utilise a broad number of analysis techniques, however, Bayesian approaches are known and understood with rigorous mathematics underpinning it. ML is a black box which, whereas one can share the training model, is ultimately doing ‘something’ and arriving at an ‘answer’. There may also be implications with regard to the amount of time taken to perform all the required calculations.

AM – Back to verification, is there anything that we’ve missed here?

AN – Everyone finds problems, some underlying libraries or operating systems can lead to inconsistencies

AH – We should always be open about such inconsistencies and why and how they arise

AM – Perhaps a ‘gotcha’ page with common inconsistencies might be of use in order to highlight certain issues and provide clarity for the wider community as to what to look out for when developing their own algorithms

**Actions:**

1. Agreement was reached on the prioritisation of tasks
2. Verification should include comparison of roughness calculations and resolution considerations
3. A shared vocabulary should be split into a magnetic and non-magnetic lexicon
4. The non-magnetic vocabulary is already, largely, defined and so should have a group created to write a white paper that can be discussed at a future meeting
5. The magnetic vocabulary will require more thought however with a group assembled to “hash it out”
6. Domain dialects could be as broad as the systems themselves, we should focus on a vocabulary and suggest the inclusion of this in publications (alongside domain specific language)
7. It was also highlighted that a ‘gotchas’ page for developers/analysts could be developed to highlight differences in results arising from differences in underlying algorithms, dependencies, OS/platform inconsistencies, etc.
