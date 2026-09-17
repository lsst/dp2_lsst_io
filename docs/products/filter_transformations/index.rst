.. _filter-transformations:

######################
Filter transformations
######################

Filter transformations to/from the LSST photometric system and other astronomical photometric systems.

Access
======

To convert between Data Preview 2 (DP2) and other photometric systems, refer to the filter transformations in `RTN-125 <https://rtn-125.lsst.io/>`_.

Description
===========

**Filter transformations for DP2** were derived and are intended to support calibration and comparison across survey systems.
The filter transformation relations include both polynomial-fit equations and lookup-table-based methods.
They are generally valid for stars with typical spectral energy distributions (SEDs), and caution should be used when applying them to objects with strong emission lines or atypical colors.
Transformation relations are currently available to/from LSSTCam Data Preview 2 (DP2) photometric system and the photometric systems of the LSSTComCam (DP1), Dark Energy Survey (DES) (DR2), PanSTARRS (DR2), SDSS (DR18), Gaia (DR3), Euclid (Q1), and Johnson-Cousins (UBVRcIc).


Tutorials
---------

DP2 tutorial notebook describing the use of these transformations can be found in the  `Filter Transformations notebook <https://dp2.lsst.io/tutorials/notebook/311/notebook-311-2.html>`_.

