.. _catalogs-forced-source:

#############
Forced source
#############

Forced measurements in visit and difference images, at the coordinates of all objects.

Schema: `ForcedSource table <https://sdm-schemas.lsst.io/dp2.html#ForcedSource>`_

Access
======

The forced source table is accessible via the TAP and Butler services.

**Recommended access service:** TAP

TAP
---

* |ForcedSource_doi|
* Table name: ``ForcedSource``
* Columns: |ForcedSource_columns|
* Rows: |ForcedSource_rows|

Butler
------

* |object_forced_source_doi|
* Dataset type: ('object_forced_source', {**skymap**, **tract**, **patch**}, ArrowAstropy)
* Format: Parquet
* Number of Butler datasets: |object_forced_source_butler_count|

Forced sources are sharded by ``patch``, not just ``tract``, because in regions with many epochs the size of a full ``tract`` catalog becomes unwieldy.

Description
===========

"Forced" photometry means a measurement made at a fixed coordinate in an image, regardless of whether an above-threshold region was detected there in that particular image.

The forced source table contains forced PSF flux photometry on both the visit (i.e., "science" or "direct") and difference images at the coordinates of every object in the object table.

Forced photometry variants and blending
---------------------------------------

There are a total of four variants of forced photometry in Rubin data release processing for the combination of two different reference catalogs (Object and DiaObject) and two different measurement images (visit images and difference images).
Each of the two forced photometry catalogs includes rows for both measurement images and a single reference catalog (ForcedSource uses Object positions, while :doc:`DiaForcedSource </products/catalogs/dia_forced_source>` uses DiaObject positions).

Forced photometry on difference images is expected to behave best, especially in crowded regions, since image subtraction should remove neighbors even more effectively than the deblender run on the coadds (note that there is no deblending when producing the DiaSource catalog, either).
There is no deblending whatsoever in forced photometry on visit images.

Processing
----------

The forced source table is the result of :doc:`/processing/detection/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the forced source table.
