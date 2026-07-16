.. _object-shear:

############
Object shear
############

Descriptions of shear objects detected and measured on coadds with metadetection.

Schema: `ShearObject table <https://sdm-schemas.lsst.io/dp2.html#ShearObject>`_

Access
======

The object shear table is accessible via the TAP and Butler services.

**Recommended access service:** TAP

TAP
---

* |ShearObject_doi|
* Table name: ``ShearObject``
* Columns: |ShearObject_columns|
* Rows: |ShearObject_rows|

Butler
------

* |object_shear_doi|
* Dataset type: ('object_shear_all', {**skymap**, **tract**}, ArrowAstropy)
* Format: Parquet
* Number of Butler datasets: |object_shear_butler_count|

Description
===========

TBD

Processing
----------

TBD

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the object shear table.
