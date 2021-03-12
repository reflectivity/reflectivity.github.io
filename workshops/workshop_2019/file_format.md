---
layout: page
title: "A shared file format"
permalink: /workshops/workshop_2019/file_format
---

### What is the scope?

The first discussion was focused on outlining a proposed scope for a share file format.

It was determined that the following must be present in a shared file format:

- Wavevector (q)
- Reflected intensity (r)
- Uncertainty in reflected intensity (dr)
- Uncertainty in wavector or resolution function (dq)
- Ideally both the uncertainties would include a functional description
- Units must either be included in the file or agreed upon

Possible additional information would be:

- Wavelength (λ) and uncertainty (dλ)
- Angle (θ) and uncertainty (dθ)
- Time of flight
- Gravity
- Beam profile

Reflected intensity may be affected by polarisation
- The polarisation should be recorded in the file as an extra dimension
- The spin-corrected data should be available

The background correction that is performed should be done before the output, but documented in some fashion.
- Futhermore, the software used and filenames of raw data should also be recorded

There should be a single format to cover both X-ray and neutrons.

The reduction procedure should be codified and included to ensure reproducibility

A unique descriptor (epoch time) should be associated with every file.

Metadata that would be desirable:

- Sample environment (temperature, pressure, etc.)
- Time
- Sample dimensions (shape), what the beam sees
- Fronting and backing media (and physical state)

### What are the conventions?

The second discussion in this section developed possible conventions to be used in the file format. This discussion still requires substantial work.

The following conventions were discussed and generally agreed on:

- Q<sub>z</sub>: Wavevector perpendicular to the interface
- Q<sub>x</sub> and Q<sub>y</sub>: Wavevectors projection of the beam on the sample
- R: Normalised dimensionless reflected intensity
- I: Unnormalised reflected intensity
- I<sub>dev</sub>: Standard deviation of I
- R<sub>dev</sub>: Standard deviation of R
- dQ<sub>z</sub>: Resolution on Q<sub>z</sub>
- Q<sub>dev</sub>: Standard deviation of Q
