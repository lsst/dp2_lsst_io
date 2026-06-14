.. _catalogs-visit-detector:

##############
Visit detector
##############

Observation metadata for individual detectors (CCDs; date, time, band, PSF, zeropoint).

Schema: `VisitDetector table <https://sdm-schemas.lsst.io/lsstcam.html#VisitDetector>`_

Access
======

The visit detector table is accessible via the TAP and Butler services.

**Recommended access service:** TAP

TAP
---

* |VisitDetector_doi|
* Table name: ``VisitDetector``
* Columns: |VisitDetector_columns|
* Rows: |VisitDetector_rows|

Butler
------

* |visit_detector_table_doi|
* Dataset type: ('visit_detector_table', {**instrument**}, ArrowAstropy)
* Format: Parquet
* Number of Butler datasets: |visit_detector_table_butler_count|

Description
===========

A "visit" is an observation in a single filter, obtained at a given time and sky coordinate.
A "detector" is one of the 189 CCDs (charge-coupled devices) that make up LSSTCam.
A "VisitDetector" refers to an observation with a single detector as the reference for the observational metadata (e.g., airmass, seeing).
This table includes image characterization information measured from the image, including things such as the PSF size, sky background, and image zeropoint.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the visit detector table.
