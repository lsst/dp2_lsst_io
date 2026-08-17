.. _flag-recommendations:

###################
Flag usage guidance
###################

.. important::

   DP2 flag descriptions and guidance for their scientific applications are still being developed and validated.
   The guidance below is intentionally **minimal and conservative**: it lists only a small set of flags that are almost always appropriate to exclude, for typical science-quality selections.
   It is **not** a recipe for a "clean sample." The correct flag cuts depend strongly on your science case, and stricter or looser cuts than these will be appropriate for many analyses.

The tables below give, per catalog table, a **minimal recommended set** of flags to apply, followed by **optional, science-case-dependent** cuts.
Only the minimal set is suggested for most analyses; treat everything under "optional" as a starting point to adapt, not a default.

.. note::

   General rule: whenever you use a measured quantity (a flux, shape, color, etc.), require that quantity's own general failure flag to be ``0``.
   For example, if you use ``r_cModelFlux``, require ``r_cModel_flag = 0``. The minimal sets below cover the most common quality problems; the per-quantity rule covers the rest.

.. _flags-object:

Object table
============

Guidance for the deep-coadd measurements of static-sky objects.
In the snippets below, ``f`` is the band (one of ``ugrizy``); apply the same logic independently in each band you use.

**Minimal recommended set.**

.. code-block:: sql

   WHERE detect_isPrimary = 1                   -- Primary, deblended, de-duplicated detection
     AND f_psfFlux_flag = 0                      -- (or the flag for whichever flux you use)
     AND f_invalidPsfFlag = 0                    -- Valid PSF model
     AND f_pixelFlags_saturatedCenter = 0        -- No saturation at the center
     AND f_pixelFlags_interpolatedCenter = 0     -- No interpolated pixels at the center

.. note::

   - ``detect_isPrimary = 1`` is important: it selects the single, primary, deblended version of each detection and removes duplicate and pre-deblend rows.
   - Replace ``f_psfFlux_flag`` with the failure flag of the flux you actually use (e.g. ``f_cModel_flag`` for CModel fluxes, ``f_free_psfFlux_flag`` for the free/unforced PSF flux — see the note on free versus forced measurements in :doc:`/products/flags/flag_definitions`).
   - The DP2 Object columns ``pixelFlags_bad``, ``pixelFlags_edge``, and ``pixelFlags_suspect`` are **deprecated** and must not be used as cuts here (see :doc:`/products/flags/flag_definitions`). Use ``pixelFlags_sensor_edgeCenter`` if you need a coadd edge cut.

**Optional, science-case-dependent cuts.**

.. code-block:: sql

   AND f_pixelFlags_crCenter = 0            -- No cosmic ray at center
   AND f_pixelFlags_sensor_edgeCenter = 0   -- Not near a detector boundary (coadd edge)
   AND f_pixelFlags_interpolated = 0        -- Stricter: no interpolated pixels anywhere in the footprint

Galaxy / star selection (extendedness):

.. code-block:: sql

   AND f_extendedness = 1       -- Extended source (galaxy); use = 0 for point sources (stars)
   AND f_extendedness_flag = 0  -- Classification valid

.. note::

   The ``extendedness`` classifier has not been fully characterized for purity or completeness, and there is no published selection function for these cuts.
   Treat star/galaxy separation as approximate and validate it against your own science requirements.
   DP2 also provides ``sizeExtendedness`` and a continuous ``model_extendedness`` (per band and for summed ``griz``); consider these as alternatives.

Model photometry and shapes: require the matching general flag when using the quantity, e.g. ``f_cModel_flag = 0`` for CModel fluxes, ``f_kronFlux_flag = 0`` for Kron fluxes, or ``f_hsmShapeRegauss_flag = 0`` for HSM shapes.

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

   No real/bogus reliability cut was applied when building the DP2 DiaSource catalog, and the pipeline favors completeness over purity.
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
