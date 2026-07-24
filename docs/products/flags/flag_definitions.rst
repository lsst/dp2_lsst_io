.. _flag-definitions:

###############################
Flag definitions and categories
###############################

To help users interpret flag meanings, the following sections organize key flags into categories based on what the flag indicates.
See also the :doc:`/products/flags/flag_recommendations` for recommendations on the application of flags for scientific analyses.

This is not a complete list, and focuses on the most scientifically useful flags.

Pixel quality flags
===================

Pattern: ``{band}_pixelFlags_*``

Purpose: Report on issues with individual pixels in the source footprint, derived from image mask planes <images-mask-planes>.

.. list-table::
   :header-rows: 1
   :widths: 25 15 60

   * - Pixel quality flag
     - Tables
     - Meaning when set to 1
   * - ``{band}_pixelFlags_{flagname}``
     - Table1, Table2
     - Description here.


Measurement failure flags
=========================

Pattern: ``*_flag`` (algorithm-specific)

Purpose: Indicate that a particular measurement algorithm failed or produced unreliable results.

.. list-table::
   :header-rows: 1
   :widths: 30 15 55

   * - Measurement flag
     - Tables
     - Meaning when set to 1
   * - ``{band}_{column}_flag``
     - Table1, Table2
     - Description here.


DIA flags
=========

Purpose: Indicate particular issues with difference image analysis (DIA).

.. list-table::
   :header-rows: 1
   :widths: 30 15 55

   * - DIA flag
     - Tables
     - Meaning when set to 1
   * - ``{flagname}``
     - Table1, Table2
     - Description here.


Special flags
=============

Additional notable flags that provide ancillary information about source measurements.

.. list-table::
   :header-rows: 1
   :widths: 30 20 50

   * - Flag name
     - Tables
     - Meaning when set to 1
   * - ``{flagname}``
     - Table1, Table2, Table3
     - Description here.


.. _calibration-flags:

Calibration flags
=================

Pattern: ``{band}_calib_*``

Purpose: These flags indicate whether a source was used in astrometric calibration, photometric calibration, or PSF modeling during single-visit processing.

**For most science applications, these flags can be ignored as they pertain to internal use in the calibration process.**

The public Source catalog does not contain the same single-visit detections used to estimate the PSF and fit for the astrometric and photometric calibrations.
Those initial sources (the ``single_visit_star`` and ``recalibrated_star`` butler dataset types) are some of the many intermediate data products that are not retained in a final data release, while Source detections are made on the final visit image after all calibration steps are complete.

The ``calib_*`` columns in the Object table, which purport to identify objects used in various calibration steps, are generated from a spatial match from the object positions to the initial source positions, which means they can suffer from mismatch problems in rare cases.

Note also that these flags currently reflect the preliminary single-detector astrometric and photometric calibration steps, not the later FGCM and GBDES fits (they do reflect the stars that went into the final Piff PSF models, however).
This will be fixed in future data releases.

.. list-table::
   :header-rows: 1
   :widths: 30 15 55

   * - Calibration flag
     - Tables
     - Meaning when set to 1
   * - ``{band}_calib_{flagname}``
     - Table1, Table2
     - Description here.

