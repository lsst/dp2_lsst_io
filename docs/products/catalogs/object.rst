.. _catalogs-object:

######
Object
######

Measurements of static astronomical objects (or the static aspects of variable and slowly-moving objects) detected and measured on coadds.

Schema: `Object table <https://sdm-schemas.lsst.io/dp2.html#Object>`_

Access
======

The object table is accessible via the TAP and Butler services.

**Recommended access service:** TAP

TAP
---

* |Object_doi|
* Table name: ``Object``
* Columns: |Object_columns|
* Rows: |Object_rows|

Butler
------

* |object_doi|
* Dataset type: ('object', {**skymap**, **tract**}, ArrowAstropy)
* Format: Parquet
* Number of Butler datasets: |object_butler_count|

Description
===========

An "object" is an astrophysical object at a static sky coordinate.

The object table contains forced measurements on the deep coadd images at the coordinates of every object detected with signal-to-noise ratio >5 in a deep coadd image of any filter.

Measurements include PSF and extended fluxes, shapes, and sizes, as well as processing pixel flags.
Photometry is calibrated, but not corrected for Milky Way dust extinction.

Objects are detected and deblended in each patch independently, including the "outer" patch regions that overlap.
They are then filtered down to just those whose reference-band centroid falls within the inner (non-overlapping) patch bounds when per-patch catalogs are aggregated.

Processing
----------

The object table is the result of :doc:`/processing/detection/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the object table.
