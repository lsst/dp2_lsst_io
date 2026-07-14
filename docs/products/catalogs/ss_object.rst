.. _catalogs-ss-object:

#########
SS object
#########

Derived parameters for moving (Solar System) objects.

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

A "Solar System object" is a moving object for which groupings of difference image detections (``DIASources``) have been linked together.

The SS object table contains the unique ``SSObjectId`` identifier, number of observations, and the date of the discovery submission (if a new discovery) for each solar system object detected with signal-to-noise ratio >5.

Processing
----------

The SS object table is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS object table.

