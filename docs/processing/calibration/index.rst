#################
Image calibration
#################

The process of transforming a post-ISR image into a science-ready ``visit_image``.


The Monster catalog
===================

A whole-sky standard star catalog built specifically for LSST.

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    monster


Photometric calibration
=======================

Photometric calibrations relate the counts (electrons) measured by the camera to the incident flux from a source at Earth before it enters the atmosphere.

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    photometric


Astrometric calibration
=======================

Astrometric calibrations define the World Coordinate System (WCS) of an image, the relation between pixel coordinates and sky coordinates (Right Ascension and Declination).

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    astrometric


Point spread function modeling
==============================

Characterizing how the optical system and atmosphere "blur" a point source into a two-dimensional shape on the detector called the point spread function (PSF).

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    psf


Background subtraction
======================

Removing the sky background flux from the image.

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    backgrounds
