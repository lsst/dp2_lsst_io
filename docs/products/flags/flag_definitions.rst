.. _flag-definitions:

###############################
Flag definitions and categories
###############################

Flag inventory
==============

TBD.

Flag categories
===============

To help users interpret flag meanings, the following sections organize flags into categories based on what the flag indicates.

Pixel quality flags
-------------------

Pattern: ``{band}_pixelFlags_*``

Purpose: Report on issues with individual pixels in the source footprint, derived from image mask planes <images-mask-planes>.

The most commonly used pixel quality flags are listed in the table below.

.. list-table::
   :header-rows: 1
   :widths: 25 15 60

   * - Flag name
     - Tables
     - Meaning when set to 1
   * - ``{band}_pixelFlags_{flagname}``
     - Table1, Table2
     - Description here.


Measurement failure flags
--------------------------

Pattern: ``*_flag`` (algorithm-specific)

Purpose: Indicate that a particular measurement algorithm failed or produced unreliable results.

The general measurement failure flags are listed in the table below.

.. list-table::
   :header-rows: 1
   :widths: 30 15 55

   * - Flag name
     - Tables
     - Meaning when set to 1
   * - ``{band}_{column}_flag``
     - Table1, Table2
     - Description here.

Difference imaging flags
------------------------

The DiaSource-specific flags are listed in the table below.

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Flag name
     - Meaning and recommendation
   * - ``{flagname}``
     - Description here.

The ForcedSourceOnDiaObject difference imaging flags are listed in the table below.

.. list-table::
   :header-rows: 1
   :widths: 35 65

   * - Flag name
     - Meaning when set to 1
   * - ``{flagname}``
     - Description here.

Specialized flags
-----------------

Other notable flags are listed in the table below.

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
-----------------

Pattern: ``{band}_calib_*``

For most science, these flags can be ignored as they pertain to internal use in the calibration process.

These flags indicate whether a source was used in astrometric calibration, photometric calibration, or PSF modeling during single-visit processing.

The common calibration flags are listed in the table below.

.. list-table::
   :header-rows: 1
   :widths: 30 15 55

   * - Flag name
     - Tables
     - Meaning when set to 1
   * - ``{band}_calib_{flagname}``
     - Table1, Table2
     - Description here.

These flags are primarily diagnostic.
Calibrator stars can be excluded if needed (e.g., ``calib_photometry_used = 0`` to remove stars used for zeropoint fitting), but the preliminary nature means some true calibrators will not be flagged and vice versa.
