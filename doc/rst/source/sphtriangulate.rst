.. index:: ! sphtriangulate
.. include:: module_core_purpose.rst_

**************
sphtriangulate
**************

|sphtriangulate_purpose|

Synopsis
--------

.. include:: common_SYN_OPTs.rst_

**gmt sphtriangulate**
[ *table* ]
[ |-A| ]
[ |-C| ]
[ |-D| ]
[ |-L|\ *unit* ]
[ |-N|\ *file* ]
[ |-Q|\ **d**\|\ **v** ]
[ |-T| ]
[ |SYN_OPT-V| ]
[ |SYN_OPT-b| ]
[ |SYN_OPT-d| ]
[ |SYN_OPT-e| ]
[ |SYN_OPT-h| ]
[ |SYN_OPT-i| ]
[ |SYN_OPT-j| ]
[ |SYN_OPT-qi| ]
[ |SYN_OPT-r| ]
[ |SYN_OPT-s| ]
[ |SYN_OPT-:| ]
[ |SYN_OPT--| ]

|No-spaces|

Description
-----------

**sphtriangulate** reads one or more ASCII [or binary] files (or
standard input) containing lon, lat and performs a spherical Delaunay
triangulation, i.e., it determines how the points should be connected to give
the most equilateral triangulation possible on the sphere. Optionally,
you may choose **-Qv** which will do further processing to obtain the
Voronoi polygons. Normally, either set of polygons will be written as
closed fillable segment output; use |-T| to write unique arcs instead. As an
option, compute the area of each triangle or polygon. The algorithm used
is STRIPACK.

Required Arguments
------------------

.. |Add_intables| unicode:: 0x20 .. just an invisible code
.. include:: explain_intables.rst_

Optional Arguments
------------------

.. _-A:

**-A**
    Compute the area of the spherical triangles (**-Qd**) or polygons
    (**-Qv**) and write the areas (in chosen units; see |-L|) in the
    output segment headers [no areas calculated].

.. _-C:

**-C**
    For large data set you can save some memory (at the expense of more
    processing) by only storing one form of location coordinates
    (geographic or Cartesian 3-D vectors) at any given time, translating
    from one form to the other when necessary [Default keeps both arrays in memory].

.. _-D:

**-D**
    Used to skip the last (repeated) input vertex at the end
    of a closed segment if it equals the first point in the segment.
    [Default uses all points].

.. _-L:

**-L**\ *unit*
    Specify the unit used for distance and area calculations. Choose
    among **e** (m), **f** (foot), **k** (km), **M** (mile), **n**
    (nautical mile), **u** (survey foot), or **d** (spherical degree). A
    spherical approximation is used unless **-je** is set,
    in which case we convert latitudes to authalic
    latitudes before calculating areas. When degree is selected the
    areas are given in steradians.

.. _-N:

**-N**\ *file*
    Write the information pertaining to each polygon. For Delaunay: the
    three node number and the triangle area (if |-A| was set); for
    Voronoi the unique node lon, lat and polygon area (if |-A| was
    set)) to a separate file. This information is also encoded in the
    segment headers of ASCII output files. Required if binary output is needed.

.. _-Q:

**-Q**\ **d**\|\ **v**
    Append **d** for Delaunay triangles or **v** for Voronoi polygons [Delaunay].
    If **-bo** is used then |-N| may be used to specify a separate file where the
    polygon information normally is written.

.. _-T:

**-T**
    Write the unique arcs of the construction [Default writes fillable
    triangles or polygons]. When used with |-A| we store arc length in
    the segment header in chosen unit (see |-L|).

.. |Add_-V| replace:: |Add_-V_links|
.. include:: explain_-V.rst_
    :start-after: **Syntax**
    :end-before: **Description**

.. |Add_-bi| replace:: [Default is 2 input columns].
.. include:: explain_-bi.rst_

.. |Add_-bo| replace:: [Default is same as input].
.. include:: explain_-bo.rst_

.. |Add_-d| unicode:: 0x20 .. just an invisible code
.. include:: explain_-d.rst_

.. |Add_-e| unicode:: 0x20 .. just an invisible code
.. include:: explain_-e.rst_

.. |Add_-h| unicode:: 0x20 .. just an invisible code
.. include:: explain_-h.rst_

.. include:: explain_distcalc.rst_

.. include:: explain_-qi.rst_

.. include:: explain_-s.rst_

.. include:: explain_colon.rst_

.. |Add_nodereg| unicode:: 0x20 .. just an invisible code
.. include:: explain_nodereg.rst_

.. include:: explain_help.rst_

.. include:: explain_precision.rst_

Examples
--------

.. include:: explain_example.rst_

.. include:: oneliner_info.rst_

To create a spherical triangulation from the remote data set hotspots.txt and
then plot it on a sphere, try::

    gmt sphtriangulate @hotspots.txt -Qd -T | gmt plot -Rg -JG-120/-30/7i -Bafg -W3p -pdf map

To triangulate the points in the file testdata.txt, and make a Voronoi
diagram via :doc:`plot`, use

::

  gmt sphtriangulate testdata.txt -Qv | gmt plot -Rg -JG30/30/6i -L -W1p -Bag -pdf testdata

To compute the optimal Delaunay triangulation network based on the
multiple segment file globalnodes.txt and save the area of each triangle
in the header record, try

::

  gmt sphtriangulate globalnodes.txt -Qd -A > global_tri.txt

Notes
-----

The STRIPACK algorithm and implementation expect that there are no duplicate points
in the input.  It is best that the user ensures that this is the case.  GMT has tools,
such as :doc:`blockmean` and others, to combine close points into single entries.
Also, **sphtriangulate** has a |-D| option to determine and exclude duplicates, but
it is a very brute-force yet exact comparison that is very slow for large data sets.
Detection of duplicates in the STRIPACK library will exit the module.

See Also
--------

:doc:`gmt`,
:doc:`triangulate`,
:doc:`sphdistance`

References
----------

Renka, R, J., 1997, Algorithm 772: STRIPACK: Delaunay Triangulation and
Voronoi Diagram on the Surface of a Sphere, *AMC Trans. Math. Software*,
**23**\ (3), 416-434.
