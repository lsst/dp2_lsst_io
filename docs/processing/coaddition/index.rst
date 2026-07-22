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
approximately 122 square arcminutes.

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



.. _coaddition-visitselect:

Input image selection
=====================

**Deep coadd images**: as the name suggests, these images are created to have the faintest limiting magnitudes for source detection.
For the deep coadds, a visit image had to have a circular (ellipticity < 0.2) PSF with median FWHM < 1.7 arcseconds to be selected as an input image.
An additional criterion removes images that do not meet depth criteria based on the "effective exposure time," which is the exposure time that would be needed under nominal conditions to achieve the same signal-to-noise as the visit image in question.
Images with effective exposure time less than 0.75 times the actual exposure time were excluded from coaddition.
In addition to the per-visit image quality selection, detectors that are outliers in various PSF shape and model-fitting parameters are excluded.
This selection is the most inclusive of all coadd productions and aims to maximize the depth of the coadd while still ensuring good image quality.

**Per-cell contributions**: The visits that contribute to the coadd can differ for each 150x150-pixel cell within the coadd patch.
There are 6 reasons that a visit would not be contributing to a particular pixel:
* None of the visit's detectors (in the raw image) overlap that pixel (e.g., because it's in a chip gap).
* The input data products associated with the overlapping detectors for that visit don't exist because one of the upstream single-frame processing tasks failed. A `preliminary_visit_image` and `visit_summary` containing successful photometric and astrometric solutions for that detector are needed.
* The detector or visit was excluded because it did not meet the PSF or transparancy thresholds outlined above.
* The visit's contributing pixels were masked with any of the visit-level bad mask planes: `'NO_DATA', 'BAD', 'SAT', 'SUSPECT', 'PARTLY_VIGNETTED', 'SPIKE'`.  This will flip on the coadd-level `REJECTED` mask bit in the `deep_coadd` to indicate that at least one of the inputs visits have been rejected for that pixel.
* The visit's input pixels were labeled as an "artifact" (e.g., a satellite trail, cosmic ray, or optical ghost) and were marked `CLIPPED` in the resulting `deep_coadd`.
* The visit overlaps less than 50% of the cell's pixels, _or_ one of the visit's chip gaps overlaps the cell, and thus the full visit was excluded from contributing to the whole cell.

..
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

After image coaddition, a background residual is fit and subtracted following a procedure similar to that described in :doc:`processing/calibration/backgrounds`.



WHAT SHOULD I CALL THIS SECTION?
================================

There are 6 reasons that a visit would not be contributing to a pixel in a coadd. This tutorial demonstrates how to check which of these 5 reasons are in play:
1) **raws overlap**: None of the visits detectors (raws) overlap that pixel.
2) **single-frame processing succeeds**: The input data products associated with the overlapping detectors for that visit don't exist because one of the upstream tasks failed. A `preliminary_visit_image` and `visit_summary` containing successful photometric and astrometric solutions for that detector are needed.
3) **meets quality criteria**: the detectors or visits were excluded because they did not meet PSF or transparancy thresholds
4) **pixel was not REJECTED** : The input pixels were masked with any of the visit-level bad mask planes: `'NO_DATA', 'BAD', 'SAT', 'SUSPECT', 'PARTLY_VIGNETTED', 'SPIKE'`.  This will flip on the coadd-level `REJECTED` mask bit in the `deep_coadd` to indicate that at least one of the inputs visits have been rejected for that pixel.
5) **pixel not CLIPPED**: The input pixels were labeled as an artifact and were marked `CLIPPED` in the resulting `deep_coadd`
6) In the cell-based `deep_coadd`, if < 50% of a visit overlaps a cell (not CLIPPED, REJECTION), it is excluded from the whole cell.

I am a pixel in a visit. Why might I contribute to a coadd? My raw image needs to overlap that patch. I needed to have succeeded through single-frame processing. I must meet the PSF and transparancy requirements. I must not boe labeled `'NO_DATA', 'BAD', 'SAT', 'SUSPECT', 'PARTLY_VIGNETTED', 'SPIKE'`. I must not be "CLIPPED" during artifact rejection. And ALL of my neighbors in my cell, must be not be a chip gap AND <50% of my neighbors in my cell must be not REJECTED or CLIPPED.