.. _catalogs-ss-source:

#########
SS source
#########

Measured and predicted single-epoch quantities for detections associated with known small bodies.

Schema: `SSSource table <https://sdm-schemas.lsst.io/dp2.html#SSSource>`_

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

Each row represents a one-to-one positional association between a ``DiaSource`` and the predicted position of a known small body.
The table combines selected measured astrometry and photometry with ephemeris quantities predicted from an MPC orbit using Sorcha.
It does not include unassociated detections, and its ephemeris quantities are not a Rubin-derived orbit solution.

The association used a 1-arcsecond radius without positional uncertainties or photometric information.
The input catalog was a 2026 March 13 MPC orbit snapshot restricted to observational arcs longer than two days.
Small bodies discovered later are not represented, and no comets were associated because the input orbit catalog did not contain them.
See :doc:`/processing/moving/ss_association` for the algorithm and selection effects.

Every ``diaSourceId`` occurs at most once.
Join to :doc:`DiaSource <dia_source>` on ``diaSourceId`` for the complete detection record and quality fields, and join to :doc:`SSObject <ss_object>` on ``ssObjectId`` for object-level summaries.
The ``designation`` column is the object's unpacked primary provisional designation.

The ``diaDistanceRank`` column is 1 for every delivered row and should not be used as an association-quality or ambiguity metric.
For quality filtering, use the measured-minus-predicted offsets together with ``DiaSource`` reliability information and consistency with the predicted magnitude.

Processing
----------

The SS source table is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS source table.
