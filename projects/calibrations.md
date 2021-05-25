---
layout: page
title: "Calibrations"
permalink: /projects/calibrations
author: "Andrew McCluskey, Andrew Nelson"
---

In this page, we are looking to harvest information about calibrations that are performed at neutron reflectometry instruments.
For a given calibration, please include the following information:
- What is being calibrated?
- How is the calibration performed?
- An example of the calibrated data and details of how you get this information
- References/Code for the above would be great!

Once you add your calibration, please include yourself in the authors of this page.
 
Things to get you thinking: (if you calibrate/measure something not on this list then please let us know)
 
1. How do you measure the total flux of your beamline?
2. How do you measure the TOF flight distance of your beamline with neutrons (and without?) Neutron TOF only
3. How do you measure the wavelength or wavelengths of your beamline? Similar to 2 but for monochromatic beamlines.
4. How do you measure the polarisation of your beamline? (TOF, monochromatic)
5. How do you check with a non-magnetic sample you get zero magnetic response with polarised neutrons, i.e your zero is zero, and to what precision? (TOF/Mono)
6. If you have a pixelated detector. How do you measure your detector corrections to then flatten the detector response?  (i.e. Correct all pixels to have a similar response, this is sometimes referred to as a flood measurement)
7. If you have a pixelated detector how do you measure the pixel positions (often called a detector map)
8. How do your scan your slit gaps and positions (bit brain dead this one but everyone has to do it) 
9. How do you measure the feathering function for the moderator (I know this is a TOF source thing at pulses sources)
10. How do you measure the resolution function for your beamline.
11. How do you measure the background levels for your beamline. Lots of things here (dark/quiet counts for detector noise, instrument background from cosmic noise, incoherent background during a reflectivity measurement from samples etc.)
12. How do you calibrate your angle stages to ensure that when you move a degree they move a degree.
13. How do you label your polarisation states?
14. How do you measure the lateral coherence length of the beamline. (important for offspecular scattering, or if you have partial surface coverage; [10.1103/PhysRevA.89.033851](https://doi.org/10.1103/PhysRevA.89.033851))
15. How do you measure guide structure. Using pinhole camera setup. Plus a special neutron camera. See [Platypus paper](https://doi.org/10.1016/j.nima.2010.12.075)
 
 
 ## [Platypus TOF reflectometer at ANSTO](https://doi.org/10.1016/j.nima.2010.12.075)
 
 ### Wavelength calibration
 
 1. Optically align the instrument with a theodolite so everything is roughly in a straight line.
 2. Optically align the instrument with neutrons for the collimation slits (slits 2 and 3) by scanning the upper blade past the lower blade and see when counts start to increase, the zero opening is when counts diverge from 0. On Platypus we have ~micron accuracy and reproducibility for these two slits which is essential for measuring samples without a critical edge.
 3. Optically align the instrument for slits 1 and 4 (pre and post collimation slits), such that the slit openings open around the beam defined by slits 2 and 3.
 4. Steps 2+3 might have to be repeated if the beam needs to pass through a certain optical element (e.g. polariser) at a specific place. This is done by moving slits 2 + 3 around until that's achieved, then adjusting 1 + 4 again.
 5. Measure all the TOF flight distances on the instrument using a Laser Tracker. This should get to at least mm accuracy.
 6. Place a highly reflective sample on the sample stage and align the sample. Vary the sample - detector distance (SDD) across its entire range. Plot the difference in pixel number for the direct and reflected beams as a function of SDD. This plot should go through the origin. If it doesn't then enter an offset for SDD until the plot goes through the origin. This step ensures that the overall flight distance is correct because the exact detection plane of the detector is unknown.
 7. Place a sample with Bragg peaks on the sample (in our case we use a Ni-Ti multilayer, we have 4 useful peaks) and align it. Vary SDD and measure spectra at each distance. For each of the four diffraction peaks plot the TOF channel number as a function  of SDD. This plot should furnish 4 straight lines for each of the diffraction peaks. The gradient of the straight line is then used to calculate the wavelength of the neutrons producing each of the diffraction peaks. The gradient of that graph is d_time_channel / d_SDD, you multiply by the time channel length (we use linear time bins on Platypus) to give dt/dx, then take the inverse to give a velocity dx/dt, which then furnishes the wavelength. This step should be carried out at an angle which results in the wavelengths covering as much of the useful wavelength spectrum as possible.
 8. The phasing of the following chopper with respect to the leading chopper is then calibrated by closing the following chopper (such that the leading/opening edge of the following chopper crosses the beam after the trailing/closing edge of the leading chopper crosses the beam, rather than at the same time), and measuring spectra as a function of following chopper phase. The intensity of each of the diffraction peaks (from step 7) is integrated over and plotted as a function of chopper phase. Closing the following chopper has the effect of removing shorter wavelengths from the beam (because they don't get to the following chopper by the time it closes). One can calculate the theoretical phase angle which should extinguish neutrons of a certain wavelength (if one knows the distance between the choppers). This theoretical phase angle is compared to the intercept of integrated peak intensity vs phase angle. The difference in the angles is then used to operate the choppers such that the opening edge of the following chopper crosses the beam at the same time as the closing edge of the leading chopper ("optically blind operation"). For example, on Platypus the disc openings for choppers 1 and 3 are 60 degrees and 25 degrees respectively. Because the T0 pulses are aligned to the middle of the windows we should operate at a nominal phase angle of 42.5 degrees. However, when we do the calibration plot we notice that the integrated intensity for a given peak is extinguished 0.3 degrees too early. This means we need to operate the choppers in a slightly more open fashion, at 42.2 degrees.
 9. The last remaining correction is a phase offset for the leading chopper, the angle from where we believe the T0 pulse originates from, compared to where it actually is. This phase offset shifts the wavelength spectrum to lower/higher values because T0 will be at a slightly wrong time. This angle is calculated by acquiring a spectrum of the Bragg mirror for reasonably good statistics. The leading chopper phase offset is then systematically altered in the reduction code such that the peak locations (which will vary as this phase offset changes) then match those calculated for each of the four peaks measured in step 7. We minimise the sum of least squares differences between the actual locations (step7) and those produced by the reduction code, as a function of leading chopper phase offset. The reduction code for Platypus and Spatz is currently contained in the [refnx project](https://github.com/refnx/refnx/blob/master/refnx/reduce/platypusnexus.py).

### Resolution Function
On Platypus and Spatz the resolution functions are theoretically calculated using [Nelson and Dewhurst](https://doi.org/10.1107/S1600576714009595), but can also be calculated as part of a [Monte Carlo approach](https://github.com/refnx/refnx-models/tree/master/platypus-simulate) for simulating datasets on a TOF reflectometer.
