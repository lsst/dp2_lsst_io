.. _bandpasses:

##########
Bandpasses
##########

LSSTCam filter bandpasses.

|standard_passband_doi|


Access
======

The bandpasses are accessible via the Butler.

* Dataset type: ``('standard_passband', {band, instrument}, ArrowAstropy)``
* Number of files: |standard_passband_butler_count|


Description
===========

There are six standard passband datasets, one for each of the *ugrizy* bands.
These datasets tabulate the full-system transmission of the six filters as a function of wavelength, which was used as a reference for the photometry.

Find more information (e.g., effective wavelength, a throughput curve plot) in the `LSSTCam documentation <https://lsstcam.lsst.io/#filters>`_.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the passbands.
