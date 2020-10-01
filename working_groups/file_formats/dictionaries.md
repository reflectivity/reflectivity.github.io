# ORSO standard for reflectometry data files



## human-readable data file

The header should be formatted using [YAML](https://en.wikipedia.org/wiki/YAML). 

### Structure of the header

The first line should state what the file is. E.g.   
`#reflectivity data file                  orso file format 0.0`

The header is structured into information on the

- creator
  : ownership of the data file

- data source
  : ownership and provenience of the raw data

- reduction
  : software and reduction steps

- misc
  : non-orso content

- data
  : column description and units

using key words and structure as listed in the dictionary.

And finaly a one-line column description refering to the data section
of the type   
`#     1 Qz    2 RQz   3 sRQz    4 sQz        5 ...`   
or just  
`#        1        2        3        4        5 ...`   



### Dictionary of the key words used in the header 

creator
: (required)
: This section referes to the creation of this *file*, not the data.

    name
    : (required) NX_CHAR
    : Name of the person who created this file

    affiliation
    : (optional) NX_CHAR
    : Affiliation of the person who created this file

    time
    : (optional) NX_DATE_TIME
    : Date and time of the creation of this file

    system
    : (optional) NX_CHAR
    : Computer and user who created this file

data source
: (required)
: This section deals with the source of the data used for generating this file. 

    origin
    : (required)
    : This referes to the legal ownership of the raw data.
       
        owner
        : NX_CHAR
        : Name of the owner of the raw data facility
        : Name of the facility where the measurement has been berformed.

        experiment ID
        : (required, if applicable) NX_CHAR
        : The proposal number or experiment ID under which the data were cllected.

        experiment date
        : (optional) NX_DATE_TIME
        : Dates when the experiement was performed (the whole period rather than the
        individual measurement). 

        title
        : Title of the experiment / the measurement campain.   

    experiment
    : (required)

        instrument
        : Name and if applicable type of the instrument used.

        probe
        : (required)
        : Radiation used during the experiemnt. Either `neutrons` or `x-rays`.

            polarisation
            : (optional)
            : For neutrons the polarisation might be given as *+1* for fully spin up
            polarised, *-1* for fully spin down polarised and *0* for unpolarised. 
            : Partial polarisation can be expressed as ....

        measurement
        : (required)
        : How and parameters

            scheme
            : (optional)
            : Measurement scheme / geometry. This might be angle- of erergy 
            dispersive, or both.

            wavelength range
            : (optional) NX_FLOAT
            : Value and unit for angle dispersive scheme
            : Format `<value> # <unit>`
            : Value range and unit for wavelength dispersive scheme.
            : Format `[<lower limit>, <upper limit>] # <unit>`

            angular range
            : (optional) NX_FLOAT
            : Value and unit for wavelength  dispersive scheme
            : Format `<value> # <unit>`
            : Value range and unit for angle dispersive scheme.
            : Format `[<lower limit>, <upper limit>] # <unit>`

            sample
            : (required)
            : Description of the measured sample.

                name
                : (required)
                : A name uniquely identifying the sample

                description
                : (optional)
                : Nominal composition of the sample if known.
                : Format suggestion following GenX nomenclature   
                `- amb: air`   
                `- layer: {material: Ni, thickness: 100 nm}`   
                `- subs: Si`   

            links
            : (optional)
            : List of links to related data, publications, instruments and so on.
            Free format, e.g.  
            `related extensive file : fulldatafile.hdf`  
            `doi                    : orso2020.123456.789`  
            `instrument reference   : doi:10.1016/j.nima.2016.03.007`  

reduction
: (required)
: Information on the reduction steps performed to obtain the data 
below from the raw data set(s) listed here.

    software
    : (required)
    : Name and version of the software.

        call
        : (required)
        : Echo of the call of the software or soemthing similar which allows 
        to reproduce the data content of this file.

        comments
        : (optional)
        : Plain text with comments about the data reduction.
        This allows to explain details of the reduction algorithm or
        what assumptions have been made.

        corrections
        : (optional)
        : List of reduction steps that have been performed. Probably with
        reference to a standadised procedure (orso repository) or to a
        publication.

        binning
        : (optional)
        : Description of the binning applied to the data. 
        : several ranges require a repetition of the block.

            Qz range 
            : [:0.01] # Aa^-1

            type
            : linear

            delta Qz
            : 0.001 # Aa^-1

    input files
    : (required)
    : Data files used for creating the data below.

        references
        : (required if applicable)
        : List of files used for normalisation of the data.

              file
              : File name

              created
              : Date of creation (measurement?) of the raw file 
              : Format YYYY/MM/DD:hh:mm:ss

        datafiles
        : (required)
        : List of files containing the raw data.

              file
              : File name

              created
              : Date of creation (measurement?) of the raw file 
              : Format YYYY/MM/DD:hh:mm:ss

    data state
    : (optional)
    : key word like summary of the reduction steps 
    : Format `<key word> : <comment>

misc
: (optional)
: Optional section to be used with non-orso-standard key words.

data
: (required)
: Column description and data array containing the reduced data and related quantities.
: The content of columns 1 to 4 is defined. Further columns may contain 
  whatever the creator wants - as long as it is clearly stated what it is 
  and what the units are.

    column 1 
    : (required)
    : Must be one of `Qz`, `alpha_i` or `lambda`. 
    : Together with the unit, i.e. `nm^-1`, `Aa^-1`, `deg`, `rad`, `nm` or `Aa`.

    column 2 
    : (required)
    : Must be the reflectivity or intensity as a function column 1.
    : If applicable with unit.

    column 3 
    : (required)
    : Must be the uncertainty of the quantity in column 2. 
    : This might be the standard deviation (sigma), FWHM, or the like. 
    : Including appropriate units.

    column 4 
    : (optional, but defined if present)
    : If available the uncertainty of the quantity in column 1.
    : This might be the standard deviation (sigma), FWHM, or the like.
    : Including appropriate units.

    column 5
    : (optional)
    : ...

