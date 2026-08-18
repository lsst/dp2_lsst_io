.. _isolated-star-stellar-motions:

#############################
Isolated star stellar motions
#############################

Position, proper motion, and parallax for isolated stars, fit using associated source measurements, with positions for associated reference objects if available.

Schema: `IsolatedStarStellarMotions table <https://sdm-schemas.lsst.io/dp2.html#IsolatedStarStellarMotions>`_

Access
======

This table is accessible via the TAP and Butler services.

**Recommended access service:** TAP

TAP
---

* |IsolatedStarStellarMotions_doi|
* Table name: ``IsolatedStarStellarMotions``
* Columns: |IsolatedStarStellarMotions_columns|
* Rows: |IsolatedStarStellarMotions_rows|

Butler
------

* |isolated_star_stellar_motions_doi|
* Dataset type: ('isolated_star_stellar_motions', {instrument, **skymap**, **tract**}, ArrowAstropy)
* Format: Parquet
* Number of Butler datasets: |isolated_star_stellar_motions_butler_count|

Description
===========

An "isolated" star is defined as an unblended, point-like detection with a signal-to-noise ratio between 8 and 1000 that, after being spatially matched within a matching radius of 1 arcsec across all visits and bands in a given tract, has no other candidate within 2 arcsec of its average position.

The Isolated Star Stellar Motions catalog provides a best-fit position (``ra``, ``dec``), proper motion in mas/yr (``raPM``, ``decPM``), and parallax in mas at a reference epoch for all identified isolated stars. The ``raPM`` parameter already includes the cos(decl.) factor. The reference epoch in Modified Julian Date (MJD) is included in the catalog. Each entry includes a unique identifier and, where applicable, the ID, position, proper motion, and parallax of the matched reference Gaia DR3 source. The catalog also includes 1-sigma uncertainties for all astrometric parameters, along with the off-diagonal elements of the covariance matrix.

Processing
----------

The position, proper motion, and parallax are computed jointly across the matched visits using ``FitStellarMotionTask``. For details, see :doc:`/processing/detection/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the isolated stars stellar motions table.
