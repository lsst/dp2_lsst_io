.. _visit_summary:

#############
Visit summary
#############

A summary of visit metadata.

Access
======

The visit summary table is only accessible via the Butler service.

TAP
---

This dataset is not available via TAP.

Butler
------

* |visit_summary_doi|
* Dataset type: ('visit_summary', {band, **instrument**, day_obs, physical_filter, **visit**}, ExposureCatalog)
* Format: FITS
* Number of Butler datasets: |visit_summary_butler_count|

Description
===========

The ``visit_summary`` dataset is a catalog with one row per detector processed for the visit.
Each row contains metadata, statistics, the PSF model, the photometric calibration, and the full WCS.

Note that the :doc:`/products/catalogs/visit` and :doc:`/products/catalogs/visit_detector` tables also contain aggregated per-visit and per-detector metadata and statistics, but not PSF models, photometric calibration objects, or WCS.

Processing
----------

The visit summary is computed by the ``updateVisitSummary`` task and aggregates all the input metadata as well as the final PSF, global photometric calibration, and multi-visit WCS that was used in coaddition.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the visit summary table.
