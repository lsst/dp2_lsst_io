.. _deep-coadd-input-summary:

########################
Deep coadd input summary
########################

A table that documents which visits and detectors contributed to which tract and patch, for deep coadd images.

Access
======

The deep coadd input summary table is only accessible via the Butler service.

TAP
---

This dataset is not available via TAP.

Butler
------

* |deep_coadd_input_summary_doi|
* Dataset type: ('deep_coadd_input_summary', {**skymap**}, ArrowAstropy))
* Format: Parquet
* Number of Butler datasets: |deep_coadd_input_summary_butler_count|

Description
===========

The ``deep_coadd_input_summary`` table provides a patch-level summary of the visit detector images that contributed to build each deep coadd. Note that the ``deep_coadd_input_summary`` table does not include information about which visit detector images contributed to each deep coadd cell. The ``deep_coadd_input_summary`` table contains 7 columns and 21,235,500 rows.

A schema is not yet published for the ``deep_coadd_input_summary`` table. The table contains one row per visit detector image per deep coadd that the visit detector image contributes to. The columns are:

* ``tract``: ID number of the top level, 'tract', within the standard LSST skymap
* ``patch``: ID number of the second level, 'patch', within the standard LSST skymap
* ``visit``: visit ID number
* ``detector``: detector ID number
* ``weight``: dimensionless weight for the visit detector image when building the deep coadd, which is built using a weighted mean of the relevant visit detector images
* ``goodpix``: number of pixels from the visit detector image that overlap the coadd patch
* ``band``: name of the band used to take the visit detector image

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the ``deep_coadd_input_summary`` table.
