.. _products_known_issues:

############
Known issues
############

Crowded fields and deblend skipping
===================================

The DP2 observations include several fields that present a challenge for our pipelines, especially with respect to crowding.

The most common failure mode is when so many peaks are detected in a single above-threshold region that the deblender determines that it would run out of memory trying to separate them and simply gives up, resulting in few or no entries in the :ref:`catalogs-object` catalog in that region.
Poor deconvolution in crowded fields can also leave rings that connect large regions, resulting in blends that span most or all of a patch; we do not have the compute to process blends that large, so measurements may be missing for many patches in dense stellar fields.

We are currently integrating a major change to our deblender that we hope will mitigate this problem in future processing, and future data releases will include a coadd mask plane indicating when skipping occurs, making the problem easier to diagnose.

It is likely that our direct image processing in crowded fields will still lag dedicated crowded-field photometry codes; Rubin's focus for these fields has always been image subtraction (where we do expect to compete with the state of the art), with the :ref:`catalogs-object` catalog a best-effort addition.

.. _products_wcs_known_issues:

WCS FITS approximations and misleading interfaces
==================================================

Rubin's single-visit World Coordinate System (WCS) objects are not, in general, exactly representable via the FITS WCS standard.
The FITS WCS in the headers of the :ref:`visit image <images-visit-image>` and :ref:`difference image <images-difference-image>` data products (they are the same) are ``TAN-SIP`` approximations that were expected to be good enough for visualization and object finding, not for precision astrometry.

Current recommendations for WCS use
------------------------------------

To make use of the full WCS of a visit image or difference image, it is necessary to use Rubin's ``lsst.afw.geom.SkyWcs`` objects::

  wcs = visit_image.wcs
  x, y = wcs.skyToPixelArray(ra, dec, degrees=True)  # or False for radians
  ra, dec = wcs.pixelToSkyArray(x, y, degrees=True)

where ``x``, ``y``, ``ra``, and ``dec`` are double-precision (``dtype=np.float64``) NumPy arrays.

Unfortunately, these ``lsst.afw.geom.SkyWcs`` objects are not included in our Python API documentation reference at present, and they are counterintuitive and easy to misuse in several ways, which can make it appear as if the WCS fits are terrible.
These problems center around the fact that their ``getSkyOrigin`` and ``getPixelOrigin`` methods generally return a point that is far off the image the WCS corresponds to (where the mapping can extrapolate poorly), and other methods like ``getPixelScale`` will by default evaluate at this often-irrelevant point.
The ``skyToPixel[Array]`` and ``pixelToSky[Array]`` are always safe to use, and ``getPixelScale`` can be used safely if you explicitly provide it with the point to evaluate the pixel scale at.
Most other methods are best avoided (including ``getFitsMetadata``, which does *not* return the FITS approximation in the file headers).

The WCSs objects attached to coadds *are* exactly representable in FITS WCS, but can also be tricky to use since each full ``tract`` has a single WCS, and each patch image has an integer offset relative to that coordinate system.
So if you're indexing the NumPy array that backs a coadd image directly, e.g.::

  coadd.image.array[i, j]

be aware that this corresponds to the ``x`` and ``y`` pixel coordinates of the WCS via::

  bbox = coadd.getBBox()
  i = y - bbox.y.min
  j = x - bbox.x.min

When we write a coadd image to a file, we offset the WCS in the FITS header to be consistent with the pixels (since FITS requires that the origin of any image be ``(1, 1)``).
But this is not done when calling ``wcs.getFitsMetadata()``, since the necessary offset is stored with the coadd object, not the WCS, and this is another common source of confusion.

Finally, the WCS objects of :ref:`raw <images-raw>` images simply should not be used; they are based on an initial guess from the telescope at its pointing, but at present this can be quite far off from the true pointing.
The corresponding :ref:`visit image <images-visit-image>` WCS should be used instead.

**Please do not:**

- Call ``getPixelOrigin()``. This may return CRPIX for a FITS WCS, or a value that is not meaningful for your data.
- Call ``getSkyOrigin()``. This may return CRVAL for a FITS WCS, or a value that is not meaningful for your data.
- Call ``getCdMatrix()``. This may return the CD matrix for a FITS WCS, or a value that is not meaningful for your data.
- Call ``getPixelScale()`` without arguments. This is evaluated at ``getPixelOrigin()``, which may not correspond to a relevant location in your image.
- Call ``getTanWcs()``. This returns a FITS-compatible TAN WCS, but it may reference a point far from the image and differ significantly from the original WCS. It can appear superficially correct in some cases, making it more prone to causing subtle errors.

**With an lsst.afw.geom.SkyWcs object, you should:**

- Call ``pixelToSky[Array]`` to transform points from pixel coordinates (which may not start at (0, 0)!) to celestial coordinates.
- Call ``skyToPixel[Array]`` to transform points from celestial coordinates to pixel coordinates.
- Call ``linearizePixelToSky`` and ``linearizeSkyToPixel`` to obtain local affine approximations to the WCS at a given point.
- Call ``getPixelScale(point)`` with a specific position to evaluate the local pixel scale.

For more details on Rubin WCS pitfalls (especially outside the context of DP2), see `this community post <https://community.lsst.org/t/how-to-use-wcss-in-dp1-and-commissioning-processing/10769>`__.
We expect our single-visit WCS objects to continue to be non-representable as FITS.

Image units
===========

All image data products are annotated as having units of ``nJy``, but this does not mean individual pixel values can be treated as direct measures of spectral flux density (any more than any other image with an AB magnitude zero point - which would be directly proportional to ``nJy`` - can be used this way).

Instead, it simply means that photometry algorithms that account for the PSF model and aperture corrections (see below) directly yield ``nJy`` fluxes.

Aperture corrections
====================

Rubin processing uses aperture corrections to ensure that different photometry estimators produce consistent results on point sources.
These corrections are measured by applying different algorithms to the same set of bright stars on each single-visit image and interpolating the ratio of each algorithm to a standard one (a background-compensated top-hat aperture flux), which is then used for all photometric calibration.
All fluxes other than the standard algorithm's are then multiplied by the interpolated flux ratio.
Aperture corrections on coadds are computed by averaging the single-detector ratios with the same weights that were used to combine images.

This scheme has several problems:

- These aperture corrections are well-defined for point sources only, but we still apply them for most of our galaxy-focused photometry algorithms (the ``sersic_*`` fluxes are the sole exception), since this at least makes them well-calibrated for poorly-resolved galaxies.

- Coadding apertures with the same weights as the images is only correct in the limit that the images have the same PSF.
  For fixed-aperture photometry a different combination should be used (and will be used in future data releases, if we use this scheme at all), and for PSF-dependent photometry no formally correct combination is possible.

- Ratios of fluxes on even bright stars can be very noisy, and in some cases the aperture correction is a significant fraction of our error budget.

Improving our approach to aperture corrections is a research project; we are not happy with the current situation, but have not yet identified a satisfactory alternative.

Flag columns
============

The measurement algorithms that populate our catalogs typically have both a "general" failure flag that is set for any failure and one or more detailed flags that can be used to identify exactly what happened.
We also have a suite of "pixel flags" (``*_pixelFlags_*``) that report on the image mask planes in the neighborhood of a source or object.

Science users are expected to filter their samples using both types of flags explicitly; almost no filtering has already been performed, and filtering on flags can depend greatly on the science case.

Forced photometry variants and blending
=======================================

There are a total of four variants of forced photometry in Rubin data release processing for the combination of two different reference catalogs (:ref:`catalogs-object` and :ref:`catalogs-dia-object`) and two different measurement images (:ref:`images-visit-image` and :ref:`images-difference-image`).
Each of the two forced photometry catalogs includes rows for both measurement images and a single reference catalog (:ref:`catalogs-forced-source` uses :ref:`catalogs-object` positions, while :ref:`catalogs-dia-forced-source` uses :ref:`catalogs-dia-object` positions).

We expect forced photometry on :ref:`images-difference-image` to behave best, especially in crowded regions, since image subtraction should remove neighbors even more effectively than the deblender we run on the coadds (note that we do not deblend when producing the :ref:`catalogs-dia-source` catalog, either).
There is no deblending whatsoever in our forced photometry on :ref:`images-visit-image`.

Calibration source catalogs and flags
=====================================

The public :ref:`catalogs-source` catalog does not contain the same single-visit detections used to estimate the PSF and fit for the astrometric and photometric calibrations.
Those initial sources (the ``single_visit_star`` and ``recalibrated_star`` butler dataset types) are some of the many intermediate data products that are not retained in a final data release, while :ref:`catalogs-source` detections are made on the final :ref:`images-visit-image` after all calibration steps are complete.

Furthermore, the :ref:`catalogs-object` catalog has a few columns (prefixed with ``calib_``) that purport to identify objects used in various calibration steps.
These are generated from a spatial match from the object positions to the initial sources positions, which means they can suffer from mismatch problems in rare cases.

A more serious problem is that these flags currently reflect our preliminary single-detector astrometric and photometric calibration steps, not the later FGCM and GBDES fits (they do reflect the stars that went into our final Piff PSF models, however).
This problem will be fixed in future data releases.

Ellipse parameterizations and units
====================================

``TBD``

Instrumental signature removal
==============================

Amplifier offsets
-----------------

Through much of DP2 processing, there were amplifier offsets that propagated through to photometry and PSF residuals.
The "amp offset correction" code was enabled to mitigate this issue.
While this correction is not fully principled, it appears to work on average with no evidence of pathological behavior so far.

Camera sequencer changes
------------------------

Three different camera sequencer configurations were used during the DP2 observing period:

- **"2s_v30" sequencer** (April 2025 through July 2, 2025): 2+ second readout, with extra noise in some E2V sensors and most ITL amplifiers, and random gain jumps (~0.02% level) for some ITL amplifiers. Basic calibrations were derived from clean-room data.

- **"3s_v1" sequencer** (July 2, 2025 through January 23, 2026): 3 second readout, with significantly reduced noise in both E2V and ITL amplifiers, though some unexpected even/odd pixel correlations in E2V noise remained. Random gain jumps (~0.02% level) for some ITL amplifiers persisted.

- **"3s_v2" sequencer** (from January 23, 2026): not included in DP2.

These changes result in three calibration epochs (2025-04-24 to 2025-07-02, 2025-07-03 to 2025-09-21, and 2025-10-24 to 2026-01-06) with different noise characteristics. Flat fields improved with each successive epoch, and the y-band flats are notably better for the third epoch due to the installation of a new 1050 nm LED.

Flat fielding
-------------

The flat-field screen is not uniformly illuminated, nor does it uniformly fill the focal plane.
For each filter except u, two "anaglyph" flats are taken (one with a bluer LED and one with a redder LED), with special gradient-removal code to correct for gradients and centering.
This correction does not work well in the outer partly vignetted region of the focal plane.

Camera temperature variations (particularly during the second calibration epoch) and focal plane restarts also changed gains slightly, resulting in additional amplifier offsets.

Photometric calibration: background oversubtraction
====================================================

Photometrically-derived background offset QA plots reveal systematic oversubtraction in all bands, worsening toward redder bands and in the densest regions.
DP2 includes many dense regions, making this effect more pronounced.
This oversubtraction can affect photometry and PSF estimation in crowded fields.

Astrometric calibration
========================

The final astrometry in DP2 is performed in two steps: GBDES fits static and per-visit polynomials, and remaining atmospheric turbulence is fit by Gaussian Processes.
In a separate step, proper motion and parallax are now fit for all isolated stars.
Single-frame astrometry uses `astrometry_camera`, the camera distortion model built from the astrometry fit, though pointing accuracy remains an issue.

Known astrometric issues include:

- Not yet understood behavior in the AA1 (per-visit median offset from Gaia) metrics.
- Stacking residuals in focal plane coordinates reveals unmodeled camera behavior, including tree ring signatures and other small-scale effects.
- The GBDES fit currently consumes a large amount of memory and is a processing bottleneck.

PSF modeling issues in crowded fields
======================================

PSF residuals in DP2 show structure that correlates with stellar density, with the largest biases appearing in the Milky Way, the LMC, and the SMC.
This stellar-density dependence also explains observed differences in PSF residuals between bands (bands with more crowding show larger residuals).
The underlying cause is believed to be related to background subtraction in dense fields.

DP2 has an order of magnitude more visits than previous processing campaigns, revealing a new variety of PSF-related issues, including annealing patterns in E2V sensors visible in u and g-band stacks.

Cell-based coadds
==================

The deep coadds used to build the Object table are now built with cell-level input selection: a ``{visit, detector}`` must fully overlap a cell to be included in that cell.
These "edge-free" cell coadds eliminate detector-edge discontinuities, but there are discontinuities between cells on the coadd, and these may be more pronounced than the old discontinuities at detector edges.
Cells are 150x150 pixels, and the PSF model for each cell is the weighted average of the PSFs of the input images, assumed constant over the cell.

Cell coadd injection is not yet working for DP2, so characterization relies on matching against external catalogs in deep drilling fields. A large injection campaign is planned for the DP2 release.

Deblending quality
===================

A population of faint, semi-isolated sources causes catastrophic deblending failures, which appear to exist in every patch.
A sizeable number of outliers (approximately the bottom 2% for most patches) need investigation.
Resolved grand-design spirals and other non-monotonic structures are also expected to have poor deblending chi-squared values.

Users should be cautious with objects that have reduced deblending chi-squared values much greater than 3.

Coadd photometry in crowded fields
====================================

Stellar photometry on coadds is biased bright in crowded fields.
This is likely due to undersubtracted backgrounds, though the diagnosis is not yet complete.

Coadd astrometry: galaxy RA bias
==================================

Galaxy Right Ascension (but not Declination) has a magnitude-dependent bias, seen when compared to external catalogs (Euclid, DES, and others) and confirmed in injection runs.
This may be related to the way the aperture used in ``SdssCentroid`` grows with source brightness.
Exponential and Sersic model centroids, which have a free centroid parameter, are generally more accurate and have a smaller bias.

Difference imaging
===================

Difference imaging in DP2 benefits from new masking features, including a ``SPIKE`` mask plane for diffraction spikes and a ``HIGH_VARIANCE`` mask plane in template detectors.

Remaining known issues in difference imaging include:

- Deblending on the difference image is not yet implemented.
- Detection and measurement with preconvolution needs further development.
- The reliability model for DIA sources shows improved performance relative to DP1, but does not perform well on bad stellar subtractions.

Background subtraction
=======================

The ``SkyCorrectionTask`` that performs full-focal-plane background correction is not designed to accommodate detector-to-detector offsets in the input data.
These offsets arise from several sources, including E2V/ITL sensor differences, offset detectors, and offset REBs, and are more pronounced in the u and y bands.
A new full-focal-plane background fitter is under development but was not ready for DP2.

Star/galaxy classification
===========================

A new floating-point, per-band and griz ``model_extendedness`` classifier using Sersic fluxes and sizes generally performs better than the previous ``refExtendedness``.
However, ``griz_model_extendedness`` tends to classify everything as a galaxy fainter than approximately ``i = 24``; adjusting the threshold can help, but optimal faint-star selection will require user input.
A classification for bad or bogus detections is still lacking.

Solar system processing
========================

DP2 includes solar system discovery and association results.
Association uses 1 arcsecond matching without uncertainties, yielding approximately 4 million associations.
Astrometry for solar system objects shows a 30 mas bias compared to orbit catalogs, with a dependence on asteroid properties but not on observation parameters.

Three-night discovery candidates are not sufficiently pure (approximately 1 in 500), while four-night candidates achieve much higher purity (approximately 1 in 10,000).
