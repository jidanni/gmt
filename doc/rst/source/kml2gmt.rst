.. index:: ! kml2gmt
.. include:: module_core_purpose.rst_

*******
kml2gmt
*******

|kml2gmt_purpose|

Synopsis
--------

.. include:: common_SYN_OPTs.rst_

**gmt kml2gmt** [ *kmlfiles* ]
[ |-E| ]
[ |-F|\ **s**\|\ **l**\|\ **p** ]
[ |SYN_OPT-V| ]
[ |-Z| ]
[ |SYN_OPT-bo| ]
[ |SYN_OPT-do| ]
[ |SYN_OPT-h| ]
[ |SYN_OPT-:| ]
[ |SYN_OPT--| ]

|No-spaces|

Description
-----------

**kml2gmt** reads a Google Earth KML file and outputs a GMT table file.
Only KML files that contain points, lines, or polygons can be processed.
This is a bare-bones operation that aims to extract coordinates and
possibly the name and description tags of each feature. The main use
intended is to capture coordinates modified in Google Earth and then
reinsert the modified data into the original GMT data file. For a more
complete reformatting, consider using
``ogr2ogr -f "GMT" somefile.gmt somefile.kml``.

Required Arguments
------------------

*kmlfiles*
    Name of one or more KML files to work on. If none are given, then
    standard input is read.

Optional Arguments
------------------

.. _-E:

**-E**
    Get the altitude from the *ExtendData* property; *z* coordinates are then ignored. KML provides
    more than one mechanism to store information via *ExtendData* but here we only implemented the
    *<SimpleData name="string">* variation. Implicitly sets |-Z|

.. _-F:

**-F**\ **s**\|\ **l**\|\ **p**
    Specify a particular feature type to output. Choose from points (**s**),
    **l**\ ine, or **p**\ olygon.  By default we output all geometries.

.. _-Z:

**-Z**
    Output the altitude coordinates as GMT z coordinates [Default will
    output just longitude and latitude].

.. |Add_-V| replace:: |Add_-V_links|
.. include:: explain_-V.rst_
    :start-after: **Syntax**
    :end-before: **Description**

.. |Add_-bo| unicode:: 0x20 .. just an invisible code
.. include:: explain_-bo.rst_

.. |Add_-do| unicode:: 0x20 .. just an invisible code
.. include:: explain_-do.rst_

.. |Add_-h| unicode:: 0x20 .. just an invisible code
.. include:: explain_-h.rst_

.. include:: explain_colon.rst_

.. include:: explain_help.rst_

Notes
-----

If polygons are digitized you can enter them in any order. If you have both perimeters
and holes we suggest you run the output through **gmt spatial -Sh** which will
determine which polygons are perimeters and holes and organize them so that
any holes immediately will follow their perimeters and that their segment headers will
contain the **-Ph** flag and have the reverse handedness.  This output may be
plotted by :doc:`plot` and the holes will be honored.

Examples
--------

.. include:: explain_example.rst_

To extract the lon,lat values from the KML file google.kml, try

::

  gmt kml2gmt google.kml -V > google.txt


To separate the point and polygon geometries from the KML file google.kml, try

::

  gmt kml2gmt google.kml -Fp -V > polygons.txt

  gmt kml2gmt google.kml -Fs -V > points.txt

See Also
--------

:doc:`gmt`, :doc:`gmt.conf`,
:doc:`img2google <supplements/img/img2google>`,
:doc:`psconvert`, :doc:`gmt2kml`, :doc:`gmtspatial`
