.. _catalogs-visit:

#####
Visit
#####

Observation metadata for the full focal plane (date, time, band, coordinates).

Schema: `Visit table <https://sdm-schemas.lsst.io/dp2.html#Visit>`_

Access
======

The visit table is accessible via the TAP and Butler services.

**Recommended access service:** TAP

TAP
---

* |Visit_doi|
* Table name: ``Visit``
* Columns: |Visit_columns|
* Rows: |Visit_rows|

Butler
------

* |visit_table_doi|
* Dataset type: ('visit_table', {**instrument**}, ArrowAstropy)
* Format: Parquet
* Number of Butler datasets: |visit_table_butler_count|

Description
===========

A "visit" is an observation in a single filter, obtained at a given time and sky coordinate.
It refers to an observation with the full focal plane, with the boresight (center) as the reference for the observational metadata (e.g., airmass).

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the visit table.

