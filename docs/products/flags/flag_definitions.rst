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

.. list-table::
   :header-rows: 1
   :widths: 30 15 55

   * - Calibration flag
     - Tables
     - Meaning when set to 1
   * - ``{band}_calib_{flagname}``
     - Table1, Table2
     - Description here.

