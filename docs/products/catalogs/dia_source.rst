.. _catalogs-dia-source:

##########
DIA source
##########

Measurements for sources detected in difference images.

.. (This line commented-out for now)  Schema: `DiaSource table <https://sdm-schemas.lsst.io/dp1.html#DiaSource>`_

Access
======

The DIA source catalog is accessible via the TAP and Butler services.

**Recommended access service:** TAP

TAP
---

.. (This line commented-out for now)  * |DiaSource_doi| [:download:`BibTeX </bib/tap-DiaSource.bib>`]
* Table name: ``DiaSource``
* Columns: |DiaSource_columns|
* Rows: |DiaSource_rows|

Butler
------

.. (This line commented-out for now)  * |source_doi| [:download:`BibTeX </bib/butler-dia_source.bib>`]
.. (This line commented-out for now)  * :ref:`Dataset type <products_butler_terminology>`\ : ('dia_source', {**skymap**, **tract**}, ArrowAstropy)
* Format: Parquet
* Number of Butler datasets: |source_butler_count|

DIA sources are sharded by ``tract``, not ``visit`` in order to make them easier to join to their :ref:`catalogs-dia-object`.

Description
===========

A "DIA source" is a signal-to-noise ratio > 5 detection in a difference image.

The DIA source catalog contains measurements on a difference image
at the coordinates of every source detected in that difference image.
These measurements include PSF-fit and forced PSF fluxes, and aperture and
trailed-source fluxes.
Forced PSF fluxes on the corresponding visit (i.e., "direct" or "science") image
at the coordinates of the DIA source are also included.


Processing
----------

The DIA source catalog is the result of :doc:`/processing/dia/index`.

Tutorials
---------

**UPDATE FOR DP2**

.. (This line commented-out for now) See the :ref:`200-level notebook <notebook-200>` or :ref:`200-level portal <portal-200>`
.. (This line commented-out for now) tutorials demonstrating how to access the DIA source table.
