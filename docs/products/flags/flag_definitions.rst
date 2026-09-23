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
   :widths: 25 15 60

   * - Flag name
     - Tables
     - Meaning when set to 1
   * - ``pixelFlags_saturated``
     - Object, Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - Saturated pixels in footprint; photometry unreliable.
   * - ``pixelFlags_saturatedCenter``
     - Object, Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - Saturated pixel in central 3x3 footprint; critical quality issue.
   * - ``pixelFlags_cr``
     - Object, Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - Cosmic ray detected and interpolated in footprint.
   * - ``pixelFlags_crCenter``
     - Object, Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - Cosmic ray at center.
   * - ``pixelFlags_interpolated``
     - Object, Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - Interpolated pixels in footprint (from CRs, defects, saturation).
   * - ``pixelFlags_interpolatedCenter``
     - Object, Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - Interpolated pixel at center; affects core photometry and shapes.
   * - ``pixelFlags_sensor_edge``
     - Object
     - Detector boundary crossed footprint.
   * - ``pixelFlags_sensor_edgeCenter``
     - Object
     - Detector edge near center; important for coadds.
   * - ``pixelFlags_clipped``
     - Object
     - Artifact rejection during coaddition excluded input pixels.
   * - ``pixelFlags_clippedCenter``
     - Object
     - Clipping occurred at center.
   * - ``pixelFlags_inexact_psf``
     - Object
     - Coadd PSF model is discontinuous in footprint, typically at cell or patch boundaries or where input artifacts were rejected.
   * - ``pixelFlags_inexact_psfCenter``
     - Object
     - Coadd PSF model is discontinuous at center. Covers a large area; not recommended as a general cut.
   * - ``pixelFlags_nodata``
     - Object, Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject
     - No pixel data available (off coverage area).
   * - ``pixelFlags_nodataCenter``
     - DiaSource
     - No pixel data available at center.
   * - ``pixelFlags_streak``
     - DiaSource
     - Masked streak (e.g., satellite trail) overlaps footprint.
   * - ``pixelFlags_streakCenter``
     - DiaSource
     - Masked streak overlaps center.
   * - ``pixelFlags_injected``
     - DiaSource
     - Synthetic-source injection overlaps footprint in the science image. Relevant only for injection test datasets.
   * - ``pixelFlags_injectedCenter``
     - DiaSource
     - Synthetic-source injection overlaps center in the science image.
   * - ``pixelFlags_injected_template``
     - DiaSource
     - Synthetic-source injection overlaps footprint in the template image.
   * - ``pixelFlags_injected_templateCenter``
     - DiaSource
     - Synthetic-source injection overlaps center in the template image.

Key points:

- Center variants: Flags with a ``Center`` suffix indicate the issue affects the object's central footprint (typically a 3x3 pixel box), which is more critical for photometry and shapes than flags affecting only the outer footprint.
- Coadd-specific flags: On Object table coadds, use ``pixelFlags_sensor_edge`` and ``pixelFlags_sensor_edgeCenter`` as the edge indicator, since they record where detector boundaries from input visits crossed the object.

.. warning::

   **Flags omitted from the table above because they are deprecated in the DP2 Object table.**
   ``pixelFlags_bad`` (known bad pixels, i.e. detector defects, in footprint), ``pixelFlags_edge`` (source on the edge of the usable exposure region), ``pixelFlags_suspect`` and ``pixelFlags_suspectCenter`` (suspect pixels near saturation or with non-linear response), and ``pixelFlags_offimage`` are deprecated in the coadd Object table: they are only set in the (rare) case of missing band data, and should **not** be used as quality cuts there.
   Use ``pixelFlags_sensor_edge`` / ``pixelFlags_sensor_edgeCenter`` for coadd edges instead.
   These flags remain valid and useful in the single-epoch and difference-image tables (Source, ForcedSource, DiaSource, ForcedSourceOnDiaObject).


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
     - Moments-based star/galaxy classifier failed. Note that the third classifier, ``{band}_model_extendedness`` (and ``griz_model_extendedness``), has **no** corresponding failure flag; see :ref:`flags-object`.
   * - ``{band}_hsmShapeRegauss_flag``
     - Object
     - HSM Regaussianization shape measurement failed.
   * - ``{band}_blendedness_flag``
     - Object, Source
     - Blendedness measurement failed.
   * - ``sersic_no_data_flag``
     - Object
     - Multi-band Sersic model fit had no data. New in DP2.
   * - ``sersic_unknown_flag``
     - Object
     - Multi-band Sersic model fit failed for an unspecified reason. New in DP2.
   * - ``exponential_no_data_flag``
     - Object
     - Multi-band exponential model fit had no data. New in DP2.
   * - ``exponential_unknown_flag``
     - Object
     - Multi-band exponential model fit failed for an unspecified reason. New in DP2.
   * - ``{band}_moments_flag``
     - Object
     - Higher-order moments measurement failed. New in DP2.
   * - ``{band}_moments_psf_flag``
     - Object
     - Higher-order moments of the PSF model failed. New in DP2.
   * - ``{band}_moments_psf_debiased_flag``
     - Object
     - Debiased PSF moments measurement failed. New in DP2.
   * - ``{band}_psfModel_TwoGaussian_unknown_flag``
     - Object
     - Two-Gaussian PSF model fit failed for an unspecified reason. New in DP2.
   * - ``{band}_psfModel_TwoGaussian_no_inputs_flag``
     - Object
     - Two-Gaussian PSF model fit had no inputs. New in DP2.

.. note::

   **Free versus forced measurements.**
   In the Object table most fluxes (e.g. ``{band}_psfFlux``, ``{band}_cModel_*``) are **forced**: they are measured at the reference-band position and shape so that colors are consistent across bands.
   DP2 also provides **free** (unforced) variants — ``{band}_free_psfFlux`` / ``{band}_free_psfFlux_flag`` and ``{band}_free_cModelFlux`` / ``{band}_free_cModelFlux_flag`` — which are measured independently in each band.
   When filtering, apply the flag that matches the flux you use: use ``{band}_psfFlux_flag`` with the forced flux and ``{band}_free_psfFlux_flag`` with the free flux.


Difference image analysis (DIA) flags
=====================================

Purpose: indicate particular issues with difference image analysis (DIA), i.e. transient/variable detections on difference images (DiaSource).

.. important::

   **No real/bogus reliability cut was applied** before writing the DiaSource catalog.
   Users who need a higher-purity transient sample should apply a minimum threshold on the DiaSource ``reliability`` column (the machine-learned real/bogus score) themselves.

.. list-table::
   :header-rows: 1
   :widths: 30 15 55

   * - Flag name
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
