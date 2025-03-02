.. index:: ! grdgravmag3d
.. include:: ../module_supplements_purpose.rst_

************
grdgravmag3d
************

|grdgravmag3d_purpose|

Synopsis
--------

.. include:: ../../common_SYN_OPTs.rst_

**gmt grdgravmag3d** *grdfile_top* [*grdfile_bot*] [ |-C|\ *density* ]
[ |-E|\ *thickness* ]
[ |-F|\ *xy_file* ]
[ |-G|\ *outgrid* ]
[ |-H|\ *args* ]
[ |SYN_OPT-I| ]
[ |-L|\ *z_obs* ]
[ |-Q|\ [**n**\ *n_pad*]\|\ [*pad_dist*]\|\ [*region*] ]
[ |SYN_OPT-R| ]
[ |-S|\ *radius* ]
[ |SYN_OPT-V| ]
[ |-Z|\ *level*\ [**b**\|\ **t**] ]
[ |SYN_OPT-f| ]
[ **-x**\ *+a|n|-n* ]
[ |SYN_OPT--| ]

|No-spaces|

Description
-----------

**grdgravmag3d** will compute the gravity anomaly of a body described by
one or (optionally) two grids The output can either be along a given set
of xy locations or on a grid. This method is not particularly fast but
allows computing the anomaly of arbitrarily complex shapes.

Required Arguments
------------------

*grdfile_top* [*grdfile_bot*]
   Grid file whose gravity effect is going to be computed. If two grids are
   provided then the gravity/magnetic effect of the volume between them is computed.

.. _-C:

**-C**\ *density*
    Sets body density in SI. Append either a constant or the name of a grid file with variable densities.
    This option is mutually exclusive with |-H|

.. _-F:

**-F**\ *xy_file*
    Provide locations where the anomaly will be computed. Note this
    option is mutually exclusive with |-G|.

.. _-G:

.. |Add_outgrid| replace:: Give the name of the output grid file.
.. include:: /explain_grd_inout.rst_
    :start-after: outgrid-syntax-begins
    :end-before: outgrid-syntax-ends

Optional Arguments
------------------

.. _-E:

**-E**\ *thickness*
    To provide the layer thickness in m [Default = 500 m].

.. _-H:

**-H**\ *f_dec/f_dip/m_int/m_dec/m_dip* **-H**\ *magfile*  **-Hx**\|\ **y**\|\ **z**\|\ **h**\|\ **t** **-H+i**\|\ **g**\|\ **r**\|\ **f**\|\ **n**
    Sets parameters for computation of magnetic anomaly (Can be used multiple times).

      *f_dec/f_dip* -> geomagnetic declination/inclination

      *m_int/m_dec/m_dip* -> body magnetic intensity/declination/inclination

    OR for a grid mode

      *magfile*, where *magfile* is the name of the magnetic intensity file.

    To compute a component, specify any of:

      **x**\|\ **X**\|\ **e**\|\ **E**  to compute the E-W component.

      **y**\|\ **Y**\|\ **n**\|\ **N**  to compute the N-S component.

      **z**\|\ **Z**      to compute the Vertical component.

      **h**\|\ **H**      to compute the Horizontal component.

      **t**\|\ **T**\|\ **f**\|\ **F**  to compute the total field.

    If we want to compute the magnetic anomalies over a large region where the ambient magnetic field
    can no longer be assumed to be constant we can set variable inclinations and declinations via IGRF.
    Set any of **-H+i**\|\ **g**\|\ **r**\|\ **f**\|\ **n** to do that

.. _-I:

.. include:: ../../explain_-I.rst_

.. _-L:

**-L**\ *z_obs*
    Sets level of observation [Default = 0]. That is the height (z) at
    which anomalies are computed.

.. _-Q:

**-Q**\ [**n**\ *n_pad*]\|\ [*pad_dist*]\|\ [*region*]
    Extend the domain of computation with respect to output |-R| region.
      **-Qn**\ *n_pad* artificially extends the width of the outer rim of
      cells to have a fake width of *n_pad* * dx[/dy].

      **-Q**\ *pad_dist* extend the region by west-pad, east+pad, etc.

      **-Q**\ *region* Same syntax as |-R|.

.. |Add_-R| replace:: **Note**: This overrides the source grid region (Default: use same region as input) |Add_-R_links|
.. include:: ../../explain_-R.rst_
    :start-after: **Syntax**
    :end-before: **Description**

.. _-S:

**-S**\ *radius*
    Set search radius in km (valid only in the two grids mode OR when |-E|) [Default = 30 km].
    This option serves to speed up the computation by not computing the effect of prisms that
    are further away than *radius* from the current node.

.. |Add_-V| replace:: |Add_-V_links|
.. include:: /explain_-V.rst_
    :start-after: **Syntax**
    :end-before: **Description**

.. _-Z:

**-Z**\ *level*\ [**b**\|\ **t**]
    level of reference plane [Default = 0]. Use this option when the
    triangles describe a non-closed surface and the volume is defined
    from each triangle and this reference level. An example will be the
    water depth to compute a Bouguer anomaly. Use **-Zb** or **Zt** to close
    the body at its bottom (for example, to compute the effect of a dome) or
    at its top (to compute the effect of a *spoon*).

|SYN_OPT-f|
   Geographic grids (dimensions of longitude, latitude) will be converted to
   meters via a "Flat Earth" approximation using the current ellipsoid parameters.

**-x**\ *+a|n|-n*
   Choose the number of processors used in multi-threading (Only available with multi-threading builds).

     *+a* Use all available processors.

     *n*  Use n processors (not more than max available off course).

     *-n* Use (all - n) processors.

.. include:: ../../explain_help.rst_

Grid Distance Units
-------------------

If the grid does not have meter as the horizontal unit, append **+u**\ *unit* to the input file name to convert from the
specified unit to meter. If your grid is geographic, convert distances to meters by supplying |SYN_OPT-f| instead.

Examples
--------

Suppose you want to compute the gravity effect of the phantom "Sandy
Island" together with its not phantom seamount

::

  gmt grdgravmag3d sandy_bat.grd -C1700 -Z-4300 -fg -I1m -Gsandy_okb.grd -V

To compute the vertical component due to a magnetization stored in *mag.grd* over a zone defined by
the surface *bat.grd*, using variable declination and inclination provided the the IGRF and using 4
processors, do:

::

  gmt grdgravmag3d bat.grd -E10000 -Gcomp_Z.grd -Hz -H+n -H+mmag.grd -x4 -V -S50

See Also
--------

:doc:`gmt </gmt>`, :doc:`gmtgravmag3d`,
:doc:`talwani2d </supplements/potential/talwani2d>`,
:doc:`talwani3d </supplements/potential/talwani3d>`

Reference
---------

Okabe, M., 1979, Analytical expressions for gravity anomalies due to
polyhedral bodies and translation into magnetic anomalies, *Geophysics*,
44, 730-741.
