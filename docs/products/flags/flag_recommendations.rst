.. _flag-recommendations:

###################
Flag usage guidance
###################

Table-specific guidance on which flags to apply for *typical* science-quality selections.
It is up to the user to adapt this guidance to their *specific* science applications.

.. _flags-object:

Object table
============

Guidance for the application of flags on the deep coadd measurements of static sky objects.

Multi-band requirements: when requiring detections in multiple bands, ensure flux measurements and key pixel flags are valid in each band used.
Check ``pixelFlags_nodata`` to confirm coverage.

Band-specific flags: The Object table has approximately 100 flags per band.
The naming pattern is ``{band}_{measurement}_flag`` (e.g., ``g_psfFlux_flag``, ``i_cModel_flag``).
Apply the same flag logic to each band independently.

In the code snippets below, ``f`` is the filter (ugrizy).

.. code-block:: sql

   WHERE f_psfFlux_flag = 0                  -- PSF flux succeeded
     AND f_pixelFlags_saturatedCenter = 0     -- No saturation at center
     AND f_pixelFlags_crCenter = 0            -- No cosmic ray at center
     AND f_pixelFlags_interpolatedCenter = 0  -- No interpolation at center
     AND f_pixelFlags_sensor_edgeCenter = 0   -- Not on detector edge
     AND f_invalidPsfFlag = 0                 -- Valid PSF model

Galaxy samples
--------------

.. code-block:: sql

   AND f_cModel_flag = 0        -- CModel fit succeeded
   AND f_extendedness = 1       -- Extended source (galaxy)
   AND f_extendedness_flag = 0  -- Classification valid

.. note::

   The ``extendedness`` classifier has not been fully characterized for purity or completeness.
   There is currently no published guidance on the selection function these cuts produce.
   Users should treat galaxy/star separation as approximate and validate against their specific science requirements.

Star samples
------------

.. code-block:: sql

   AND f_extendedness = 0       -- Point source (star)
   AND f_extendedness_flag = 0  -- Classification valid

.. note::

   The same caveats about the ``extendedness`` classifier apply as for galaxy samples above.

Stars used and reserved in PSF modeling can also be identified using ``{band}_calib_psf_used`` and ``{band}_calib_psf_reserved`` in the Source table (see :ref:`calibration-flags`).

High-precision Kron photometry
------------------------------

.. code-block:: sql

   AND f_kronFlux_flag = 0  -- Kron flux succeeded

High-precision shape measurements
---------------------------------

.. code-block:: sql

   AND f_hsmShapeRegauss_flag = 0  -- HSM shapes succeeded

Strict pixel quality
--------------------

This is optional.

.. code-block:: sql

   AND f_pixelFlags_interpolated = 0  -- No interpolated pixels in footprint


.. _flags-source:

Source table
============

Guidance for the application of flags on single-epoch visit detections.

.. code-block:: sql

   WHERE centroid_flag = 0               -- Centroid succeeded
     AND psfFlux_flag = 0                -- PSF flux succeeded
     AND pixelFlags_edge = 0             -- Not on CCD edge
     AND pixelFlags_saturatedCenter = 0  -- No saturation at center
     AND pixelFlags_bad = 0              -- No bad pixels

Additional quality filters.

.. code-block:: sql

   AND pixelFlags_crCenter = 0            -- No cosmic ray at center
   AND pixelFlags_interpolatedCenter = 0  -- No interpolation at center
   AND pixelFlags_suspectCenter = 0       -- No suspect pixels at center

Calibration stars
-----------------

If specifically selecting or excluding calibration stars, use ``calib_*`` flags (including ``calib_psf_used`` and ``calib_psf_reserved``).


.. _flags-forced-source:

ForcedSource table
==================

Guidance for the application of flags on forced photometry at Object positions.

When constructing light curves, apply these flags to each measurement (row) individually.
This filters out poor-quality epochs while retaining good measurements for the same object across other visits.

Science-image flux
------------------

.. code-block:: sql

   WHERE psfFlux_flag = 0                -- PSF flux succeeded
     AND pixelFlags_saturatedCenter = 0  -- No saturation at forced position
     AND pixelFlags_edge = 0             -- Position not on edge
     AND invalidPsfFlag = 0              -- PSF model valid


Difference-image flux
---------------------

.. code-block:: sql

   AND psfDiffFlux_flag = 0              -- Difference flux succeeded
   AND diff_PixelFlags_nodataCenter = 0  -- Difference image has coverage


.. _flags-dia-source:

DiaSource table
===============

Guidance for the application of flags on transient/variable detections on difference images.

High-confidence real astrophysical transients.

.. code-block:: sql

   WHERE isDipole = 0                    -- Not a subtraction dipole artifact
     AND psfFlux_flag = 0                -- Difference flux succeeded
     AND pixelFlags_edge = 0             -- Not on edge
     AND pixelFlags_saturatedCenter = 0  -- No saturation
     AND pixelFlags_bad = 0              -- No bad pixels

Additional quality filters.

.. code-block:: sql

   AND centroid_flag = 0  -- Position reliable
   AND pixelFlags_cr = 0  -- Not a cosmic ray residual


.. _flags-dia-forced:

ForcedSourceOnDiaObject table
=============================

Guidance for the application of flags on forced photometry at DiaObject positions.

When constructing light curves, apply these flags to each measurement (row) individually.
This filters out poor-quality epochs while retaining good measurements for the same object across other visits.

Difference-image flux
---------------------

.. code-block:: sql

   WHERE psfDiffFlux_flag = 0              -- Difference flux succeeded
     AND diff_PixelFlags_nodataCenter = 0  -- Difference image has coverage
     AND pixelFlags_saturatedCenter = 0    -- No saturation
     AND invalidPsfFlag = 0                -- PSF valid

Science-image flux
------------------

.. code-block:: sql

   WHERE psfFlux_flag = 0                  -- Science image flux succeeded
     AND pixelFlags_bad = 0                -- No bad pixels
     AND pixelFlags_saturatedCenter = 0    -- No saturation
     AND invalidPsfFlag = 0                -- PSF valid

