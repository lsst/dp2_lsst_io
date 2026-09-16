.. _processing_failures:

###################
Processing failures
###################

These pages trace the number of data products that are successfully processed at each step of the DRP.
Tasks are grouped by the products they produce, and final output data products are highlighted with links to their documentation page.

Each task lists the datasets it wrote, followed by the error modes that prevented it from writing a product and the number of quanta affected.

.. _processing_failures_single_visit:

Single-visit products
=====================

These are products that contain data from a single visit:

- :ref:`images-visit-image`
- :ref:`catalogs-source`

Precursor datasets
------------------

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``raw``
     - 5,289,060

Single-frame processing
-----------------------

isr
^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``post_isr_image``
     - 5,288,244

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Failure
     - Count
   * - Data too bad to process
     - 816

calibrateImage
^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``preliminary_visit_image``
     - 5,288,221
   * - ``single_visit_star_footprints``
     - 5,219,551
   * - ``preliminary_visit_image_background``
     - 5,288,221
   * - ``preliminary_visit_mask``
     - 5,211,275

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Failure
     - Count
   * - ``ObjectSizeNoGoodSourcesError``
     - 28,694
   * - ``PsfexTooFewGoodStarsError``
     - 17,237
   * - ``MatcherFailure``
     - 9,594
   * - ``MeasureApCorrError``
     - 9,296
   * - ``BadAstrometryFit``
     - 6,383
   * - ``AllCentroidsFlaggedError``
     - 2,436
   * - ``NonfinitePsfShapeError``
     - 2,159
   * - ``NormalizedCalibrationFluxError``
     - 1,139
   * - ``NoPsfStarsToStarsMatchError``
     - 90
   * - ``RuntimeError``
     - 23
   * - ``PsfexNoGoodStarsError``
     - 22
   * - ``AstrometryError``
     - 2
   * - ``ObjectSizeNoSourcesError``
     - 2
   * - ``TooManyCosmicRays``
     - 1

Recalibration
-------------

refitPsfModelDetector
^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``refit_psf_star_detector``
     - 5,218,106

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Failure
     - Count
   * - No work found
     - 52,075

consolidateRefitPsfModelDetector
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``refit_psf_star``
     - 29,680

Covers 5,284,048 visit/detector dataIds.

No failures recorded.

fgcmOutputProducts
^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``fgcmPhotoCalibCatalog``
     - 28,945

Covers 5,194,385 visit/detector dataIds.

No failures recorded.

gbdesHealpix3AstrometricFit
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``gbdesHealpix3AstrometricFit_fitStars``
     - 1,619
   * - ``gbdesHealpix3AstrometricFitSkyWcsCatalog``
     - 72,369

``gbdesHealpix3AstrometricFitSkyWcsCatalog`` covers 5,215,558 visit/detector dataIds.

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Failure
     - Count
   * - ``RuntimeError``
     - 145
   * - No work found
     - 40
   * - ``CholeskyError``
     - 17

gaussianProcessesTurbulenceFit
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``turbulenceCorrectedSkyWcsCatalog``
     - 72,115

Covers 5,268,032 visit/detector dataIds.

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Failure
     - Count
   * - ``SingularMatrixError``
     - 253
   * - ``NotPositiveDefiniteMatrixError``
     - 1

updateVisitSummary
^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``visit_summary``
     - 28,698

Covers 5,289,060 visit/detector dataIds.

No failures recorded.

Image reprocessing
------------------

reprocessVisitImage
^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - :ref:`images-visit-image`
     - 4,988,654

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Failure
     - Count
   * - One or more upstream datasets was incomplete
     - 115,955
   * - ``TooManyCosmicRays``
     - 31

standardizeSource
^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``source_detector``
     - 4,988,623

No failures recorded.

consolidateSource
^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``source_all``
     - 28,589

No failures recorded.

splitPrimarySource
^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - :ref:`catalogs-source`
     - 28,589

No failures recorded.

.. _processing_failures_coadds:

Image coaddition products
=========================

.. _processing_failures_forced_source:

Forced source
=============

- :ref:`catalogs-forced-source`

Precursor datasets
------------------

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``visit_image``
     - 4,988,654
   * - ``difference_image``
     - 3,552,364
   * - ``object_patch``
     - 195,366

See the ``visit_image``, ``difference_image``, and ``object`` failure tracing for these inputs.

Forced photometry
-----------------

forcedPhotObjectDetector
^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``object_forced_source_unstandardized``
     - 6,208,791

No failures recorded.

Table reformatting
------------------

standardizeObjectForcedSource
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``object_forced_source_all``
     - 192,151

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Failure
     - Count
   * - No work found
     - 30

splitPrimaryObjectForcedSource
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - :ref:`catalogs-forced-source`
     - 192,151

No failures recorded.

.. _processing_failures_stellar_motions:

Stellar motions products
========================

- :ref:`isolated-star-stellar-motions`

Precursor datasets
------------------

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``preliminary_visit_summary``
     - 29,474
   * - ``recalibrated_star``
     - 28,698
   * - ``preliminary_visit_table``
     - 1

See the ``visit_image`` failure tracing for these inputs.

Source association
------------------

associateIsolatedStar
^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - ``isolated_star``
     - 8,006
   * - ``isolated_star_association``
     - 8,006

No failures recorded.

Stellar motion fit
------------------

fitStellarMotion
^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Dataset
     - Count
   * - :ref:`isolated-star-stellar-motions`
     - 7,809

.. list-table::
   :header-rows: 1
   :widths: auto
   :class: dp2-count-table

   * - Failure
     - Count
   * - ``RuntimeError``
     - 8
