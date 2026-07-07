################
Image coaddition
################

Coadded images are created by combining multiple calibrated images obtained in the same filter and of the same region of the sky, either to achieve greater sensitivity (i.e., "depth") or a sharper image (i.e., by selecting images with good seeing).


.. _coaddition-patch:

Coadds by patch
===============

When producing coadded images, the processing pipeline refers to a skymap, which defines a grid of
2.8 square degree tracts covering the entire celestial sphere.
Each tract is further subdivided into 10 by 10 equally-sized patches, each covering
approximately 128 square arcminutes.

To create a coadded image, the processing pipeline selects all suitable
(i.e., surpassing certain quality thresholds) calibrated visit images
covering a given patch in a given filter,
warps them onto a single consistent pixel grid for that patch,
then coadds them.

Each individual coadd image covers a single patch.
Patches at the edge of the survey area might not be entirely covered
by the input visit images.
Unobserved sections of the deep coadd image have pixels with
no data flagged as "NO_DATA" in the mask plane.


Cell-based coadds
-----------------

The coadd images are made up of a grid of 22 by 22 150-pixel "cells", within which only visit images that *completely* overlap the cell are used as inputs.
The effect is that, within a cell, the PSF may be considered "edge-free": it was created in a way that avoids any discontinuities.
The cells are created separately, then stitched together into the coadd image.
Thus the set of input visits for each cell can be slightly different from neighboring cells, within the same coadd image.

Cell coadd injection is not yet working for DP2, so characterization relies on matching against external catalogs in deep drilling fields.
A large injection campaign is planned for the DP2 release.



.. _coaddition-visitselect:

Input image selection
=====================

.. important::

   Parameter values to be added from `RTN-121 <https://rtn-115.lsst.io>`_.

**Deep coadd images**: as the name suggests, these images are created to have the faintest limiting magnitudes for source detection.
For the deep coadds, a visit image had to have a circular (ellipticity < ``max_ell``) PSF with FWHM < ``max_psf`` arcseconds to be selected as an input image.
This selection is the most inclusive of all coadd productions and aims to maximize the depth of the coadd while still ensuring good image quality.

For the template coadds, good seeing (low PSF FWHM) is more important than depth.
The ``fraction`` of visit images with the smallest PSF are selected to contribute to a template coadd image.
If that corresponds to less than ``number`` visits, then the ``min_num`` of visits with the smallest PSFs are selected.
This strategy optimizes for image quality in regions that are well-covered by visit images, while also ensuring template coadd images exist in regions that are not well covered.


.. _coaddition-algorithm:

Image combination algorithm
===========================

A mean stacking algorithm, weighted by inverse variance, combines selected exposures.
To mitigate transient artifacts before coaddition, an artifact rejection procedure first identifies and masks features such as satellite trails, optical ghosts, and cosmic rays.
See "Coaddition Artifact Rejection and CompareWarp" (`dmtn-080.lsst.io <https://dmtn-080.lsst.io/>`_).


.. _coaddition-background:

Coadd background subtraction
============================

After image coaddition, a constant background residual is fit and subtracted.
