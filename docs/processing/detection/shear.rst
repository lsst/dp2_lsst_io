#################
Shear measurement
#################

Shear object detections and measurements in four "counterfactual" deep coadd images with different small shear values, plus a fifth with no shear.
All shear measurements are stored in the catalog :doc:`/products/index`.


Counterfactual images
=====================

The ``GalSim`` ``Shear`` class is used to create four "counterfactual" versions of the deep coadd images with different artificial linear shears introduced, using the "reduced shear" option with a step value (``g1``, ``g2``) of +0.01 or -0.01 (`GalSim documentation <https://galsim-developers.github.io/GalSim/_build/html/overview.html>`_; `Sheldon et al. 2023 <https://ui.adsabs.harvard.edu/abs/2023OJAp....6E..17S/abstract>`_).
The application of (weak lensing) shear to an image essentially performs a transformation that adds ellipticity with a set minor-to-major axis ratio and position angle, but approximately conserves area.

The five counterfactual images and their shear directions are:

* ``ns``: no shear
* ``1p``: x-direction
* ``1m``: y-direction
* ``2p``: 45 degree diagonal
* ``2m``: 135 degree diagonal


Detection and measurement
=========================

All objects detected with a significance of :math:`> 5\sigma` in a given counterfactual image have their shapes measured.

Two approaches are used for measurement (`Yamamoto et al. 2025 <https://ui.adsabs.harvard.edu/abs/2025MNRAS.543.4156Y/abstract>`_).

1. A Gaussian forward model fit across the r,i,z band images individually (``gauss``). The resulting ellipticity components in the image x-y (``gauss_g1``) and diagonal (``gauss_g2``) directions are measured.

2. A specialized Fourier-space method to measure a Gaussian weighted flux for each detection on a PSF-deconvolved image (``pgauss``). This flux measure has the advantage of matching the effective flux apertures between different bands, which is important for the measurement of object colors and photometric redshifts.

Both ``gauss`` and ``pgauss`` measure pre-PSF properties of the object.

In addition to the measurements above, the positions, sizes, and PSF shapes are also measured.


Resulting catalog
=================

The results of shear detection and measurement are stored in the ``ShearObject`` catalog.

If a given galaxy is detected with :math:`> 5\sigma` significance in all five counterfactual images, then it will populate five rows of the ``ShearObject`` table (with five separate ``shearObjectId``).

The process of detecting and measuring objects in the counterfactual images, and the resulting ``ShearObject`` table, is completely independent of the process that creates the ``Object`` table.
These two tables are not joinable and have independent (unique and unrelated) object identifiers.
