.. index:: ! basemap
.. include:: module_core_purpose.rst_

*******
basemap
*******

|basemap_purpose|

Synopsis
--------

.. include:: common_SYN_OPTs.rst_

**gmt basemap** |-J|\ *parameters*
|SYN_OPT-Rz|
[ |-A|\ [*file*] ]
[ |SYN_OPT-B| ]
[ |-F|\ *box* ]
[ |-J|\ **z**\|\ **Z**\ *parameters* ]
[ |-L|\ *scalebar* ]
[ |-T|\ *rose* ]
[ |-T|\ *mag_rose* ]
[ |SYN_OPT-U| ]
[ |SYN_OPT-V| ]
[ |SYN_OPT-X| ]
[ |SYN_OPT-Y| ]
[ |SYN_OPT-f| ]
[ |SYN_OPT-p| ]
[ |SYN_OPT-t| ]
[ |SYN_OPT--| ]

.. module_common_begins

Description
-----------

Creates a basic or fancy basemap with axes, fill, and titles. Several map projections are available, and the user may
specify separate tick-mark intervals for boundary annotation, ticking, and (optionally) gridlines. A simple map scale
(|-L|) or directional rose (|-T|) may also be plotted. At least one of the options |-B|, |-L|, or |-T| must be
specified.

Required Arguments
------------------

.. |Add_-J| replace:: |Add_-J_links|
.. include:: explain_-J.rst_
    :start-after: **Syntax**
    :end-before: **Description**

.. |Add_-R| replace:: |Add_-R_links|
.. include:: explain_-R.rst_
    :start-after: **Syntax**
    :end-before: **Description**

.. |Add_-Rz| unicode:: 0x20 .. just an invisible code
.. include:: explain_-Rz.rst_

Optional Arguments
------------------

.. _-A:

**-A**\ [*file*]
    No plotting is performed.  Instead, we determine the geographical coordinates of the polygon outline for the
    (possibly oblique) rectangular map domain.  The plot domain must be given via |-R| and |-J|, with no other options
    allowed. The sampling interval is controlled via :term:`MAP_LINE_STEP` parameter. The coordinates are written to
    *file* or to standard output if no file is specified.

.. |Add_-B| replace:: |Add_-B_links|
.. include:: explain_-B.rst_
    :start-after: **Syntax**
    :end-before: **Description**

.. _-F:

**-F**\ [**d**\|\ **l**\|\ **t**][**+c**\ *clearances*][**+g**\ *fill*][**+i**\ [[*gap*/]\ *pen*]][**+p**\ [*pen*]]\
[**+r**\ [*radius*]][**+s**\ [[*dx*/*dy*/][*shade*]]]

    Without further options, draws a rectangular border around any map inset (|-D|; classic mode only), map scale (|-L|)
    or map rose (|-T|) using :term:`MAP_FRAME_PEN`. Used in combination with |-D|, |-L| or |-T|.  Append **d** for map
    inset, **l** for map scale, or **t** for map rose to specify which plot embellisment the |-F| parameters should be
    applied to [default uses the same panel parameters for all selected map embellishments]. The following modifiers can
    be appended to |-F|, with additional explanation and examples provided in the
    :ref:`cookbook/features:The background panel` cookbook section:

    .. include:: explain_-F_box.rst_

.. _-L:

.. include:: explain_-L_scale.rst_

.. _-T:

.. include:: explain_-T_rose.rst_

.. |Add_-U| replace:: |Add_-U_links|
.. include:: explain_-U.rst_
    :start-after: **Syntax**
    :end-before: **Description**

.. |Add_-V| replace:: |Add_-V_links|
.. include:: explain_-V.rst_
    :start-after: **Syntax**
    :end-before: **Description**

.. |Add_-XY| replace:: |Add_-XY_links|
.. include:: explain_-XY.rst_
    :start-after: **Syntax**
    :end-before: **Description**

.. |Add_-f| replace:: This applies only to the coordinates specified in the |-R| option.
.. include:: explain_-f.rst_

.. |Add_perspective| unicode:: 0x20 .. just an invisible code
.. include:: explain_perspective.rst_

.. include:: explain_-t.rst_

.. include:: explain_help.rst_

.. module_common_ends

Examples
--------

.. include:: oneliner_info.rst_

The following section illustrates the use of the options by giving some
examples for the available map projections. Note how scales may be given
in several different ways depending on the projection. Also note the use
of upper case letters to specify map width instead of map scale.

Non-geographical Projections
----------------------------

Linear x-y plot
~~~~~~~~~~~~~~~

To make a linear x/y frame with all axes, but with only left and bottom
axes annotated, using xscale = yscale = 1cm per unit, ticking every 1 unit and
annotating every 2, and using xlabel = "Distance" and ylabel = "No of samples", use

::

  gmt begin linear
    gmt basemap -R0/9/0/5 -Jx1c -Bf1a2 -Bx+lDistance -By+l"No of samples" -BWeSn
  gmt end show

As mentioned above, such simple modern mode script can take advantage of the one-liner
format.  We repeat the same example using the one-liner format and then only show this
format for the remaining examples:

::

  gmt basemap -R0/9/0/5 -Jx1c -Bf1a2 -Bx+lDistance -By+l"No of samples" -BWeSn -pdf linear

Log-log plot
~~~~~~~~~~~~

To make a log-log frame with only the left and bottom axes, where the
x-axis is 25 cm and annotated every 1-2-5 and the y-axis is 15 cm and
annotated every power of 10 but has tick-marks every 0.1, run

::

  gmt basemap -R1/10000/1e20/1e25 -JX25cl/15cl -Bx2+lWavelength -Bya1pf3+lPower -BWS -pdf loglog

Power axes
~~~~~~~~~~

To design an axis system to be used for a depth-sqrt(age) plot with
depth positive down, ticked and annotated every 500m, and ages (in millions of years) annotated
at 1 My, 4 My, 9 My etc., use

::

  gmt basemap -R0/100/0/5000 -Jx1cp0.5/-0.001c -Bx1p+l"Crustal age" -By500+lDepth -pdf power

Polar (theta,r) plot
~~~~~~~~~~~~~~~~~~~~

For a base map for use with polar coordinates, where the radius from 0
to 1000 should correspond to 3 inch and with gridlines and ticks intervals
automatically determined, use

::

  gmt basemap -R0/360/0/1000 -JP6i -Bafg -pdf polar

Cylindrical Map Projections
---------------------------

Cassini
~~~~~~~

A 10-cm-wide basemap using the Cassini projection may be obtained by

::

gmt basemap -R20/50/20/35 -JC35/28/10c -Bafg -B+tCassini -pdf cassini

Mercator [conformal]
~~~~~~~~~~~~~~~~~~~~

A Mercator map with scale 0.025 inch/degree along equator, and showing
the length of 5000 km along the equator (centered on 1/1 inch), may be
plotted as

::

  gmt basemap -R90/180/-50/50 -Jm0.025i -Bafg -B+tMercator -Lx1i/1i+c0+w5000k -pdf mercator

Miller
~~~~~~

A global Miller cylindrical map with scale 1:200,000,000 may be plotted as

::

  gmt basemap -Rg -Jj180/1:200000000 -Bafg -B+tMiller -pdf miller

Oblique Mercator [conformal]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To create a page-size global oblique Mercator basemap for a pole at
(90,30) with gridlines every 30 degrees, run

::

  gmt basemap -R0/360/-70/70 -Joc0/0/90/30/0.064cd -B30g30 -B+t"Oblique Mercator" -pdf oblmerc

Transverse Mercator [conformal]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A regular Transverse Mercator basemap for some region may look like

::

  gmt basemap -R69:30/71:45/-17/-15:15 -Jt70/1:1000000 -Bafg -B+t"Survey area" -pdf transmerc

Equidistant Cylindrical Projection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This projection only needs the central meridian and scale. A 25 cm wide
global basemap centered on the 130E meridian is made by

::

  gmt basemap -R-50/310/-90/90 -JQ130/25c -Bafg -B+t"Equidistant Cylindrical" -pdf cyl_eqdist

Universal Transverse Mercator [conformal]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To use this projection you must know the UTM zone number, which defines
the central meridian. A UTM basemap for Indo-China can be plotted as

::

  gmt basemap -R95/5/108/20+r -Ju46/1:10000000 -Bafg -B+tUTM -pdf utm

Cylindrical Equal-Area
~~~~~~~~~~~~~~~~~~~~~~

First select which of the cylindrical equal-area projections you want by
deciding on the standard parallel. Here we will use 45 degrees which
gives the Gall projection. A 9 inch wide global basemap centered
on the Pacific is made by

::

  gmt basemap -Rg -JY180/45/9i -Bafg -B+tGall -pdf gall

Conic Map Projections
---------------------

Albers [equal-area]
~~~~~~~~~~~~~~~~~~~

A basemap for middle Europe may be created by

::

  gmt basemap -R0/90/25/55 -Jb45/20/32/45/0.25c -Bafg -B+t"Albers Equal-area" -pdf albers

Lambert [conformal]
~~~~~~~~~~~~~~~~~~~

Another basemap for middle Europe may be created by

::

  gmt basemap -R0/90/25/55 -Jl45/20/32/45/0.1i -Bafg -B+t"Lambert Conformal Conic" -pdf lambertc

Conic Equidistant
~~~~~~~~~~~~~~~~~

Yet another basemap of width 6 inch for middle Europe may be created by

::

  gmt basemap -R0/90/25/55 -JD45/20/32/45/6i -Bafg -B+t"Equidistant conic" -pdf econic

Polyconic
~~~~~~~~~

A basemap for north America may be created by

::

  gmt basemap -R-180/-20/0/90 -JPoly/4i -Bafg -B+tPolyconic -pdf polyconic

Azimuthal Map Projections
-------------------------

Lambert [equal-area]
~~~~~~~~~~~~~~~~~~~~

A 15-cm-wide global view of the world from the vantage point -80/-30
will give the following basemap:

::

  gmt basemap -Rg -JA-80/-30/15c -Bafg -B+t"Lambert Azimuthal" -pdf lamberta

Follow the instructions for stereographic projection if you want to
impose rectangular boundaries on the azimuthal equal-area map but
substitute |-J|\ **a** for |-J|\ **s**.

Azimuthal Equidistant
~~~~~~~~~~~~~~~~~~~~~

A 15-cm-wide global map in which distances from the center (here 125/10)
to any point is true can be obtained by:

::

  gmt basemap -Rg -JE125/10/15c -Bafg -B+tEquidistant -pdf equi

Gnomonic
~~~~~~~~

A view of the world from the vantage point -100/40 out to a horizon of
60 degrees from the center can be made using the Gnomonic projection:

::

  gmt basemap -Rg -JF-100/40/60/6i -Bafg -B+tGnomonic -pdf gnomonic

Orthographic
~~~~~~~~~~~~

A global perspective (from infinite distance) view of the world from the
vantage point 125/10 will give the following 6-inch-wide basemap:

::

  gmt basemap -Rg -JG125/10/6i -Bafg -B+tOrthographic -pdf ortho

General Perspective
~~~~~~~~~~~~~~~~~~~

The |-J|\ **G** option can be used in a more generalized form, specifying
altitude above the surface, width and height of the view point, and
twist and tilt. A view from 160 km above -74/41.5 with a tilt of 55 and
azimuth of 210 degrees, and limiting the viewpoint to 30 degrees width
and height will product a 6-inch-wide basemap:

::

  gmt basemap -Rg -JG-74/41.5/6i+z160+a210+t55+v30 -Bafg -B+t"General Perspective" -pdf genper

Stereographic [conformal]
~~~~~~~~~~~~~~~~~~~~~~~~~

To make a polar stereographic projection basemap with radius = 12 cm to
-60 degree latitude, with plot title "Salinity measurements", using 5
degrees annotation/tick interval and 1 degree gridlines, run

::

  gmt basemap -R-45/45/-90/-60 -Js0/-90/12c/-60 -B5g1 -B+t"Salinity measurements" -pdf stereo1

To make a 12-cm-wide stereographic basemap for Australia from an
arbitrary view point (not the poles), and use a rectangular boundary, we
must give the pole for the new projection and use the |-R| option to
indicate the lower left and upper right corners (in lon/lat) that will
define our rectangle. We choose a pole at 130/-30 and use 100/-45 and
160/-5 as our corners. The command becomes

::

  gmt basemap -R100/-45/160/-5+r -JS130/-30/12c -Bafg -B+t"General Stereographic View" -pdf stereo2

Miscellaneous Map Projections
-----------------------------

Hammer [equal-area]
~~~~~~~~~~~~~~~~~~~

The Hammer projection is mostly used for global maps and thus the
spherical form is used. To get a world map centered on Greenwich at a
scale of 1:200000000, use

::

  gmt basemap -Rd -Jh0/1:200000000 -Bafg -B+tHammer -pdf hammer

Sinusoidal [equal-area]
~~~~~~~~~~~~~~~~~~~~~~~

To make a sinusoidal world map centered on Greenwich, with a scale along
the equator of 0.02 inch/degree, use

::

  gmt basemap -Rd -Ji0/0.02i -Bafg -B+tSinusoidal -pdf sinus1

To make an interrupted sinusoidal world map with breaks at 160W, 20W,
and 60E, with a scale along the equator of 0.02 inch/degree, run the
following sequence of commands:

::

  gmt begin
  gmt basemap -R-160/-20/-90/90 -Ji-90/0.02i -Bx30g30 -By15g15 -BWesn
  gmt basemap -Bx30g30 -By15g15 -Bwesn -X2.8i
  gmt basemap -Bx30g30 -By15g15 -BwEsn -X1.6i
  gmt end show

Eckert IV [equal-area]
~~~~~~~~~~~~~~~~~~~~~~

Pseudo-cylindrical projection typically used for global maps only. Set
the central longitude and scale, e.g.,

::

  gmt basemap -Rg -Jkf180/0.064c -Bafg -B+t"Eckert IV" -pdf eckert4

Eckert VI [equal-area]
~~~~~~~~~~~~~~~~~~~~~~

Another pseudo-cylindrical projection typically used for global maps
only. Set the central longitude and scale, e.g.,

::

  gmt basemap -Rg -Jks180/0.064c -Bafg -B+t"Eckert VI" -pdf eckert6

Robinson
~~~~~~~~

Projection designed to make global maps "look right". Set the central
longitude and width, e.g.,

::

  gmt basemap -Rd -JN0/8i -Bafg -B+tRobinson -pdf robinson

Winkel Tripel
~~~~~~~~~~~~~

Yet another projection typically used for global maps only. You can set
the central longitude, e.g.,

::

  gmt basemap -R90/450/-90/90 -JR270/25c -Bafg -B+t"Winkel Tripel" -pdf winkel

Mollweide [equal-area]
~~~~~~~~~~~~~~~~~~~~~~

The Mollweide projection is also mostly used for global maps and thus
the spherical form is used. To get a 25-cm-wide world map centered on
the Dateline:

::

  gmt basemap -Rg -JW180/25c -Bafg -B+tMollweide -pdf mollweide

Van der Grinten
~~~~~~~~~~~~~~~

The Van der Grinten projection is also mostly used for global maps and
thus the spherical form is used. To get a 18-cm-wide world map centered on the Dateline:

::

  gmt basemap -Rg -JV180/18c -Bafg -B+t"Van der Grinten" -pdf grinten

Arbitrary rotation
~~~~~~~~~~~~~~~~~~

If you need to plot a map but have it rotated about a vertical axis then
use the **-p** option.  For instance, to rotate the basemap below 90
degrees about an axis centered on the map, try

::

  gmt basemap -R10/40/10/40 -JM10c -Bafg -B+t"I am rotated" -p90+w25/25 -Xc -pdf rotated

.. module_note_begins

Custom Labels or Intervals
--------------------------

The |-B| option sets up a regular annotation interval and the
annotations derive from the corresponding *x*, *y*, or *z* coordinates.
However, some applications requires special control on which annotations
to plot and even replace the annotation with other labels. This is
achieved by using **c**\ *intfile* in the |-B| option, where *intfile*
contains all the information about annotations, ticks, and even
gridlines. Each record is of the form *coord* *type* [*label*], where
*coord* is the coordinate for this annotation (or tick or gridline),
*type* is one or more letters from **a** (annotation), **i** interval
annotation, **f** tickmark, and **g** gridline. Note that **a** and
**i** are mutually exclusive and cannot both appear in the same
*intfile*. Both **a** and **i** requires you to supply a *label* which
is used as the plot annotation. If not given then a regular formatted
annotation based on the coordinate will occur.

Restrictions
------------

For some projections, a spherical earth is implicitly assumed. A warning
will notify the user if |-V| is set.

Bugs
----

The |-B| option is somewhat complicated to explain and comprehend.
However, it is fairly simple for most applications (see examples).

.. module_note_ends

See Also
--------

:doc:`gmt`, :doc:`gmt.conf`, :doc:`gmtcolors`
