.. _catalogs-ss-source:

#########
SS source
#########

Instantaneous physical parameters for moving objects at the time of every observation.

Schema: `SSSource table <https://dp1.lsst.io/products/catalogs/ss_source.html>`_

Access
======

The SS source table is accessible via the TAP and Butler services.

**Recommended access service:** TAP

TAP
---

* |SSSource_doi|
* Table name: ``SSSource``
* Columns: |SSSource_columns|
* Rows: |SSSource_rows|

Butler
------

* |ss_source_doi|
* Dataset type: ('ss_source', {}, ArrowAstropy)
* Format: Parquet
* Number of Butler datasets: |ss_source_butler_count|

Description
===========

A "Solar System source" is a signal-to-noise ratio > 5 moving object detection in a visit image.

The SS source table contains the 2-d (sky) coordinates and 3-d distances and velocities for every ``SSObject`` at the time of every LSST observation of that ``SSObject``.

Processing
----------

The SS source table is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS source table.

