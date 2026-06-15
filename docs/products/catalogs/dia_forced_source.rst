.. _catalogs-dia-forced-source:

#################
DIA forced source
#################

Forced measurements in visit and difference images, at the coordinates of all DIA objects.

Schema: `ForcedSourceOnDiaObject table <https://sdm-schemas.lsst.io/lsstcam.html#ForcedSourceOnDiaObject>`_

Access
======

The DIA forced source table is accessible via the TAP and Butler services.

**Recommended access service:** TAP

TAP
---

* |ForcedSourceOnDiaObject_doi|
* Table name: ``ForcedSourceOnDiaObject``
* Columns: |ForcedSourceOnDiaObject_columns|
* Rows: |ForcedSourceOnDiaObject_rows|

Butler
------

* |dia_object_forced_source_doi|
* Dataset type: ('dia_object_forced_source', {**skymap**, **tract**, **patch**}, ArrowAstropy)
* Format: Parquet
* Number of Butler datasets: |dia_object_forced_source_butler_count|

Forced sources are sharded by patch, not just tract, because in regions with many epochs the size of a full tract table becomes unwieldy.

Description
===========

"Forced" photometry means a measurement made at a fixed coordinate in an image, regardless of whether an above-threshold region was detected there in that particular image.

The DIA forced source table contains forced PSF flux photometry on both the visit (i.e., "direct" or "science") and difference images at the coordinates of every object in the DIA object table.

Processing
----------

The DIA forced source table is the result of :doc:`/processing/dia/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the DIA forced source table.
