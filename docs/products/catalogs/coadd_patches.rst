.. _catalogs-coadd-patches:

#############
Coadd patches
#############

Coordinates and regions for the patches of the all-sky map.

Schema: `CoaddPatches table <https://sdm-schemas.lsst.io/lsstcam.html#CoaddPatches>`_

Access
======

The coadd patches table is accessible via the TAP service only.

TAP
---

* Table name: ``CoaddPatches``
* Columns: |CoaddPatches_columns|
* Rows: |CoaddPatches_rows|

Description
===========

The LSST all-sky map is divided into "tracts".
One tract is one square region of LSST's all-sky tesselation ("skymap"), $~1.66$ deg per side.
Tracts are subdivided into 100 overlapping patches, and one deep coadd image is created per patch.

The tracts are numbered with four-digit numbers, and the patches with three-digit numbers.
While tract numbers are unique, patch numbers are only unique for a given patch.
To uniquely identify a patch, both the tract and patch numbers are required.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the coadd patches table.
