.. _products_known_issues:

############
Known issues
############

.. important::

   This webpage contains some placeholder information from Data Preview 1 and is currently under development.


The purpose of this page is to document and provide guidance on known issues with the dataset.


.. _issues_crowded_fields:

Crowded fields
==============

Deblending quality
------------------

Poor deconvolution in crowded fields leaves rings that connect large regions, resulting in blends that take up most or all of a patch.
There are missing measurements for several patches in dense stellar fields.


PSF modeling issues in crowded fields
-------------------------------------

PSF residuals in DP2 show structure that correlates with stellar density, with the largest biases appearing in the Milky Way, the LMC, and the SMC.
This stellar-density dependence also explains observed differences in PSF residuals between bands (bands with more crowding show larger residuals).
The underlying cause is believed to be related to background subtraction in dense fields.

DP2 has an order of magnitude more visits than previous processing campaigns, revealing a new variety of PSF-related issues, including annealing patterns in E2V sensors visible in u and g-band stacks.

.. _issues_astrometry:

Astrometry
==========

.. _products_wcs_known_issues:

WCS FITS approximations and misleading interfaces
--------------------------------------------------

.. important::

   This webpage contains some placeholder information from Data Preview 1. WCS issues have been fixed and will not persist to DP2.


Rubin's single-visit World Coordinate System (WCS) objects are not, in general, exactly representable via the FITS WCS standard.
The FITS WCS in the headers of the visit image and difference image data products (they are the same) are ``TAN-SIP`` approximations that were expected to be good enough for visualization and object finding, not for precision astrometry.

Current recommendations for WCS use
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note::
  TODO: Update this section. New safer types (lsst.images) should be what most users see,
  but it will still be possible to access the old lsst.afw.geom.SkyWcs types.

To make use of the full WCS of a visit image or difference image, it is necessary to use Rubin's ``lsst.afw.geom.SkyWcs`` objects::

  wcs = visit_image.wcs
  x, y = wcs.skyToPixelArray(ra, dec, degrees=True)  # or False for radians
  ra, dec = wcs.pixelToSkyArray(x, y, degrees=True)

where ``x``, ``y``, ``ra``, and ``dec`` are double-precision (``dtype=np.float64``) NumPy arrays.

Unfortunately, these ``lsst.afw.geom.SkyWcs`` objects are not included in our Python API documentation reference at present, and they are counterintuitive and easy to misuse in several ways, which can make it appear as if the WCS fits are terrible.
These problems center around the fact that their ``getSkyOrigin`` and ``getPixelOrigin`` methods generally return a point that is far off the image the WCS corresponds to (where the mapping can extrapolate poorly), and other methods like ``getPixelScale`` will by default evaluate at this often-irrelevant point.
The ``skyToPixel[Array]`` and ``pixelToSky[Array]`` are always safe to use, and ``getPixelScale`` can be used safely if you explicitly provide it with the point to evaluate the pixel scale at.
Most other methods are best avoided (including ``getFitsMetadata``, which does *not* return the FITS approximation in the file headers).

The WCS objects attached to coadds *are* exactly representable in FITS WCS, but can also be tricky to use since each full ``tract`` has a single WCS, and each patch image has an integer offset relative to that coordinate system.
So if you're indexing the NumPy array that backs a coadd image directly, e.g.::

  coadd.image.array[i, j]

be aware that this corresponds to the ``x`` and ``y`` pixel coordinates of the WCS via::

  bbox = coadd.getBBox()
  i = y - bbox.y.min
  j = x - bbox.x.min

When we write a coadd image to a file, we offset the WCS in the FITS header to be consistent with the pixels (since FITS requires that the origin of any image be ``(1, 1)``).
But this is not done when calling ``wcs.getFitsMetadata()``, since the necessary offset is stored with the coadd object, not the WCS, and this is another common source of confusion.

Finally, the WCS objects of raw images simply should not be used; they are based on an initial guess from the telescope at its pointing, but at present this can be quite far off from the true pointing.
The corresponding visit image WCS should be used instead.

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

We expect our single-visit WCS objects to continue to be non-representable as FITS.

Astrometric calibration
-----------------------

The final astrometry in DP2 is performed in two steps: GBDES fits static and per-visit polynomials, and remaining atmospheric turbulence is fit by Gaussian Processes.
In a separate step, proper motion and parallax are now fit for all isolated stars.
Single-frame astrometry uses ``astrometry_camera``, the camera distortion model built from the astrometry fit, though pointing accuracy remains an issue.

Known astrometric issues include:

- Not yet understood behavior in the per-visit median offset from Gaia metrics.
- Stacking residuals in focal plane coordinates reveals unmodeled camera behavior, including tree ring signatures and other small-scale effects.

Coadd astrometry: galaxy RA bias
--------------------------------

Galaxy Right Ascension (but not Declination) has a magnitude-dependent bias, seen when compared to external catalogs (Euclid, DES, and others) and confirmed in injection runs.
This may be related to the way the aperture used in ``SdssCentroid`` grows with source brightness.
Exponential and Sersic model centroids, which have a free centroid parameter, are generally more accurate and have a smaller bias.

.. _issues_flags:

Flags and catalogs
==================

.. note::
  TODO: Move flags and catalogs content to the appropriate catalog data product pages.

Flag columns
------------

The measurement algorithms that populate our catalogs typically have both a "general" failure flag that is set for any failure and one or more detailed flags that can be used to identify exactly what happened.
We also have a suite of "pixel flags" (``*_pixelFlags_*``) that report on the image mask planes in the neighborhood of a source or object.

Science users are expected to filter their samples using both types of flags explicitly; almost no filtering has already been performed, and filtering on flags can depend greatly on the science case.

Calibration source catalogs and flags
--------------------------------------

The public Source catalog does not contain the same single-visit detections used to estimate the PSF and fit for the astrometric and photometric calibrations.
Those initial sources (the ``single_visit_star`` and ``recalibrated_star`` butler dataset types) are some of the many intermediate data products that are not retained in a final data release, while Source detections are made on the final visit image after all calibration steps are complete.

Furthermore, the Object catalog has a few columns (prefixed with ``calib_``) that purport to identify objects used in various calibration steps.
These are generated from a spatial match from the object positions to the initial sources positions, which means they can suffer from mismatch problems in rare cases.

A more serious problem is that these flags currently reflect our preliminary single-detector astrometric and photometric calibration steps, not the later FGCM and GBDES fits (they do reflect the stars that went into our final Piff PSF models, however).
This problem will be fixed in future data releases.

Star/galaxy classification
---------------------------

A new floating-point, per-band and griz ``model_extendedness`` classifier using Sersic fluxes and sizes generally performs better than the previous ``refExtendedness``.
However, ``griz_model_extendedness`` tends to classify everything as a galaxy fainter than approximately ``i = 24``; adjusting the threshold can help, but optimal faint-star selection will require user input.
A classification for bad or bogus detections is still lacking.

.. _issues_photometry:

Photometry
==========

Image units
-----------

.. note::
  TODO: Move this section to the image data product pages.

All image data products are annotated as having units of ``nJy``, but this does not mean individual pixel values can be treated as direct measures of spectral flux density (any more than any other image with an AB magnitude zero point - which would be directly proportional to ``nJy`` - can be used this way).

Instead, it simply means that photometry algorithms that account for the PSF model and aperture corrections (see below) directly yield ``nJy`` fluxes.

Aperture corrections
--------------------

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

Photometric calibration: background oversubtraction
----------------------------------------------------

Photometrically-derived background offset QA plots reveal systematic oversubtraction in all bands, worsening toward redder bands and in the densest regions.
DP2 includes many dense regions, making this effect more pronounced.
This oversubtraction can affect photometry and PSF estimation in crowded fields.

Forced photometry variants and blending
----------------------------------------

There are a total of four variants of forced photometry in Rubin data release processing for the combination of two different reference catalogs (Object and DiaObject) and two different measurement images (visit images and difference images).
Each of the two forced photometry catalogs includes rows for both measurement images and a single reference catalog (ForcedSource uses Object positions, while DiaForcedSource uses DiaObject positions).

We expect forced photometry on difference images to behave best, especially in crowded regions, since image subtraction should remove neighbors even more effectively than the deblender we run on the coadds (note that we do not deblend when producing the DiaSource catalog, either).
There is no deblending whatsoever in our forced photometry on visit images.

Ellipse parameterizations and units
-------------------------------------

``TBD``

.. _issues_backgrounds:

Background subtraction
=======================

The ``SkyCorrectionTask`` that performs full-focal-plane background correction is not designed to accommodate detector-to-detector offsets in the input data.
These offsets arise from several sources, including E2V/ITL sensor differences, offset detectors, and offset REBs, and are more pronounced in the u and y bands.
A new full-focal-plane background fitter is under development but was not ready for DP2.

.. _issues_coadds:

Cell discontinuities in coadds
===============================

The deep coadds used to build the Object table are now built with cell-level input selection: a ``{visit, detector}`` must fully overlap a cell to be included in that cell.
These "edge-free" cell coadds eliminate detector-edge discontinuities, but there are discontinuities between cells on the coadd, and these may be more pronounced than the old discontinuities at detector edges.
Cells are 150x150 pixels, and the PSF model for each cell is the weighted average of the PSFs of the input images, assumed constant over the cell.

Cell coadd injection is not yet working for DP2, so characterization relies on matching against external catalogs in deep drilling fields. A large injection campaign is planned for the DP2 release.

.. _issues_dia:

Difference imaging
===================

Difference imaging in DP2 benefits from new masking features, including a ``SPIKE`` mask plane for diffraction spikes and a ``HIGH_VARIANCE`` mask plane in template detectors.

Remaining known issues in difference imaging include:

- Deblending on the difference image is not yet implemented.
- Detection and measurement with preconvolution needs further development.
- The reliability model for DIA sources shows improved performance relative to DP1, but does not perform well on bad stellar subtractions.

.. _issues_solarsystem:

Solar system processing
========================

DP2 includes solar system discovery and association results.
Association uses 1 arcsecond matching without uncertainties, yielding approximately 4 million associations.
Astrometry for solar system objects shows a 30 mas bias compared to orbit catalogs, with a dependence on asteroid properties but not on observation parameters.

Three-night discovery candidates are not sufficiently pure (approximately 1 in 500), while four-night candidates achieve much higher purity (approximately 1 in 10,000).

.. _issues_forcedsource:

Missing ForcedSources
=====================

Approximately 3% of visit images in the coadded area that were processed successfully and included in coadd construction were not measured during force photometry and did not generate ForcedSources.
The affected images had failed image differencing, and because force photometry is normally performed on both the visit image and its corresponding difference image by the same task, the pipeline skipped both types of force photometry since the required inputs were not all available.
In some areas where very few visits were obtained, this means there may be Objects that have no corresponding ForcedSources at all.
In future data releases this task will be corrected to perform visit image force photometry regardless of the difference image status.

