.. _catalogs-current-identifications:

#######################
Current identifications
#######################

A catalog snapshot adopted from the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC) containing mappings among provisional designations for the same physical object.

Schema: `current_identifications table <https://sdm-schemas.lsst.io/dp2.html#current_identifications>`_

Access
======

The current identifications table is accessible via the TAP service only.

TAP
---

* Table name: ``current_identifications``

Butler
------

Not available.

Description
===========

The ``current_identifications`` table is an auxiliary snapshot adopted from the `Minor Planet Center PostgreSQL database <https://data.minorplanetcenter.net/mpcops/documentation/replicated-tables-schema/>`_.
It maps primary and secondary provisional designations that refer to the same physical object and includes both self-mappings and mappings among aliases.
The upstream definitions and field semantics are documented in the `MPC replicated-tables schema <https://docs.minorplanetcenter.net/mpc-ops-docs/data-and-services/replicated-tables-schema/>`_.

This table is useful for resolving historical or alternate designations before joining to the Rubin-derived Solar System tables.
It was _not_ computed from Rubin DP2 observations.
