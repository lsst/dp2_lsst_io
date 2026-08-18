.. _catalogs-mpc-orbits:

##########
MPC orbits
##########

An orbit-catalog snapshot adopted from the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC).
The MPC has assigned `"observatory code" <https://minorplanetcenter.net/iau/lists/ObsCodesF.html>`_ ``X05`` to the Rubin Observatory.

Schema: `mpc_orbits table <https://sdm-schemas.lsst.io/dp2.html#mpc_orbits>`_

Access
======

The MPC orbits catalog is accessible via the TAP service only.

TAP
---

* Table name: ``mpc_orbits``
* Columns: |mpc_orbits_columns|
* Rows: |mpc_orbits_rows|

Butler
------

Not available.


Description
===========

The `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC) is the single worldwide location for receipt and distribution of positional measurements of small bodies.
The MPC is responsible for the identification, designation, and orbit computation for all of these objects.

The ``mpc_orbits`` table is a snapshot of the `MPC PostgreSQL orbit table <https://data.minorplanetcenter.net/mpcops/documentation/replicated-tables-schema/>`_.
It contains MPC orbital elements, photometric parameters, fit metadata, and uncertainties; it does _not_ contain Rubin difference-image detections and was _not_ derived by fitting Rubin DP2 observations.
The upstream definitions and field semantics are documented in the `MPC replicated-tables schema <https://docs.minorplanetcenter.net/mpc-ops-docs/data-and-services/replicated-tables-schema/>`_.

For end-user convenience, DP2 adds a ``designation`` column containing the unpacked primary provisional designation.
It is identical row by row to ``unpacked_primary_provisional_designation`` and provides a readable join key to the ``SSSource`` and ``SSObject`` tables.
For improved performance, SQL joins on integer identifiers such as ``ssObjectId`` are preferred wherever they are available.

The delivered DP2 ``mpc_orbits`` table is the same 2026 March 13 MPC orbit snapshot used as input for :doc:`DP2 association </processing/moving/ss_association>`.

Processing
----------

The table was adopted from the MPC database; its contents were not recomputed by DP2 processing.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the MPC orbits table.
