.. _flag-definitions:

###############################
Flag definitions and categories
###############################

To help users interpret flag meanings, the sections below organize flags into categories based on what each flag indicates.

.. note::

   Which flags exist and whether they are meaningfully populated changes between tables and data releases.
   Some DP1-era pixel quality flags are deprecated or removed in DP2, and these cases are listed below.


**\*Table legend:**  O = Object; S = Source; FS = ForcedSource; DS = DiaSource; DFS = ForcedSourceOnDiaObject


Pixel quality flags
===================

**Pattern:**
``{band}_pixelFlags_*`` (Object table) or ``pixelFlags_*`` (Source tables).

**Purpose:**
To report on the mask-plane status of the pixels in a source's footprint, derived from the image :doc:`/products/images/mask_planes` as explained in :doc:`/products/flags/mask_planes`.

**Key points:**
Flags without a ``Center`` suffix are set if *any* pixel in the footprint carries the corresponding mask bit; ``Center`` variants are set only if a pixel in the central 3×3 box carries it.
Flags with a ``Center`` suffix indicate the issue affects the object's central footprint (typically a 3x3 pixel box), which is more critical for photometry and shapes than flags affecting only the outer footprint.

**Deprecated flags:**
For DP2, the Object table flags ``pixelFlags_sensor_edge`` and ``pixelFlags_sensor_edgeCenter`` still exist but they are deprecated and always set to false because the deep coadd images are :ref:`images-new-cell-based` which are not affected by the sensor edges of the input visit images.
Other deprecated flags include: ``pixelFlags_bad``, ``pixelFlags_edge``, ``pixelFlags_suspect``, ``pixelFlags_suspectCenter``, and ``pixelFlags_offimage``.

.. list-table::
   :header-rows: 1
   :widths: 25 60

   * - Flag name and tables\*
     - Meaning when set to 1
   * - | ``pixelFlags_saturated``
       | O, S, FS, DS, DFS
     - Saturated pixels in footprint; photometry unreliable.
   * - | ``pixelFlags_saturatedCenter``
       | O, S, FS, DS, DFS
     - Saturated pixel in central 3x3 footprint; critical quality issue.
   * - | ``pixelFlags_cr``
       | O, S, FS, DS, DFS
     - Cosmic ray detected and interpolated in footprint.
   * - | ``pixelFlags_crCenter``
       | O, S, FS, DS, DFS
     - Cosmic ray at center.
   * - | ``pixelFlags_interpolated``
       | O, S, FS, DS, DFS
     - Interpolated pixels in footprint (from CRs, defects, saturation).
   * - | ``pixelFlags_interpolatedCenter``
       | O, S, FS, DS, DFS
     - Interpolated pixel at center; affects core photometry and shapes.
   * - | ``pixelFlags_clipped``
       | O
     - Artifact rejection during coaddition excluded input pixels.
   * - | ``pixelFlags_clippedCenter``
       | O
     - Clipping occurred at center.
   * - | ``pixelFlags_inexact_psf``
       | O
     - Coadd PSF model is discontinuous in footprint, typically at cell or patch boundaries or where input artifacts were rejected.
   * - | ``pixelFlags_inexact_psfCenter``
       | O
     - Coadd PSF model is discontinuous at center. Covers a large area; not recommended as a general cut.
   * - | ``pixelFlags_nodata``
       | O, S, FS, DS, DFS
     - No pixel data available (off coverage area).
   * - | ``pixelFlags_nodataCenter``
       | DS
     - No pixel data available at center.
   * - | ``pixelFlags_streak``
       | DS
     - Masked streak (e.g., satellite trail) overlaps footprint.
   * - | ``pixelFlags_streakCenter``
       | DS
     - Masked streak overlaps center.
   * - | ``pixelFlags_injected``
       | DS
     - Synthetic-source injection overlaps footprint in the science image. Relevant only for injection test datasets.
   * - | ``pixelFlags_injectedCenter``
       | DS
     - Synthetic-source injection overlaps center in the science image.
   * - | ``pixelFlags_injected_template``
       | DS
     - Synthetic-source injection overlaps footprint in the template image.
   * - | ``pixelFlags_injected_templateCenter``
       | DS
     - Synthetic-source injection overlaps center in the template image.



Measurement failure flags
=========================

**Pattern:**
``{band}_{algorithm}_flag`` (Object table) or ``{algorithm}_flag`` (Source tables).

**Purpose:**
To indicate that a particular measurement algorithm failed or produced unreliable results.

**Key points:**
In general, when using a measured quantity for an object or source, check that its general failure flag is false.
For example, when using ``r_psfFlux``, the value of ``r_psfFlux_flag`` should be false (or 0).
Most algorithms provide both the general failure flag and one or more diagnostic subflags that explain what went wrong (e.g., ``psfFlux_flag_edge``, ``psfFlux_flag_noGoodPixels``).

**New for DP2:**
In the Object table most fluxes are forced (measured at the reference-band position; e.g., ``{band}_psfFlux``, ``{band}_cModel_*``).
DP2 also provides free (unforced) variants of these fluxes which are measured independently in each band (e.g., ``{band}_free_psfFlux``, ``{band}_free_cModelFlux``).
Both options have associated flags, so be sure to apply the flag that matches the type of flux measurement (e.g., use ``{band}_psfFlux_flag`` with ``{band}_psfFlux``, and ``{band}_free_psfFlux_flag`` iwth ``{band}_free_psfFlux``).


.. list-table::
   :header-rows: 1
   :widths: 25 60

   * - Flag name and tables\*
     - Meaning when set to 1
   * - | ``{band}_psfFlux_flag``
       | O, S, FS, DFS
     - PSF flux measurement failed; do not use the PSF flux.
   * - | ``{band}_cModel_flag``
       | O
     - CModel (galaxy model) fit failed; do not use the model fluxes.
   * - | ``{band}_kronFlux_flag``
       | O
     - Kron aperture flux failed (e.g. bad radius, near edge).
   * - | ``{band}_gaapFlux_flag``
       | O
     - GAaP (Gaussian Aperture and PSF) photometry failed.
   * - | ``{band}_apNNFlux_flag``
       | O, S
     - Aperture flux in the ``NN``-pixel aperture failed (e.g. ``ap12Flux_flag``).
   * - | ``centroid_flag``
       | S, DS
     - Centroid algorithm failed; do not trust the position.
   * - | ``coord_flag``
       | O
     - General reference-band centroid/coordinate failure.
   * - | ``shape_flag``
       | O, DS
     - Shape (second-moments) measurement failed.
   * - | ``{band}_extendedness_flag``
       | O
     - Flux-ratio star/galaxy classifier failed; ``extendedness`` unreliable.
   * - | ``{band}_sizeExtendedness_flag``
       | O
     - Moments-based star/galaxy classifier failed. Note that the third classifier, ``{band}_model_extendedness`` (and ``griz_model_extendedness``), has **no** corresponding failure flag; see :ref:`flags-object`.
   * - | ``{band}_hsmShapeRegauss_flag``
       | O
     - HSM Regaussianization shape measurement failed.
   * - | ``{band}_blendedness_flag``
       | O, S
     - Blendedness measurement failed.
   * - | ``sersic_no_data_flag``
       | O
     - Multi-band Sersic model fit had no data. New in DP2.
   * - | ``sersic_unknown_flag``
       | O
     - Multi-band Sersic model fit failed for an unspecified reason. New in DP2.
   * - | ``exponential_no_data_flag``
       | O
     - Multi-band exponential model fit had no data. New in DP2.
   * - | ``exponential_unknown_flag``
       | O
     - Multi-band exponential model fit failed for an unspecified reason. New in DP2.
   * - | ``{band}_moments_flag``
       | O
     - Higher-order moments measurement failed. New in DP2.
   * - | ``{band}_moments_psf_flag``
       | O
     - Higher-order moments of the PSF model failed. New in DP2.
   * - | ``{band}_moments_psf_debiased_flag``
       | O
     - Debiased PSF moments measurement failed. New in DP2.
   * - | ``{band}_psfModel_TwoGaussian_unknown_flag``
       | O
     - Two-Gaussian PSF model fit failed for an unspecified reason. New in DP2.
   * - | ``{band}_psfModel_TwoGaussian_no_inputs_flag``
       | O
     - Two-Gaussian PSF model fit had no inputs. New in DP2.


Difference image analysis (DIA) flags
=====================================

**Purpose:**
To indicate particular issues with :doc:`/processing/dia/index` (DIA; detections of variable, transient, and moving objects in difference images).

**Key points:**
No cut on the ``reliability`` column was applied before writing the DiaSource catalog (:ref:`dia-reliability` is a machine-learned real/bogus score).
The DiaSource table carries a general ``pixelFlags`` column, and when it has a value of true (or 1) the mask-plane bookkeeping for that footprint failed and *other* ``pixelFlags_*`` for that source may be incorrectly reported as false.


.. list-table::
   :header-rows: 1
   :widths: 25 60

   * - Flag name and tables\*
     - Meaning when set to 1
   * - | ``isDipole``
       | DS
     - Detection is well fit by a dipole model, i.e. a subtraction artifact (typically at bright stars). Exclude for clean transient samples.
   * - | ``isNegative``
       | DS
     - Source was detected as significantly negative (a flux decrease). New in DP2; keep or exclude depending on whether the science targets fading/disappearing sources.
   * - | ``glint_trail``
       | DS
     - Source is part of a "glint trail" (a line of detections likely from rotating orbital debris). Flagged, not removed.
   * - | ``dipoleFitAttempted``
       | DS
     - A dipole model was fit to this source (informational, not a quality reject).
   * - | ``trail_flag_edge``
       | DS
     - A trailed source extends onto or past edge pixels.
   * - | ``psfFlux_flag``
       | DS
     - PSF flux on the difference image failed. Require 0 to use the difference flux.
   * - | ``forced_PsfFlux_flag``
       | DS
     - Forced PSF photometry on the science (direct) image failed.
   * - | ``psfDiffFlux_flag``
       | FS, DFS
     - Forced PSF flux on the difference image failed.
   * - | ``diff_PixelFlags_nodataCenter``
       | FS, DFS
     - Forced position falls outside difference-image coverage (no template); the difference flux is invalid.


Special flags
=============

**Purpose:**
Additional notable flags that provide ancillary information about sources and objects.

**New for DP2:**
DP2 adds the :doc:`/products/catalogs/object_shear` table, which is produced by multi-band metadetection and carries its own suite of flags (e.g. ``gauss_flags``, ``pgauss_flags``, and their ``*_object_flags`` and ``*_shape_flags`` variants; ``bmask_flags``, ``ormask_flags``, ``image_flags``, ``psfOriginal_flags``, and the ``is_*_inner`` and ``is_primary`` selection flags).
Note that no deblending is performed prior to these measurements.


.. list-table::
   :header-rows: 1
   :widths: 25 60

   * - Flag name and tables\*
     - Meaning when set to 1
   * - | ``{band}_invalidPsfFlag``
       | O
     - The PSF model is invalid (no usable inputs); measurements are unreliable. Exclude these objects.
   * - | ``invalidPsfFlag``
       | S, FS, DFS
     - As above, for the single-epoch and forced tables.
   * - | ``{band}_inputCount_flag``
       | O
     - Failed to compute the number of coadd input exposures.



.. _calibration-flags:

Calibration flags
=================

**Pattern:**
``calib_*`` (Source table) or ``{band}_calib_*`` (Object table).

**Purpose:**
To indicate whether a source was used in astrometric calibration, photometric calibration, or PSF modeling during single-visit processing.

**Key points:**
For most science applications these flags can be ignored because they pertain to internal use in the calibration process.

**New for DP2:**
The ``calib_photometry_reserved`` flag is available in the Source table in DP2 but is no longer in the Object table.

**Additional details:**
The Source table does not contain the same single-visit detections used to estimate the PSF and fit the astrometric and photometric calibrations.
Those initial sources (the ``single_visit_star`` and ``recalibrated_star`` Butler dataset types) are intermediate products that are not retained in a final data release.
Detections for the Source table are made on the final visit image after all calibration steps are complete.
The ``{band}_calib_*`` columns in the Object table are propagated from the single-visit sources by a spatial match, and can suffer from mismatch problems in rare cases.
Note also that these flags currently reflect the preliminary single-detector astrometric and photometric calibration steps, not the later FGCM (see :doc:`/processing/calibration/photometric`) and GBDES (see :doc:`/processing/calibration/astrometric`) fits: they do reflect the stars that went into the final Piff :doc:`/processing/calibration/psf`.


.. list-table::
   :header-rows: 1
   :widths: 25 60

   * - Flag name and tables\*
     - Meaning when set to 1
   * - | ``calib_astrometry_used``
       | S, O
     - Source was used in the astrometric (WCS) solution.
   * - | ``calib_photometry_used``
       | S, O
     - Source was used in the photometric zeropoint determination.
   * - | ``calib_photometry_reserved``
       | S
     - Source was reserved (held out) from photometric calibration for validation.
   * - | ``calib_psf_used``
       | S, O
     - Source was used for PSF modeling.
   * - | ``calib_psf_reserved``
       | S, O
     - Source was reserved (held out) from PSF determination.
   * - | ``calib_psf_candidate``
       | S, O
     - Source was a candidate for PSF-star selection.

