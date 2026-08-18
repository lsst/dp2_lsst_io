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

A DP2 “Solar System source” is a signal-to-noise ratio > 5 moving object detection in a difference image that has been associated with a previously known small body.

Each row in the ``SSSource` table represents a one-to-one positional association between a ``DiaSource`` and the predicted position of a known small body.
The table combines selected measured astrometry and photometry with ephemeris quantities predicted from an MPC orbit using `Sorcha <https://sorcha.space/>`_ (see `Merritt et al. 2025 <https://scixplorer.org/abs/2025AJ....170..100M/abstract>`_ and `Holman et al. 2025 <https://scixplorer.org/abs/2025AJ....170...97H/abstract>`_).
The DP2 ``SSSource`` table does _not_ include unassociated detections, and its ephemeris quantities are _not_ a Rubin-derived orbit solution.

The association used a 1-arcsecond radius without positional uncertainties or photometric information.
The input catalog was a 2026 March 13 MPC orbit snapshot restricted to observational arcs longer than two days.
Small bodies discovered later than this date are not represented in the DP2 tables.
No comets were associated in the DP2 tables, because the input MPC orbit catalog did not contain them.
For further information on the algorithm and selection effects, see the :doc:`/processing/moving/ss_association` documentation.

The table can be joined to the :doc:`DiaSource <dia_source>` table for the complete detection record and quality fields (including flux measurements) using the ``diaSourceId`` unique identifier, and joined to the :doc:`SSObject <ss_object>` table on ``ssObjectId`` for object-level summaries.
The ``designation`` column is the object's unpacked primary provisional designation.

The ``diaDistanceRank`` column is 1 for every delivered row and should not be used as an association-quality or ambiguity metric.
For quality filtering, use the measured-minus-predicted offsets together with ``DiaSource`` reliability information and consistency with the predicted magnitude.

Processing
----------

The SS source table is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS source table.
