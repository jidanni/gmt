.. index:: ! grdfilter
.. include:: module_core_purpose.rst_

*********
grdfilter
*********

|grdfilter_purpose|

Synopsis
--------

.. include:: common_SYN_OPTs.rst_

**gmt grdfilter** *ingrid* |-D|\ *distance_flag*
|-F|\ **x**\ *width*\ [/*width2*][*modifiers*]
|-G|\ *outgrid*
[ |SYN_OPT-I| ]
[ |-N|\ **i**\|\ **p**\|\ **r** ]
[ |SYN_OPT-R| ]
[ |-T| ]
[ |SYN_OPT-V| ]
[ |SYN_OPT-f| ]
[ |SYN_OPT-r| ]
[ |SYN_OPT--| ]

|No-spaces|

Description
-----------

**grdfilter** will filter a grid in the time (or space) domain using one of
the selected convolution or non-convolution isotropic or rectangular
filters and compute distances using Cartesian or Spherical geometries.
The output grid can optionally be generated as a sub-region of the
input (via |-R|) and/or with new increment (via |-I|) or
registration (via |-T|). In this way, one may have "extra space" in
the input data so that the edges will not be used and the output can be
within one half-width of the input edges. If the filter is low-pass,
then the output may be less frequently sampled than the input.  **Note**:
For filtering in the frequency (or wavenumber) domain instead, see :doc:`grdfft`.

Required Arguments
------------------

.. |Add_ingrid| replace:: The grid file of points to be filtered.
.. include:: explain_grd_inout.rst_
    :start-after: ingrid-syntax-begins
    :end-before: ingrid-syntax-ends

.. _-D:

**-D**\ *flag*
    Distance *flag* tells how grid (x,y) relates to filter *width* as
    follows:

    - *flag* = p: grid (px,py) with *width* an odd number of pixels;
      Cartesian distances.
    - *flag* = 0: grid (x,y) same units as *width*, Cartesian distances.
    - *flag* = 1: grid (x,y) in degrees, *width* in km, Cartesian
      distances.
    - *flag* = 2: grid (x,y) in degrees, *width* in km, dx scaled by
      cos(middle y), Cartesian distances.

    The above options are fastest because they allow weight matrix to be
    computed only once. The next three options are slower because they
    recompute weights for each latitude.

    - *flag* = 3: grid (x,y) in degrees, *width* in km, dx scaled by
      cosine(y), Cartesian distance calculation.
    - *flag* = 4: grid (x,y) in degrees, *width* in km, Spherical distance
      calculation.
    - *flag* = 5: grid (x,y) in Mercator **-Jm**\ 1 img units, *width* in
      km, Spherical distance calculation.

.. _-F:

**-Fx**\ *width*\ [/*width2*][*modifiers*]
    Sets the filter type. Choose among convolution and non-convolution
    filters. Use any filter code **x** (listed below) followed by the full
    diameter *width*. This gives an isotropic filter; append /*width2*
    for a rectangular filter (requires **-Dp** or **-D0**).  By default we
    perform low-pass filtering; append **+h** to select high-pass filtering.
    For isotropic filters, *width* may be a grid for variable filter width,
    in which case the grid must have the same registration and dimensions as
    the output filtered grid.
    Some filters allow for optional arguments and modifiers.

    Convolution filters (and their codes) are:

    - (**b**) Boxcar: All weights are equal.
    - (**c**) Cosine Arch: Weights follow a cosine arch curve.
    - (**g**) Gaussian: Weights are given by the Gaussian function, where
      *width* is 6 times the conventional Gaussian sigma.
    - (**f**) Custom: Weights are given by the precomputed values in the
      filter weight grid file *weight*, which must have odd dimensions;
      also requires **-D0** and output spacing must match input spacing or
      be integer multiples.
    - (**o**) Operator: Weights are given by the precomputed values in the
      filter weight grid file *weight*, which must have odd dimensions;
      also requires **-D0** and output spacing must match input spacing or
      be integer multiples. Weights are assumed to sum to zero so no
      accumulation of weight sums and normalization will be done.

    Non-convolution filters (and their codes) are:

    - (**m**) Median: Returns median value. To select another quantile
      append **+q**\ *quantile* in the 0-1 range [Default is 0.5, i.e., median].
    - (**p**) Maximum likelihood probability (a mode estimator): Return
      modal value. If more than one mode is found we return their average
      value. Append **+l** or **+u** if you rather want
      to return the lowermost or uppermost of the modal values.
    - (**h**) Histogram mode (another mode estimator): Return the modal
      value as the center of the dominant peak in a histogram. Append /*binwidth* to
      specify the binning interval.  Use modifier **+c** to center the
      bins on multiples of *binwidth* [Default has bin edges that are
      multiples of *binwidth*].  If more than one mode is found we return their average
      value. Append **+l** or **+u** if you rather want
      to return the lowermost or uppermost of the modal values.
    - (**l**) Lower: Return the minimum of all values.
    - (**L**) Lower: Return minimum of all positive values only.
    - (**u**) Upper: Return maximum of all values.
    - (**U**) Upper: Return maximum of all negative values only.

    In the case of **L**\|\ **U** it is possible that no data passes
    the initial sign test; in that case the filter will return NaN.

.. _-G:

.. |Add_outgrid| replace:: Give the name of the output filtered grid file.
.. include:: /explain_grd_inout.rst_
    :start-after: outgrid-syntax-begins
    :end-before: outgrid-syntax-ends

Optional Arguments
------------------

.. _-I:

.. include:: explain_-I.rst_

.. _-N:

**-N**\ **i**\|\ **p**\|\ **r**
    Determine how NaN-values in the input grid affects the filtered
    output: Append **i** to ignore all NaNs in the calculation of
    filtered value [Default], **r** is same as **i** except if the input
    node was NaN then the output node will be set to NaN (only applies
    if both grids are co-registered), and **p** which will force the
    filtered value to be NaN if any grid-nodes with NaN-values are found
    inside the filter circle.

.. _-R:

**-R**
    *west*, *east*, *south*, and *north* defines the Region of the
    output points. [Default: Same as input.]

.. _-T:

**-T**
    Toggle the node registration for the output grid so as to become the
    opposite of the input grid [Default gives the same registration as
    the input grid]. Alternatively, use **-r**\ [**g**\|\ **p**] to
    set the registration explicitly.

.. |Add_-V| replace:: |Add_-V_links|
.. include:: explain_-V.rst_
    :start-after: **Syntax**
    :end-before: **Description**

.. |Add_-f| unicode:: 0x20 .. just an invisible code
.. include:: explain_-f.rst_

.. |Add_nodereg| unicode:: 0x20 .. just an invisible code
.. include:: explain_nodereg.rst_

.. include:: explain_help.rst_

.. include:: explain_grd_coord.rst_

Examples
--------

.. include:: explain_example.rst_

The @earth_relief_05m is a remote file of 5 minute bathymetry
and you want to find the medians of
values within a 300km radius (600km full width) of the output points,
which you choose to be from 150E to 250E and 10N to 40N, and you want
the output values every 0.5 degree. Using spherical distance
calculations, you need:

::

  gmt grdfilter @earth_relief_05m -Gfiltered_pacific.nc -Fm600 -D4 -R150/250/10/40 -I0.5 -V

If we instead wanted a high-pass result then one can perform the
corresponding low-pass filter using a coarse grid interval as **grdfilter**
will resample the result to the same resolution as the input grid so we
can compute the residuals, e.g.,

::

  gmt grdfilter @earth_relief_05m -Gresidual_pacific.nc -Fm600+h -D4 -R150/250/10/40 -I0.5 -V

Here, the residual_pacific.nc grid will have the same 5 minute
resolution as the original.

To filter the dataset in ripples.nc using a custom anisotropic Gaussian
filter exp (-0.5\*r^2) whose distances r from the center is given by
(2x^2 + y^2 -2xy)/6, with major axis at an angle of 63 degrees with the
horizontal, try

::

  gmt grdmath -R-10/10/-10/10 -I1 X 2 POW 2 MUL Y 2 POW ADD X Y MUL 2 MUL \
              SUB 6 DIV NEG 2 DIV EXP DUP SUM DIV = gfilter.nc
  gmt grdfilter ripples.nc -Ffgfilter.nc -D0 -Gsmooth.nc -V

Limitations
-----------

#. To use the **-D**\ 5 option the input Mercator grid must be created by
   img2mercgrd using the |-C| option so the origin of the y-values is the
   Equator (i.e., x = y = 0 correspond to lon = lat = 0).
#. If the new *x\_inc*, *y\_inc* set with |-I| are **not** integer multiples
   of the increments in the input data, filtering will be considerably slower.
   [Default increments: Same as input.]


See Also
--------

:doc:`gmt`, :doc:`grdfft` :doc:`img2grd <supplements/img/img2grd>`
