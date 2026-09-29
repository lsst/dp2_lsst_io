.. _flag-usage:

#################
Flag use examples
#################

The flag use examples below include only a small set of flags that apply to most typical science analyses.

**Warning:**
These examples are not recipes for a "clean" sample (i.e., pure or complete).
The correct application of flags depends on the science analysis.

**General advice:**

* **It is recommended to test queries with and without flag cuts to understand the selection effects and how they impact the science analysis.**
* When a measured quantity is used in an analysis, check also that the quantity's general failure flag is false. For example, when using ``r_cModelFlux``, check that the value of ``r_cModel_flag`` is false (or 0). In the snippets below, ``{band}`` stands for one of ``u``, ``g``, ``r``, ``i``, ``z``, ``y``.


.. _flags-object:

Object table
============

**Guidance:**

* Use the ``{band}`` flag for each band that is necessary for the analysis.
* In the typical example below, replace ``{band}_psfFlux_flag`` with the failure flag for the flux type used in the analysis (e.g., ``{band}_cModel_flag``, ``{band}_free_psfFlux_flag``).
* For model photometry and shapes, require the matching general flag when using the quantity, e.g., ``{band}_cModel_flag = 0`` for CModel fluxes, ``{band}_kronFlux_flag = 0`` for Kron fluxes, or ``{band}_hsmShapeRegauss_flag = 0`` for HSM shapes.


**Selection effects:**
For a spatial query on the Object table near the edge of the COSMOS field, the typical example below reduces by :math:`\sim`10% the number of objects returned; with the two additional examples, the total reduction is :math:`\sim`23%.
The fraction of objects cut will change depending on the observations obtained in the region being queried, and the filters included.
It is important to consider which flag cuts are necessary for a given science analysis, and to characterize the selection effects.


Typical example:

.. code-block:: sql
   :force:

   WHERE {band}_inputCount > 0                        -- At least one input image at the position
     AND {band}_psfFlux_flag = 0                      -- Successful flux measurement (replace with flux type used)
     AND {band}_invalidPsfFlag = 0                    -- Valid PSF model
     AND {band}_pixelFlags_saturatedCenter = 0        -- No saturation at the center
     AND {band}_pixelFlags_interpolatedCenter = 0     -- No interpolated pixels at the center


Additional examples:

.. code-block:: sql
   :force:

   AND {band}_pixelFlags_crCenter = 0            -- No cosmic ray at center
   AND {band}_pixelFlags_interpolated = 0        -- No interpolated pixels anywhere in the footprint



Extendedness
------------

**Key points:**

* An "extendedness" parameter provides a measure of whether an astrophysical sources is point-like or extended. These parameters can help to distinguish between point-like stars and extended galaxies, but keep in mind that high-redshift objects can also appear point-like.
* There are three measures of extendedness in the Object table, per band, plus one multi-band variant. Each differ with respect to what is measured, whether a companion failure flag exists, and how meaningful the numeric value is.

**Guidance:**

* The extendedness parameters have not been characterized or validated as a star/galaxy separation parameter, and performance will vary across the DP2 fields due to their varying image depth and image quality. Any selection based on the extendedness parameters should be tested and validated for the science analysis.
* The ``model_extendedness`` parameter is the most likely to have a finite value for a given object, and the ``griz_model_extendedness`` combines the four bands with the best signal. However, the drawback is that there is no associated flag column and the ``griz_model_extendedness`` tends to classify everything as a galaxy fainter than approximately ``i`` = 24 mag (see the discussion under :ref:`detection-measurement`).
* See also the :doc:`tutorial notebook </tutorials/notebook/index>` on extendedness.


.. list-table::
   :header-rows: 1
   :widths: 24 14 50

   * - Columns
     - Range
     - Notes
   * - | ``{band}_extendedness``
       | ``{band}_extendedness_flag``
     - | 0 or 1
       | 0 or 1
     - PSF-to-CModel flux ratio, thresholded by the pipeline.
   * - | ``refExtendedness``
       | *no flag*
     - 0 or 1
     - PSF-to-CModel flux ratio, in the ``refBand``.
   * - | ``{band}_sizeExtendedness``
       | ``{band}_sizeExtendedness_flag``
     - | 0 to 1
       | 0 or 1
     - Moments-based comparison of the source size to the local PSF.
   * - | ``refSizeExtendedness``
       | *no flag*
     - 0 to 1
     - Moments-based extendedness, in the ``refBand``.
   * - | ``{band}_model_extendedness``
       | *no flag*
     - 0 to 1
     - Sersic model flux- and size-based, single band.
   * - | ``griz_model_extendedness``
       | *no flag*
     - 0 to 1
     - Sersic model flux- and size-based, combining the ``griz`` bands.



.. _flags-source:

Source table
============

**Guidance:**

* Unlike the Object table, ``pixelFlags_bad``, ``pixelFlags_edge``, and ``pixelFlags_suspect`` are valid in the Source table.
* When selecting or excluding calibration stars, use the ``calib_*`` flags (see :ref:`calibration-flags`).


Typical example:

.. code-block:: sql

   WHERE centroid_flag = 0                 -- Centroid succeeded
     AND psfFlux_flag = 0                    -- (or the flag for whichever flux you use)
     AND pixelFlags_saturatedCenter = 0      -- No saturation at center
     AND pixelFlags_interpolatedCenter = 0   -- No interpolation at center


Additional examples:

.. code-block:: sql

   AND pixelFlags_crCenter = 0       -- No cosmic ray at center
   AND pixelFlags_edge = 0           -- Not on the exposure edge
   AND pixelFlags_bad = 0            -- No known-bad pixels in footprint
   AND pixelFlags_suspectCenter = 0  -- No suspect pixels at center



.. _flags-forced-source:

ForcedSource table
==================

**Guidance:**
* When building light curves, apply these flags per measurement (row) so that poor epochs are dropped while good epochs for the same object are kept.

Typical example:

.. code-block:: sql

   WHERE psfFlux_flag = 0                  -- Science-image PSF flux succeeded
     AND invalidPsfFlag = 0                 -- Valid PSF model
     AND pixelFlags_saturatedCenter = 0     -- No saturation at the forced position


If using the difference-image flux, add:

.. code-block:: sql

   AND psfDiffFlux_flag = 0               -- Difference-image flux succeeded
   AND diff_PixelFlags_nodataCenter = 0   -- Difference image has coverage at this position


.. _flags-dia-source:

DiaSource table
===============

**Guidance:**
* No cut on the ``reliability`` column was applied before writing the DiaSource catalog (:ref:`dia-reliability` is a machine-learned real/bogus score).

Typical example:

.. code-block:: sql

   WHERE isDipole = 0                    -- Not a dipole subtraction artifact
     AND psfFlux_flag = 0                -- Difference-image flux succeeded
     AND pixelFlags_saturatedCenter = 0  -- No saturation at center


Additional examples:

.. code-block:: sql

   AND centroid_flag = 0   -- Reliable position
   AND pixelFlags_crCenter = 0  -- Not a cosmic-ray residual at center
   AND glint_trail = 0     -- Exclude probable orbital-debris glint trails
   AND isNegative = 0      -- Exclude flux-decrease detections (keep them for fading/disappearing sources)


.. _flags-dia-forced:

ForcedSourceOnDiaObject table
=============================

**Guidance:**
* As with ForcedSource, filter per measurement (row) to remove bad epochs while keeping good ones.

Typical example when using difference-image flux:

.. code-block:: sql

   WHERE psfDiffFlux_flag = 0              -- Difference-image flux succeeded
     AND diff_PixelFlags_nodataCenter = 0  -- Difference image has coverage
     AND invalidPsfFlag = 0                -- Valid PSF model
     AND pixelFlags_saturatedCenter = 0    -- No saturation at position

If using the science-image flux instead:

.. code-block:: sql

   WHERE psfFlux_flag = 0                  -- Science-image flux succeeded
     AND invalidPsfFlag = 0                -- Valid PSF model
     AND pixelFlags_saturatedCenter = 0    -- No saturation at position
