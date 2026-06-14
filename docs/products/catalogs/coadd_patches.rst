.. _catalogs-coadd-patches:

#############
Coadd patches
#############

Coordinates and regions for the patches of the all-sky map.

Schema: TBD

Access
======

The coadd patches catalog is accessible via the TAP service only.

TAP
---

* Table name: ``CoaddPatches``
* Columns:
* Rows:

Description
===========

The LSST all-sky map is divided into "tracts" (about the size of the LSSTCam field of view, 9.6 square degrees).
Each tract is subdivided into "patches": quadrilateral sub-regions that each cover approximately 128 square arcminutes (about the size of one LSSTCam detector).

The tracts are numbered with four-digit numbers, and the patches with three-digit numbers.
While tract numbers are unique, patch numbers are only unique for a given patch.
To uniquely identify a patch, both the tract and patch numbers are required.

Tutorials
---------

TBD
