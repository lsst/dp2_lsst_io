.. _maps_skymap:

###############################
The Skymap (tracts and patches)
###############################

An all-sky tessellation of pre-defined regions.

Access
======

The skymap is accessible via the Butler in the Notebook Aspect.
Tract and patch boundaries for the skymap are available via TAP.

Data Preview 2 uses the skymap ``lsst_cells_v2``.

Butler
------

* Dataset type: ('skyMap', {**skymap**}, SkyMap)

TAP
---

* Table name: ``CoaddPatches``

Description
===========

The DP2 skymap, ``lsst_cells_v2``, is *not the same* as the DP1 skymap.

The skymap is divided into tracts of about 1.66 degrees per side.
Each tract is subdiveded into 100 patches.
Patches and tracts overlap slightly at their edges.
The LSST deep coadd and template image are generated, and available to users, per patch.


Tutorials
---------

See the :doc:`/tutorials/index` page for a tutorial on how to load and explore the Skymap.

