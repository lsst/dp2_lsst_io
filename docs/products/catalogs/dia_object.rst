.. _catalogs-dia-object:

##########
DIA object
##########

Derived properties for transient and variable objects.

Schema: TBD

Access
======

The DIA object catalog is accessible via the TAP and Butler services.

**Recommended access service:** TAP

TAP
---

* |DiaObject_doi|
* Table name: ``DiaObject``
* Columns: |DiaObject_columns|
* Rows: |DiaObject_rows|

Butler
------

* |dia_object_doi|
* Dataset type: ('dia_object', {**skymap**, **tract**}, ArrowAstropy)
* Format: Parquet
* Number of Butler datasets: |dia_object_butler_count|

Description
===========

A "DIA object" is an astrophysical transient or variable object at a static sky coordinate.

The DIA object table is created by associating DIA sources within a 1 arcsecond radius.
DIA objects are assigned to a ``tract`` and ``patch`` based on their ``ra, dec`` at ``radecMjdTai``.
DIA objects are only created in the non-overlapping ``tract`` and ``patch`` inner regions.

The DIA object catalog contains derived per-filter variability parameters such as the minimum, mean,
maximum, standard deviation and skew in the difference-image fluxes, and the light curve’s slope, percentiles,
and StetsonJ parameter.


Processing
----------

The DIA object catalog is the result of :doc:`/processing/dia/index`.

Tutorials
---------

TBD.