Query String parameters
=======================
Service will parse URL's query string section for parameters. HTTP header fields are not observed.
Note, that some parameter values might contain characters whith special
meaning in client's environment. Follow the usual rules for encoding for your client.


Circular search
---------------

Search is performed by specifying a circular region in galactic coordinates by its center
and radius [l,b;r] in degrees. Paramters are: 

::

        ?l=_value_&b=_value_&r=_value_

Result is a list of datacubes whose area in glactic coordinates overlaps with the
specified circle.
Value of the circle radius must be positive and below 2 degrees. Special case of r = 0 is 
equivalent to point search.

Cylindrical search
------------------

Search is performed by specifying a circular region in galactic coordinates by its center
and radius [l,b;r] in degrees and portion of spectral axis by velocity's lower and upper
bound [vl,vu] in km/s.

::

        ?l=_value_&b=_value_&r=_value_&vl=_value_&vu=_value_

this will return a list of datacubes which overlap with the cylinder specified by
[l,b;r] and [vl,vu].
Specifying for [vl,vu] theoretically possible lowest/highest velocity is equivalent to the
circular search.

Rectangular search
------------------

General rectangular search is defined by a rectangle given by its center [l,b] and width
[dl,db]:

::

        ?l=_value_&b=_value_&dl=_value_&db=_value_

All values are decimal numbers in units of degrees. The values of dl and db must be
positive. Special case when dl=db=0 is equivalent to point search above.

Narrowing the scope of search
-----------------------------

Search requests as described above will search in all database. Any of the search
types can be narrowed down by survey name and/or species and/or transitions by
adding all or some of the following paramters:

::

        surveyname=_string_&species=_string_&transition=_string_

Accepted values are always strings. For actual values consult the TAP service.


Cutout
------

Cutout is specified by a PublisherDID and a region of interest. It is always performed
against one specific datacube per request. Request parameters are:

::

        ?pubdid=_string_&<any of the parameters used in search>

where pubdid is the PublisherDID returned in the response of the search and
identifies uniquely the datacube to be cut.

The result of a cutout is a FITS file. Note that FITS file headers naturally encode
rectagular regions, so even if the region specified in input is a circle, data
corresponding to square which encloses the input circle is returned.

The generated cutout FITS files are always derived from the original so their Header-
encoding follows the original FITS file encoding. From this, exception is only made if
the original encoding was not fully compatible with the reference papers (E. W.
Greisen, M. R. Calabretta, F. G. Valdes, and S. L. Allen : Representations of spectral
coordinates in FITS, Astronomy and Astrophysics 446, 747...771 (2006)).
Specifically, it is made
sure that the cutout file always contains CUNIT3 for the velocity access and RESTFRQ
keywords, even if the oriiginal files do not contain these keywords.

The URL to the generated cutout fits file is returned in XML response under \code{<URL>}
tag. Response also contains \code{<CUT>} tag, which designates the original file and pixel
coordinates of the cut.


The cutout service also accepts an optional parameter:

::

        nullvals <has no value>

The presence of this parameter in the cutout URL will trigger counting of undefined
values in the cut cube. The retunred XML will contain <nullValues> node givining total
number of pixels, number of pixels which are undefined and for convenience a percentage value
(calculated from the previous two).


Merge
-----

The merging service is defined by sky- and optional spectrum-coordinates and a list of PublisherDID's
(separated by semicolon):

::

        pubdid=PublisherDID;PublisherDID;PublisherDID

If the data covering those coordinates are stored in different FITS-files,
the service will perform reprojection to a common space including eventual
re-gridding. Finally it returns one FITS-file
with data merged from several FITS-files.

Such functionality is possible only for consistent data. Data of a 'sub-survey' are
considered consistent and so are mergeable. A sub-survey is defined by a triple:

::

        sub-survey = survey + species + transition

So, for merge these parameters are mandatory.






