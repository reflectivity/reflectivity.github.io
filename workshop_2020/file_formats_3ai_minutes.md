---
layout: page
title: "File Formats 3a(i) Minutes"
permalink: /workshop_2020/file_formats_3ai_minutes
---


### 2020-05-27 at 15:30 UTC

### Participants:

- Alexandros Koutsioumpas
- Becky Welbourn
- Gaetano Mangiapia
- Max Skoda
- Thomas Saerbeck
- Tim Snow (Chair and scribe) 

### Session Agenda

- Flexibility (units, symbols)

> - Formulate rule

- Redundancy (allow it? which is the "master" information?)

> - Formulate rule


### Key Points Arising

It was observed that, typically, the wave vector, Q/q, is typically reported/given in inverse Angstroms. Aside from this being the predominant convention within the community this also makes it consummate with the units for scattering length density.

For reflectance (AK - is it normalised to one…?) perhaps a software 'switch' could be used to move from arbitrary units (or some form of intensity unit) to a 'normalised' unit.

It was noted that absolute reflectivity is a problem for TOF instruments as the distribution of wavelengths isn’t uniform and also the delay due to chopping leads to questions about what is the true reflectance size of sample (sample acting as a slit), *i.e.* it alters the distribution of the wavelengths subtended by the TOF beam

A discussion was then had around the idea that if no units are present you could normalise to one, however, if units are given respect the units. The counterpoint was that reflectivity is never exactly one it’s close to one as there’s always absorbance

Regarding a specific file format, there appears to be a number of contenders: *e.g.* YAML, NeXus, DAT, XML, and commercial file formats

ASCII seems to be the winner as it’s the most flexible but comes with the greatest number of drawbacks. However, as it is the most commonly used format there should be provision for this *now* (and column titles should always be given with units), however, for beamline staff today there should be a second file (preferably HDF5/NeXus) with data processing provenance information alongside the processed data and, perhaps, a way of storing partially processed as well (intermediate datasets would remove the need to reprocess all the data every time). Then in future, when analysis software can read these files and when journals require more in an article's ESI, we are also ready.

It was noted that such an approach would save us time today, when users contact us with queries, and tomorrow, when they'll need it.

The final topic of discussion was multiple simultaneous measurements on the same q scale (e.g. multiple magnetic sensitivities). Would there be provision in the ASCII standard for more than 4 columns, *i.e.* multiple datasets per file, or are we to split each dataset into a separate file?
