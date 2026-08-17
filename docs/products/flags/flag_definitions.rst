.. _flag-definitions:

###############################
Flag definitions and categories
###############################

To help users interpret flag meanings, the sections below organize the most scientifically useful flags into categories based on what each flag indicates.
See also the :doc:`/products/flags/flag_recommendations` page for guidance on which flags to apply for science-quality selections.

.. note::

   Which flags exist, and whether a flag is meaningful, depends on the table.
   The same measurement flag can have a different name (or not exist) in another table, and some flags that were useful in DP1 are **deprecated** or removed in DP2.
   Deprecated cases are called out explicitly below.


Pixel quality flags
===================

Pattern: ``{band}_pixelFlags_*`` (Object) or ``pixelFlags_*`` (Source-level tables).

Purpose: report on the mask-plane status of the pixels in a source's footprint, derived from the image :doc:`mask planes </products/flags/mask_planes>`.
Flags without a ``Center`` suffix are set if *any* pixel in the footprint carries the corresponding mask bit; ``Center`` variants are set only if a pixel in the central 3×3 box carries it.
Center flags are generally the more important for photometry and shapes because they affect the core of the source.

.. list-table::
   :header-rows: 1
   :widths: 30 30 40

   * - Pixel quality flag
     - Tables
     - Meaning when set to 1
   * - ``pixelFlags_saturated`` / ``…saturatedCenter``
     - Object, Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - Saturated pixels in the footprint (or center). On coadds, saturated pixels are rejected but would otherwise have contributed appreciably.
   * - ``pixelFlags_cr`` / ``…crCenter``
     - Object, Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - A cosmic ray was detected and interpolated over in the footprint (or center).
   * - ``pixelFlags_interpolated`` / ``…interpolatedCenter``
     - Object, Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - An interpolated pixel (from cosmic rays, defects, or saturation) contributed in the footprint (or center).
   * - ``pixelFlags_sensor_edge`` / ``…sensor_edgeCenter``
     - Object
     - A detector boundary from an input visit passed through the footprint (or near the center). This is the edge indicator to use on coadds.
   * - ``pixelFlags_clipped`` / ``…clippedCenter``
     - Object
     - Input pixels were rejected by warp comparison (artifact clipping) during coaddition in the footprint (or center).
   * - ``pixelFlags_inexact_psf`` / ``…inexact_psfCenter``
     - Object
     - The coadd PSF model is discontinuous in the footprint (or near the center), typically at cell/patch boundaries or where input artifacts were rejected.
   * - ``pixelFlags_nodata`` / ``…nodataCenter``
     - Object, Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - No pixel data were available (outside the usable coverage region). ``nodataCenter`` is available in DiaSource.
   * - ``pixelFlags_edge``
     - Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - Source is on the edge of the usable exposure region (single-epoch/difference images).
   * - ``pixelFlags_bad``, ``pixelFlags_suspect`` / ``…suspectCenter``
     - Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - Known bad (detector defect) or suspect (near-saturation, non-linear) pixels in the footprint (or center).
   * - ``pixelFlags_streak`` / ``…streakCenter``
     - DiaSource
     - A masked streak (e.g. satellite trail) overlaps the footprint (or center).
   * - ``pixelFlags_injected`` / ``…injectedCenter``, ``pixelFlags_injected_template`` / ``…injected_templateCenter``
     - DiaSource
     - Synthetic-source injection overlaps the footprint (or center) in the science image or template. Relevant only for injection test datasets.

.. warning::

   **Deprecated in the DP2 Object table.**
   In the coadd Object table the columns ``pixelFlags_bad``, ``pixelFlags_edge``, ``pixelFlags_suspect``, ``pixelFlags_suspectCenter``, and ``pixelFlags_offimage`` are deprecated: they are only set in the (rare) case of missing band data and should **not** be used as quality cuts.
   Use ``pixelFlags_sensor_edge``/``sensor_edgeCenter`` for coadd edges. These flags remain valid in the single-epoch and difference-image tables (Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject).


Measurement failure flags
=========================

Pattern: ``{band}_{algorithm}_flag`` (Object) or ``{algorithm}_flag`` (Source-level tables).

Purpose: indicate that a particular measurement algorithm failed or produced unreliable results.
The general rule is simple: **if you use a measured quantity, require its general failure flag to be 0.** For example, when using ``r_psfFlux``, require ``r_psfFlux_flag = 0``.

Most algorithms provide both a *general* failure flag (set for any failure) and one or more detailed *subflags* that explain what went wrong (e.g. ``psfFlux_flag_edge``, ``psfFlux_flag_noGoodPixels``).
The general flag alone is sufficient for filtering; the subflags are diagnostic.

.. list-table::
   :header-rows: 1
   :widths: 30 30 40

   * - Measurement flag
     - Tables
     - Meaning when set to 1
   * - ``{band}_psfFlux_flag``
     - Object, Source, ForcedSource, ForcedSourceOnDiaObject
     - PSF flux measurement failed; do not use the PSF flux.
   * - ``{band}_cModel_flag``
     - Object
     - CModel (galaxy model) fit failed; do not use the model fluxes.
   * - ``{band}_kronFlux_flag``
     - Object
     - Kron aperture flux failed (e.g. bad radius, near edge).
   * - ``{band}_gaapFlux_flag``
     - Object
     - GAaP (Gaussian Aperture and PSF) photometry failed.
   * - ``{band}_apNNFlux_flag``
     - Object, Source
     - Aperture flux in the ``NN``-pixel aperture failed (e.g. ``ap12Flux_flag``).
   * - ``centroid_flag``
     - Source, DiaSource
     - Centroid algorithm failed; do not trust the position.
   * - ``coord_flag``
     - Object
     - General reference-band centroid/coordinate failure.
   * - ``shape_flag``
     - Object, DiaSource
     - Shape (second-moments) measurement failed.
   * - ``{band}_extendedness_flag``
     - Object
     - Flux-ratio star/galaxy classifier failed; ``extendedness`` unreliable.
   * - ``{band}_sizeExtendedness_flag``
     - Object
     - Moments-based star/galaxy classifier failed.
   * - ``{band}_hsmShapeRegauss_flag``
     - Object
     - HSM Regaussianization shape measurement failed.
   * - ``{band}_blendedness_flag``
     - Object, Source
     - Blendedness measurement failed.

.. note::

   **Free versus forced measurements.**
   In the Object table most fluxes (e.g. ``{band}_psfFlux``, ``{band}_cModel_*``) are **forced**: they are measured at the reference-band position and shape so that colors are consistent across bands.
   DP2 also provides **free** (unforced) variants — ``{band}_free_psfFlux`` / ``{band}_free_psfFlux_flag`` and ``{band}_free_cModelFlux`` / ``{band}_free_cModelFlux_flag`` — which are measured independently in each band.
   When filtering, apply the flag that matches the flux you use: use ``{band}_psfFlux_flag`` with the forced flux and ``{band}_free_psfFlux_flag`` with the free flux.

.. note::

   New model-fit flags in DP2 include the multi-band ``exponential_*`` and ``sersic_*`` model flags (``…_no_data_flag``, ``…_unknown_flag``), the higher-order ``{band}_moments_flag`` / ``moments_psf_flag`` / ``moments_psf_debiased_flag`` shape flags, and the ``{band}_psfModel_TwoGaussian_*`` PSF-model flags.
   These support new DP2 measurements (multi-band morphology and improved shape/PSF modeling); require the relevant general flag to be 0 if you use the associated quantity.


DIA flags
=========

Purpose: indicate particular issues with difference image analysis (DIA), i.e. transient/variable detections on difference images (DiaSource).

.. important::

   The DP2 DIA pipeline errs toward completeness rather than purity, and **no real/bogus reliability cut was applied** before writing the DiaSource catalog.
   Users who need a higher-purity transient sample should apply a minimum threshold on the DiaSource ``reliability`` column (the machine-learned real/bogus score) themselves.

.. list-table::
   :header-rows: 1
   :widths: 30 15 55

   * - DIA flag
     - Tables
     - Meaning when set to 1
   * - ``isDipole``
     - DiaSource
     - Detection is well fit by a dipole model, i.e. a subtraction artifact (typically at bright stars). Exclude for clean transient samples.
   * - ``isNegative``
     - DiaSource
     - Source was detected as significantly negative (a flux decrease). New in DP2; keep or exclude depending on whether the science targets fading/disappearing sources.
   * - ``glint_trail``
     - DiaSource
     - Source is part of a "glint trail" (a line of detections likely from rotating orbital debris). Flagged, not removed.
   * - ``dipoleFitAttempted``
     - DiaSource
     - A dipole model was fit to this source (informational, not a quality reject).
   * - ``trail_flag_edge``
     - DiaSource
     - A trailed source extends onto or past edge pixels.
   * - ``psfFlux_flag``
     - DiaSource
     - PSF flux on the difference image failed. Require 0 to use the difference flux.
   * - ``forced_PsfFlux_flag``
     - DiaSource
     - Forced PSF photometry on the science (direct) image failed.
   * - ``psfDiffFlux_flag``
     - ForcedSource, ForcedSourceOnDiaObject
     - Forced PSF flux on the difference image failed.
   * - ``diff_PixelFlags_nodataCenter``
     - ForcedSource, ForcedSourceOnDiaObject
     - Forced position falls outside difference-image coverage (no template); the difference flux is invalid.

.. note::

   The DiaSource table also carries a general ``pixelFlags`` column: when set, the mask-plane bookkeeping for that footprint failed and *other* ``pixelFlags_*`` for that source may be incorrectly reported as ``False``.


Special flags
=============

Additional notable flags that provide ancillary information about sources and objects.

.. list-table::
   :header-rows: 1
   :widths: 30 30 40

   * - Flag name
     - Tables
     - Meaning when set to 1
   * - ``detect_isPrimary``
     - Object
     - Not a failure flag: it marks the single primary version of a detection (deblended, inner-patch, inner-tract). Require ``detect_isPrimary = 1`` to avoid duplicate and pre-deblend rows.
   * - ``{band}_invalidPsfFlag``
     - Object
     - The PSF model is invalid (no usable inputs); measurements are unreliable. Exclude these objects.
   * - ``invalidPsfFlag``
     - Source, ForcedSource, ForcedSourceOnDiaObject
     - As above, for the single-epoch and forced tables.
   * - ``{band}_inputCount_flag``
     - Object
     - Failed to compute the number of coadd input exposures.

.. note::

   **Weak-lensing shear (new ShearObject table).**
   DP2 adds a ShearObject table produced by multi-band metadetection, which carries its own suite of flags (e.g. ``gauss_flags`` / ``pgauss_flags`` and their ``…_object_flags``/``…_shape_flags`` variants, ``bmask_flags``, ``ormask_flags``, ``image_flags``, ``psfOriginal_flags``, and the ``is_*_inner`` / ``is_primary`` selection flags).
   Detailed shear-flag guidance is still being validated; consult the `schema browser <https://sdm-schemas.lsst.io/dp2.html>`_ for current recommendations, and note that no deblending is performed prior to these measurements.


.. _calibration-flags:

Calibration flags
=================

Pattern: ``calib_*`` (Source) or ``{band}_calib_*`` (Object).

Purpose: these flags indicate whether a source was used in astrometric calibration, photometric calibration, or PSF modeling during single-visit processing.

**For most science applications, these flags can be ignored, as they pertain to internal use in the calibration process.**

The public Source catalog does not contain the same single-visit detections used to estimate the PSF and fit the astrometric and photometric calibrations.
Those initial sources (the ``single_visit_star`` and ``recalibrated_star`` butler dataset types) are intermediate products that are not retained in a final data release, while Source detections are made on the final visit image after all calibration steps are complete.

The ``{band}_calib_*`` columns in the Object table are propagated from the single-visit sources by a spatial match, and so can suffer from mismatch problems in rare cases.
Note also that these flags currently reflect the preliminary single-detector astrometric and photometric calibration steps, not the later FGCM and GBDES fits (they do reflect the stars that went into the final Piff PSF models).
This is expected to be improved in future data releases.

.. list-table::
   :header-rows: 1
   :widths: 35 15 50

   * - Calibration flag
     - Tables
     - Meaning when set to 1
   * - ``calib_astrometry_used``
     - Source, Object
     - Source was used in the astrometric (WCS) solution.
   * - ``calib_photometry_used``
     - Source, Object
     - Source was used in the photometric zeropoint determination.
   * - ``calib_photometry_reserved``
     - Source
     - Source was reserved (held out) from photometric calibration for validation.
   * - ``calib_psf_used``
     - Source, Object
     - Source was used for PSF modeling.
   * - ``calib_psf_reserved``
     - Source, Object
     - Source was reserved (held out) from PSF determination.
   * - ``calib_psf_candidate``
     - Source, Object
     - Source was a candidate for PSF-star selection.

.. note::

   ``calib_photometry_reserved`` is available in the Source table in DP2 but is no longer carried on the Object table.
