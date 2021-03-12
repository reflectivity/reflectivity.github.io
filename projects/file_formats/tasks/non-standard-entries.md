---
layout: page
title: ORSO file format - tasks - Non-standard entries and comments
author:
- Jochen Stahn,  
  *Paul Scherrer Institut, Switzerland*
date: 2020-10-08
---

It might be helpful for the user to add extra, non-standard information
to facilitate reading the document and e.g. identifying the raw file
and settings.

An example is the sample alignment given together with the raw file
name (which often is just a non-descriptive number).

The problem is that the keywords introduced for this purpose might 
conflict with future standardised keywords.

Possible solution: non-standard keywords are marked as comment by
a prefix. Any ideas?

## comments

### to *switch off* content

Is there a way to comment out lines? In the sense that they won't be used by
the analysis software.

### to give additional information 

What about a free-form text section of the form

```YAML
comment: |
    Attention! These data have been collected without the frame overlap mirror
    and contain thus some highly structured background.

    Still the peak positions can be analysed.
```

Or shorter comments anywhere in the header.

```YAML
    sample:
        name: Ni1000 
        # there is a big scratch on the surface!
```


