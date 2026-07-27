.. _catalogs-numbered-identifications:

########################
Numbered identifications
########################

Minor Planet Center mappings between permanent numbers and primary provisional designations.

Schema: `numbered_identifications table <https://sdm-schemas.lsst.io/dp2.html#numbered_identifications>`_

Access
======

The numbered identifications table is accessible via the TAP service only.

TAP
---

* Table name: ``numbered_identifications``
* Columns: |current_identifications_columns|
* Rows: |current_identifications_rows|

Butler
------

Not available.

Description
===========

The ``numbered_identifications`` table is an auxiliary snapshot adopted from the Minor Planet Center PostgreSQL database.
It connects an object's permanent number to its primary provisional designation and includes IAU designations, names, discovery credits, and publication references where available.
The upstream definitions and field semantics are documented in the `MPC replicated-tables schema <https://docs.minorplanetcenter.net/mpc-ops-docs/data-and-services/replicated-tables-schema/>`_.

This table is useful for translating between permanent numbers and the provisional designations used by the Rubin-derived Solar System tables.
It was not computed from DP2 observations.

The ``current_identifications`` table is joinable with the ``mpc_orbits`` table and ``numbered_identifications`` table using the ``packed_primary_provisional_designation`` and ``unpacked_primary_provisional_designation``.

Processing
----------

The table was adopted from the MPC database; its contents were not recomputed by DP2 processing.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the MPC orbits table.
