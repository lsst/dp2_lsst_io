#################
Shear measurement
#################

Shear object detections and measurements in four "counterfactual" deep coadd images with different shear steps, plus a fifth with no shear.
All shear measurements are stored in the catalog :doc:`/products/index`.


Counterfactual images
=====================

The ``GalSim`` ``Shear`` class is used to create five "counterfactual" versions of the deep coadd images with different artificial linear shears introduced, using the "reduced shear" option with a step value (``g1``) of 0.01 (`GalSim documentation <https://galsim-developers.github.io/GalSim/_build/html/overview.html>`_; `Rowe et al. 2025 <https://ui.adsabs.harvard.edu/abs/2023OJAp....6E..17S/abstract>`_).
The application of shear to an image essentially performs a convolution that adds ellipticity with a set minor-to-major axis ratio and position angle, but conserves area.

The five counterfactual images and their shear directions are:

* ``ns``: no shear
* ``1p``: x-direction
* ``1m``: y-direction
* ``2p``: 45 degree diagonal
* ``2m``: 135 degree diagonal


Detection and measurement
=========================

All objects detected with a significance of :math:`> 5\sigma` in a given counterfactual image have their shapes measured.

Two approaches are used for shape measurement:
1. A single 2D gaussian is fit jointly across the r, i, and z-band deep coadd images. The resulting ellipticity components in the image x-y (``gauss_g1``) and diagonal (``gauss_g2``) directions are measured.
2. The 2D gaussian-weighted moments on single band (r, i, z) images are fit separately. The resulting ellipticity components of the sum, weighted by inverse-variance, in the image x-y (``pgauss_g1``) and diagonal (``pgauss_g2``) directions are measured.

In addition to the shape measurements above, the positions, fluxes, gaussian moments, and PSF shapes are also measured.


Resulting catalog
=================

The results of shear detection and measurement are stored in the ``ShearObject`` catalog.

If a given galaxy is detected with :math:`> 5\sigma` in all five counterfactual images, then it will populate five rows of the ``ShearObject`` table (with five separate ``shearObjectId``).

The process of detecting and measuring objects in the counterfactual images, and the resulting ``ShearObject`` table, is completely independent of the process that creates the ``Object`` table.
These two tables are not joinable and have independent (unique and unrelated) object identifiers.
