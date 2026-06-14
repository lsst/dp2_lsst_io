.. _bandpasses:

##########
Bandpasses
##########

LSSTCam filter bandpasses.


|bandpasses_doi|


Access
======

The bandpasses are accessible via the Butler.


Butler
------

Examples of dataset types:

* ``('standard_passband', {band, instrument}, ArrowAstropy)``


Description
===========

There are six ``standard_passband`` datasets, one for each of the *ugrizy* bands.
These datasets tabulate the full-system transmission of the six filters as a function of wavelength, which was used as a reference for the photometry.
The ``standard_passband`` dataset is keyed by band and is in Astropy Table format.

Tutorials
---------

TBD