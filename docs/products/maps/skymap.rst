.. _maps_skymap:

###############################
The Skymap (tracts and patches)
###############################

An all-sky tessellation of pre-defined regions.

Access
======

The skymap is accessible via the Butler in the Notebook Aspect.
Tract and patch boundaries are also available in the TAP table ``CoaddPatches``.

Description
===========

The skymap is divided into tracts of about 1.66 degrees per side.
Each tract is subdiveded into 100 patches.
Patches and tracts overlap slightly at their edges.
The LSST deep coadd and template image are generated, and available to users, per patch.

Data Preview 2 uses the skymap ``lsst_cells_v2``.

Tutorials
---------

See the :doc:`/tutorials/index` page for a tutorial on how to load and explore the Skymap.

