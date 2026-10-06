.. _portal-305-1:

#################################
305.1. Variable star light curves
#################################

For the Portal Aspect of the Rubin Science Platform at data.lsst.cloud.

**Data Release:** DP2

**Last verified to run:** 2026-10-05

**Learning objective:** Extract and plot a light curve of a known variable star.

**LSST data products:** ``forced_source``

**Credit:** Originally developed by the Rubin Community Science team.
Please consider acknowledging them if this tutorial is used for the preparation of journal articles, software releases, or other tutorials.
DOI: `10.11578/rubin/dc.20250909.20 <https://doi.org/10.11578/rubin/dc.20250909.20>`_

**Get Support:** Everyone is encouraged to ask questions or raise issues in the `Support Category <https://www.rubin.community/c/support/6>`_ of the Rubin Community Forum.
Rubin staff will respond to all questions posted there.

----

**1. Execute an ADQL query for a light curve.**
Log in to the Portal Aspect, select the "DP1 & DP2 Catalogs" tab and click on "Edit ADQL" at upper right.
Enter the following ADQL statement and click "Search" at lower left.
This query will return all *r*-band deep coadd images that overlap coordinates RA, Dec = 53.0, -28.0 degrees.

.. code-block:: SQL

SELECT diaobj.ra, diaobj.dec, diaobj.diaObjectId, fsodo.visit, fsodo.band,
       fsodo.psfDiffFlux, fsodo.psfDiffFluxErr,
       fsodo.psfFlux as psfFlux, fsodo.psfFluxErr,
       fsodo.psfDiffFlux_flag, fsodo.diff_PixelFlags_nodataCenter,
       fsodo.pixelFlags_saturatedCenter, fsodo.invalidPsfFlag,
       fsodo.pixelFlags_bad, fsodo.psfFlux_flag,
       vis.expMidptMJD
FROM dp2.DiaObject as diaobj
JOIN dp2.ForcedSourceOnDiaObject as fsodo ON fsodo.diaObjectId = diaobj.diaObjectId
JOIN dp2.Visit as vis ON vis.visit = fsodo.visit
WHERE CONTAINS(POINT('ICRS', diaobj.ra, diaobj.dec),
               CIRCLE('ICRS', 226.7244283498404, -40.30405425755786, 0.5/3600)) = 1


Plot flux vs. time.

Demonstrate toggling measurements in different bands on/off.

Demonstrate applying flag cuts?

Calculate phase. Use the earliest observation time as t0. Use the known period.

Time since t0: expMidptMJD-60791.35148273523
phase: (tdiff/0.5180638410804721)-floor(tdiff/0.5180638410804721)

Convert to magnitude: mag = -2.5*log10(psfFlux) + 31.4 [remind that this won't work for psfDiffFlux]