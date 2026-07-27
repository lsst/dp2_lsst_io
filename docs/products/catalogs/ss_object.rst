.. _catalogs-ss-object:

#########
SS object
#########

Rubin-derived object-level quantities for recovered known small bodies.

Schema: `SSObject table <https://sdm-schemas.lsst.io/dp2.html#SSObject>`_

Access
======

The SS object table is accessible via the TAP and Butler services.

**Recommended access service:** TAP

TAP
---

* |SSObject_doi|
* Table name: ``SSObject``
* Columns: |SSObject_columns|
* Rows: |SSObject_rows|

Butler
------

* |ss_object_doi|
* Dataset type: ('ss_object', {}, ArrowAstropy)
* Format: Parquet
* Number of Butler datasets: |ss_object_butler_count|

Description
===========

Each row summarizes a previously known small body with one or more DP2 associations.
This is not a catalog of objects discovered by DP2, and the table does not contain orbits fitted from DP2 observations.

``SSObject`` includes the integer ``ssObjectId``, the unpacked primary provisional ``designation``, total and per-band observation counts, the first observation epoch and observing arc, per-band phase-angle ranges, extendedness summaries, and selected quantities derived from the input MPC orbit.
Join to :doc:`SSSource <ss_source>` on ``ssObjectId`` for single-epoch measurements and observing geometry.

Per-band absolute magnitudes were fitted to the associated PSF photometry with an H-G12 phase function.
For DP2, G12 was fixed at 0.5 and only H was fitted; valid G12 values therefore all equal 0.5, while G12 uncertainties and the H-G12 covariance are undefined.
A 0.05 magnitude uncertainty floor was included, and a robust initial fit was used to reject measurements beyond 10 sigma before the final least-squares fit.
Users should compare the per-band ``nObsUsed`` and ``nObs`` values, inspect the phase-angle coverage and fit statistics, and treat sparsely sampled fits and cross-band colors with care.

The table also contains the Tisserand parameter with respect to Jupiter and Earth minimum-orbit-intersection-distance quantities computed from the input MPC elements.
These values should not by themselves be used for hazard classification or close-approach prediction.

Processing
----------

The SS object table is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS object table.
