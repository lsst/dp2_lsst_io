.. _isolated-star-stellar-motions:

#############################
Isolated star stellar motions
#############################

Position, proper motion, and parallax for isolated stars, fit using associated source measurements, with positions for associated reference objects if available.

Schema: `IsolatedStarStellarMotions table <https://sdm-schemas.lsst.io/lsstcam.html#IsolatedStarStellarMotions>`_

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
* Number of Butler datasets: |isolated_star_stellar_motions_count|

Description
===========

TBD

Processing
----------

TBD

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the isolated stars stellar motions table.
