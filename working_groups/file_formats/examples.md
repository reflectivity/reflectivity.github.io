## Example reflectivity files
  
The examples presented and discussed below are NOT the orso standard.
Their purpose is to illustrate ideas and to form the basis for
discussions. So please feel free to do so!

You might add some comments and own examples with some text here.
Please mention who you are by putting "(Your Name)" at the beginning
of your comment or the section you added.

One major outcome of the workshop in May 2020 was that there should be
2 file formats:

- a comprehensive one, most likely in HDF format, which
  fulfils the principles of inter-operability and ....

- a pragmatic one, which is human-readable and which contains at
  least the most essential information.

#### table of content

- [HDF file](#comprehensive-hdf-file)
- [ASCII file](#pragmatic-ascii-file)
  - [minimum content](#minimum-content)
  - [recommended content](#recommended-content)
  - [optional content](#optional-content)
  
#### how to contribute

You can help to improve the formats in various ways:
- **Add a comment** below the examples. Please start with "(Your Name)".
- Add a commented alternative heading below.
- Send an [e-mail to Jochen](mailto:jochen.stahn@psi.ch?subject="... or so!")
  if you have no writing privileges or are uncertain how to edit the
  markdown source file.
- Write on the [message bord](https://gitter.im/reflectivity/file_formats).
- Try to **realise** the suggested format or something similar in your 
  reduction software. And please report on outcome and issues.

---

<a name="comprehensive-hdf-file"></a>
         
### comprehensive HDF file 

(A. Nelson) under development ...

<a name="pragmatic-ascii-file"></a>

### pragmatic ASCII file 

<a name="minimum-content"></a>

#### minimum content 

It is still under debate what the minimum content of this file should be.
For sure it should contain:

- the **columns** Qz, R(Qz) or I(Qz), and sigma\_R.

- a correct column **labelling** including **units**

- information about the owner/creator

- information which allows to **trace it back** to the raw data files and
  the **reduction software** used. If possible including the parameters
  used for reduction.

- the **probe** (neutrons or x-rays)

and probably also

- a description of the data reduction steps performed so far

- a reference to the comprehensive HDF file

- ...

- a 4th column with sigma\_Qz

A made up example (using YAML for structuring) of a minimum file could look like
[this](pragmatic-minimum.txt) (Jochen Stahn):

    # creator:
    #   name        : Jochen Stahn
    #   affiliation : PSI
    #   time        : 2020/04/06/13:21:18
    # data source:
    #   experiment:
    #     probe              : neutrons
    #     sample:
    #       name             : Ni1000
    # reduction:
    #   software: eos.py
    #     call : eos.py -Y 2020 -n 1925-1927 -y 9,55 ni1000 -O -0.2 -r 1064 -s 1 -i -a 0.005 -e
    # data:
    #   column 1 : Qz / Aa^-1
    #   column 2 : RQz
    #   column 3 : sigma RQz , standard deviation
    #            1               2               3
    1.03563296e-02  3.88100068e+00  4.33909068e+00
    1.06717294e-02  1.16430511e+01  8.89252719e+00
    ...

<a name="recommended-content"></a>

#### recommended content 

On top of this minimum set, a wide variety of information and content
can be given. Some of it in a standardised form, i.e. using the structure and
key words defined in some orso standard.

The rest in almost free form - as long as it does not interfere with the
minimum content and the format rules.

Adding the recommended content the file looks like
[this](pragmatic-recommended.txt) (Jochen Stahn):

    ## reflectivity data file                                 orso file format 0.0
    #
    # creator:
    #   name        : Jochen Stahn
    #   affiliation : PSI
    #   time        : 2020/04/06/13:21:18
    #   system      : lnsa17.psi.ch
    #
    # data source:
    #   origin:
    #     owner              : Jochen Stahn, PSI
    #     facility           : Paul Scherrer Institut, SINQ
    #     experiment ID      : 2019 0042
    #     experiment date    : 2020/02/02 - 2020/02/04
    #     title              : Generation of input for file formatting purposes
    #   experiment:
    #     instrument         : neutron reflectometer Amor
    #     probe              : neutrons
    #       polarisation     : +0.97
    #     measurement:
    #       scheme           : angle- and energy-dispersive
    #       wavelength range : [4.0, 12.0] # Aa
    #       angular range    : [0.3, 2.1] # deg
    #       omega            : 1.2 # deg
    #     sample:
    #       name             : Ni1000
    #       description:
    #         - amb          : air
    #         - layer        : {material: Ni, thickness: 100 nm}
    #         - subs         : Si
    #   links:
    #     related extensive file : fulldatafile.hdf
    #     doi                    : orso2020.123456.789
    #     instrument reference   : doi:10.1016/j.nima.2016.03.007
    #
    # reduction:
    #   software: eos.py
    #     call : eos.py -Y 2020 -n 1925-1927 -y 9,55 ni1000 -O -0.2 -r 1064 -s 1 -i -a 0.005 -e
    #     comments:
    #       - corrections performed by normalisation to measurement on reference sample (-r)
    #     corrections:
    #       - full footprint correction (-r)
    #       - incident intensity (-r)
    #       - detector spatial resolution (-i)
    #     binning:
    #       - Qz range : [:0.01] # Aa^-1
    #         type     : linear
    #         delta Qz : 0.001 # Aa^-1
    #       - Qz range : [0.01:] # Aa^-1
    #         type     : exponential
    #         calculus : Qz_n = 0.01 * (1+0.005)^n # Aa^-1
    #
    #   input files:
    #     references:
    #       - file     : amor2020n001064.hdf
    #         created  : 2020/02/02/15:38:17
    #     datafiles:
    #       - file     : amor2020n001925.hdf
    #         created  : 2020/02/03/14:27:45
    #       - file     : amor2020n001926.hdf
    #         created  : 2020/02/03/14:37:15
    #       - file     : amor2020n001927.hdf
    #         created  : 2020/02/03/14:27:02
    #
    #   data state:
    #     footprint    : corrected
    #     intensity    : normalised
    #     resolution   : not corrected for, see column sigma Qz
    #     resolution   : calculated, based on wavelength resolution and detector spatial resolution
    #     absorption   : uncorrected, not needed
    #     background   : uncorrected
    #
    # data:
    #   column 1 : Qz / Aa^-1
    #   column 2 : RQz
    #   column 3 : sigma RQz , standard deviation
    #   column 4 : sigma Qz / Aa^-1, standard deviation
    #            1               2               3               4
    1.03563296e-02  3.88100068e+00  4.33909068e+00  5.17816478e-05
    1.06717294e-02  1.16430511e+01  8.89252719e+00  5.33586471e-05
    ...

<a name="optional-content"></a>

#### optional content

And including some user-defined content leads to
[this](pragmatic-options.txt) (Jochen Stahn):

    ## reflectivity data file                                 orso file format 0.0
    #
    ...
    #
    # misc:
    #   resolution :
    #     - description : "Gaussian wit freely selectable integral, offset and sigma"
    #     - calculus    : f(q) = a * exp( - (q-b)^2 / c^2 )
    #
    # data:
    #   column 1 : Qz / Aa^-1
    #   column 2 : RQz
    #   column 3 : sigma RQz , standard deviation
    #   column 4 : sigma Qz / Aa^-1, standard deviation
    #   column 5 : parameter "a" of the resolution function
    #   column 6 : parameter "b" of the resolution function
    #   column 7 : parameter "c" of the resolution function
    #            1               2               3               4               5               6               7
    1.03563296e-02  3.88100068e+00  4.33909068e+00  5.17816478e-05  1.00000000e+00  4.00000000e+00 -2.00000000e+00
    1.06717294e-02  1.16430511e+01  8.89252719e+00  5.33586471e-05  1.10000000e+00  4.10000000e+00 -2.10000000e+00
    ...

The content of the misc section is not defined by orso. But this allows
to provide information to more sophisticated analysis software. Of cause
this software must *know* how to read and handle this input.

