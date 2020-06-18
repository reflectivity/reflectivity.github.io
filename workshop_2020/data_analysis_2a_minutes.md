Session 2A
==========
Andrew Nelson scribing

<describes agenda toward a shared calculation engine>

Brian Maranville, Tom Arnold, Wojtek (ESS), Jochen Stahn (AMOR, convergent beam), Artur Glavic (maintains GenX), John Ankner (LR, SNS/ORNL), Juan Manuel Cloiza (BornAgain developer, working with Wojtek), Paul Kienzle (NCNR, interested in optimisation analysis), Francesco Carla (I07)

Stahn interested in convergent beams with TOF, which has its own challenges.

- does it make sense to make a combined reflectivity kernel. Maybe have a reference implementation? PaulK says that there are things that one has to be careful with, such as branch cuts of complex numbers.

discussion of branch cuts on complex numbers and why they are important.

Validation repo checks refl1d, refnx, BornAgain for specular NR. It needs more examples. Validate resolution smearing of data (non gaussian distribution functions might be important)

John Ankner says that higher level aspects such as roughness implementations (Nevot Croce vs slicing) is an interesting subject for more investigations, do the two give the same output?

Had an (off topic) discussion on full resolution kernel calculations and whether they would be stored.

Comparison + validation + work towards (common?)

- resolution function
- roughness implementation


JohnAnk would like a common language of constraints that aren't necessarily in terms of SLD. e.g fitting density and chemical composition.

Wojtek - had question on validation. We then discussed developing the validation repo in more directions/featues (resolution, checking chi2, different roughness, a fit with parameter uncertainties). Develop to a more complicated level, so that individual analysis packages can import those tests as part of their unit testing. All contributions worthwhile.

Artur - Perhaps it's just better to write translators for various kernels.

Comment from Brian that validating might encourage various packages to converge.
 
