.. _catalogs-mpcorb:

##########
MPC orbits
##########

The orbit catalog produced by the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC).
The MPC has assigned `"observatory code" <https://minorplanetcenter.net/iau/lists/ObsCodesF.html>`_ ``X05`` to the Rubin Observatory.

Schema: `mpc_orbits table <https://sdm-schemas.lsst.io/lsstcam.html#mpc_orbits>`_

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

The MPCORB table contains the orbital parameters calculated by the MPC for all known solar system objects and the linked difference-image detections of moving objects submitted by Rubin.

Processing
----------

The MPCORB catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the MPC orbits table.
