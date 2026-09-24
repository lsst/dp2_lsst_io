.. _flag-recommendations:

###################
Flag usage guidance
###################

The guidance below is intentionally **minimal and conservative**: it lists only a small set of flags that are almost always appropriate to exclude, for typical science-quality selections.
It is **not** a recipe for a "clean sample." The correct flag cuts depend strongly on your science case, and stricter or looser cuts than these will be appropriate for many analyses.

.. note::

   General rule: whenever you use a measured quantity (a flux, shape, color, etc.), require that quantity's own general failure flag to be ``0``.
   For example, if you use ``r_cModelFlux``, require ``r_cModel_flag = 0``. The minimal sets below cover the most common quality problems; the per-quantity rule covers the rest.

.. _flags-object:

Object table
============

Guidance for the deep-coadd measurements of static-sky objects.
In the snippets below, ``{band}`` stands for one of ``u``, ``g``, ``r``, ``i``, ``z``, ``y``; apply the same logic independently in each band you use.

**Minimal recommended set.**

.. code-block:: sql
   :force:

   WHERE {band}_inputCount > 0                        -- At least one input image at the position
     AND {band}_psfFlux_flag = 0                      -- (or the flag for whichever flux you use)
     AND {band}_invalidPsfFlag = 0                    -- Valid PSF model
     AND {band}_pixelFlags_saturatedCenter = 0        -- No saturation at the center
     AND {band}_pixelFlags_interpolatedCenter = 0     -- No interpolated pixels at the center

.. note::

   - Require ``{band}_inputCount > 0`` in each band used.
   - Replace ``{band}_psfFlux_flag`` with the failure flag of the flux you actually use (e.g. ``{band}_cModel_flag`` for CModel fluxes, ``{band}_free_psfFlux_flag`` for the free/unforced PSF flux — see the note on free versus forced measurements in :doc:`/products/flags/flag_definitions`).
   - The DP2 Object columns ``pixelFlags_bad``, ``pixelFlags_edge``, ``pixelFlags_suspect``/``pixelFlags_suspectCenter``, and ``pixelFlags_offimage`` are **deprecated** and must not be used as cuts here (see :doc:`/products/flags/flag_definitions`). Use ``pixelFlags_sensor_edgeCenter`` if you need a coadd edge cut.

**Optional, science-case-dependent cuts.**

.. code-block:: sql
   :force:

   AND {band}_pixelFlags_crCenter = 0            -- No cosmic ray at center
   AND {band}_pixelFlags_sensor_edgeCenter = 0   -- Not near a detector boundary (coadd edge)
   AND {band}_pixelFlags_interpolated = 0        -- Stricter: no interpolated pixels anywhere in the footprint

Galaxy / star selection (extendedness)
--------------------------------------

DP2 provides three star/galaxy classifiers in the Object table, per band, plus one multi-band variant.
They differ in what they measure, in whether a companion failure flag exists, and in how meaningful the numeric value is.

.. list-table::
   :header-rows: 1
   :widths: 24 12 14 50

   * - Column
     - Range
     - Failure flag
     - Notes
   * - ``{band}_extendedness``
     - 0 or 1
     - ``{band}_extendedness_flag``
     - PSF-to-CModel flux ratio, thresholded by the pipeline.
   * - ``{band}_sizeExtendedness``
     - 0 to 1
     - ``{band}_sizeExtendedness_flag``
     - Moments-based comparison of the source size to the local PSF.
   * - ``{band}_model_extendedness``
     - 0 to 1
     - *none*
     - Sersic model flux- and size-based, single band.
   * - ``griz_model_extendedness``
     - 0 to 1
     - *none*
     - Sersic model flux- and size-based, combining the ``griz`` bands.

**Which one to use.**
``model_extendedness`` is the most broadly usable classifier in DP2: it is the most likely of the three to have a finite value for a given object, and ``griz_model_extendedness`` combines the four bands with the best signal.
There is no associated flag column.

Selecting galaxies with ``model_extendedness``:

.. code-block:: sql
   :force:

   AND {band}_model_extendedness > 0.3
   AND {band}_model_extendedness <= 1

and point sources with the complementary cut (``>= 0`` and ``<= 0.3``).

Selecting galaxies with ``sizeExtendedness``:

.. code-block:: sql
   :force:

   AND {band}_sizeExtendedness > 0.5
   AND {band}_sizeExtendedness_flag = 0

Selecting galaxies with the binary ``extendedness``:

.. code-block:: sql
   :force:

   AND {band}_extendedness = 1       -- Extended source (galaxy); use = 0 for point sources (stars)
   AND {band}_extendedness_flag = 0  -- Classification valid

.. warning::

   The 0.3 cut on ``model_extendedness`` was only tested in the Deep Drilling Fields.
   It is not a recommended default, and the best value will change with depth and seeing.
   Check the distribution in your own field and pick a cut that suits your science case.

.. note::

   None of the DP2 star/galaxy classifiers has been fully characterized for purity or completeness, and there is no published selection function for any of these cuts.
   Treat star/galaxy separation as approximate and validate it against your own science requirements.
   ``refExtendedness`` and ``refSizeExtendedness`` give the reference-band values of the first two classifiers if you want a single band-independent classification.

Model photometry and shapes: require the matching general flag when using the quantity, e.g. ``{band}_cModel_flag = 0`` for CModel fluxes, ``{band}_kronFlux_flag = 0`` for Kron fluxes, or ``{band}_hsmShapeRegauss_flag = 0`` for HSM shapes.

.. _flags-source:

Source table
============

Guidance for single-epoch visit detections.

**Minimal recommended set.**

.. code-block:: sql

   WHERE centroid_flag = 0                 -- Centroid succeeded
     AND psfFlux_flag = 0                    -- (or the flag for whichever flux you use)
     AND pixelFlags_saturatedCenter = 0      -- No saturation at center
     AND pixelFlags_interpolatedCenter = 0   -- No interpolation at center

**Optional, science-case-dependent cuts.**

.. code-block:: sql

   AND pixelFlags_crCenter = 0       -- No cosmic ray at center
   AND pixelFlags_edge = 0           -- Not on the exposure edge
   AND pixelFlags_bad = 0            -- No known-bad pixels in footprint
   AND pixelFlags_suspectCenter = 0  -- No suspect pixels at center

Unlike the Object table, ``pixelFlags_bad``, ``pixelFlags_edge``, and ``pixelFlags_suspect`` are valid in the Source table.
If you are selecting or excluding calibration stars, use the ``calib_*`` flags (see :ref:`calibration-flags`).

.. _flags-forced-source:

ForcedSource table
==================

Guidance for forced photometry at Object positions on single-epoch images.
When building light curves, apply these flags per measurement (row) so that poor epochs are dropped while good epochs for the same object are kept.

**Minimal recommended set (science-image flux).**

.. code-block:: sql

   WHERE psfFlux_flag = 0                  -- Science-image PSF flux succeeded
     AND invalidPsfFlag = 0                 -- Valid PSF model
     AND pixelFlags_saturatedCenter = 0     -- No saturation at the forced position

**If using the difference-image flux, add:**

.. code-block:: sql

   AND psfDiffFlux_flag = 0               -- Difference-image flux succeeded
   AND diff_PixelFlags_nodataCenter = 0   -- Difference image has coverage at this position

.. _flags-dia-source:

DiaSource table
===============

Guidance for transient/variable detections on difference images.

.. important::

   No real/bogus reliability cut was applied when building the DP2 DiaSource catalog.
   For a higher-purity transient sample, apply a minimum threshold on the ``reliability`` column in addition to the flags below; the appropriate threshold is science-case dependent.

**Minimal recommended set.**

.. code-block:: sql

   WHERE isDipole = 0                    -- Not a dipole subtraction artifact
     AND psfFlux_flag = 0                -- Difference-image flux succeeded
     AND pixelFlags_saturatedCenter = 0  -- No saturation at center

**Optional, science-case-dependent cuts.**

.. code-block:: sql

   AND centroid_flag = 0   -- Reliable position
   AND pixelFlags_crCenter = 0  -- Not a cosmic-ray residual at center
   AND glint_trail = 0     -- Exclude probable orbital-debris glint trails
   AND isNegative = 0      -- Exclude flux-decrease detections (keep them for fading/disappearing sources)

.. _flags-dia-forced:

ForcedSourceOnDiaObject table
=============================

Guidance for forced photometry at DiaObject positions.
As with ForcedSource, filter per measurement (row) to remove bad epochs while keeping good ones.

**Minimal recommended set (difference-image flux).**

.. code-block:: sql

   WHERE psfDiffFlux_flag = 0              -- Difference-image flux succeeded
     AND diff_PixelFlags_nodataCenter = 0  -- Difference image has coverage
     AND invalidPsfFlag = 0                -- Valid PSF model
     AND pixelFlags_saturatedCenter = 0    -- No saturation at position

**If using the science-image flux instead:**

.. code-block:: sql

   WHERE psfFlux_flag = 0                  -- Science-image flux succeeded
     AND invalidPsfFlag = 0                -- Valid PSF model
     AND pixelFlags_saturatedCenter = 0    -- No saturation at position
