.. _portal-103-5:

##########################################
103.5. Convert flux to magnitude with ADQL
##########################################

For the Portal Aspect of the Rubin Science Platform at data.lsst.cloud.

**Data Release:** Data Preview 2

**Last verified to run:** 2026-08-14

**Learning objective:** Convert fluxes to magnitudes using a special ADQL function.

**LSST data products:** ``Object`` table

**Credit:** Originally developed by the Rubin Community Science team.
Please consider acknowledging them if this tutorial is used for the preparation of journal articles, software releases, or other tutorials.
DOI: `10.11578/rubin/dc.20250909.20 <https://doi.org/10.11578/rubin/dc.20250909.20>`_

**Get Support:** Everyone is encouraged to ask questions or raise issues in the `Support Category <https://community.lsst.org/c/support/6>`_ of the Rubin Community Forum.
Rubin staff will respond to all questions posted there.

----

Magnitude conversions
=====================

While the ``Object`` table has columns for PSF and cModel magnitudes, all other photometry measures are provided only in fluxes (in nJy).
These fluxes can be retrieved as AB magnitudes using the ``scisql_nanojanskyToAbMag()`` function, which applies the equation :math:`m = -2.5 log(f) + 31.4`,
where :math:`m` is magnitude and :math:`f` is flux.

**Warning!** Fluxes measured in difference images can be negative.
Negative fluxes should not be converted to magnitudes using this special ADQL function.

**Note:** to convert fluxes to magnitudes *after* a query is executed, add a new derived column to calculate magnitudes from fluxes (see the tutorial on how to use the results interface table panel for how to add a column).



----

**1. Go to the DP1 & DP2 Catalogs ADQL interface.**
Click on Edit ADQL in the upper right-hand corner.

**2. Set up a query that returns magnitudes.**
The following query returns the *r*-band cModel and Sersic magnitudes, and their errors, for objects brighter than 25th magnitude in *r*-band.
The query uses the ``AS`` functionality to name the newly-calculated magnitude columns.
The query includes a spatial constraint: a circle with radius 0.2 degrees near the center of the ECDFS field.

.. code-block:: SQL

  SELECT r_cModelMag,
         scisql_nanojanskyToAbMag(r_sersicFlux) AS r_sersicMag,
         r_cModelMagErr,
         scisql_nanojanskyToAbMagSigma(r_sersicFlux, r_sersicFluxErr) AS r_sersicMagErr
  FROM dp2.Object
  WHERE CONTAINS(POINT('ICRS', coord_ra, coord_dec),
        CIRCLE('ICRS', 53.13, -28.1, 0.2))=1
  AND r_cModelMag < 25
  AND scisql_nanojanskyToAbMag(r_sersicFlux) < 25


**3. View the results.**
Figure 1 shows the results interface, with the table containing only magnitudes and the default plot using the first two columns.

.. figure:: images/portal-103-5-1.png
    :name: portal-103-5-1
    :alt: The table panel in the results tab shows the created magnitude columns.

    Figure 1: The table panel in the results interface shows no flux columns, but magnitude and magnitude error columns.

