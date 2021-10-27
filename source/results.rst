Results
=======
Results are always returned in XML-file. Such file will contain URL's from which the generated files can be downloaded.

Search
------
Search returns metadata for available FITS-images or FITS-cubes which satisfied the search query conditions.
The XML-response file for search is build up as follows:

::

        <result>
                <description>
                <input>
                <msg>
                <DatacubeCount>
                <survey>
                        <datacube>
                        <datacube>
                        ...
                </survey>
                 <survey>
                        <datacube>
                        <datacube>
                        ...
                </survey>
                ...
        </result>


Found datacubes are grouped by surveys. For convenience, input parameters are repeated and count of found datacubes
is also given. Each survey also contains its descriptive metadata: its name, species, transition including
rest-frequency and velocity unit.

The datacube node provides the following information:

- Overlap: quality of overlap (full-, partial-, no-overlap etc.)
- PublisherDID: unique identificator of the datacube (used in cutout)
- Datacube-bounds: in galactic sky coordintes and velocity on the spectral axis
- Datacube-vertices: in galactic sky coordintes and velocity on the spectral axis

Based on this data, client selects which datacubes are needed, and can perform a cut to
retrieve only the required data.

The PublisherDID serves as datacube identificator. Clients should consider it opaque and not interpret
it, only store and use where required (cutout, merge).

Cutout
------
The XML in response has the same preamble as search (nodes from begining until first <survey>).
Then two nodes:

- CUT : FITS-filename from which cut originates with pixels coordinates
- URL : this url downloads the cutout file
- nullValues: optional node, which shows percentage of undefined pixels in the cut region

Merge
-----
The XML contains the cubes identified by pubdid and lying in the requested region and
being part of the sub-survey. It shows the cuts used from all files (one CUT-node for each).
Finally, the URL-node contains the merged file ready to be downloaded by the client
(like in in cutout).

