.. _processing_failures:

.. |rarr| unicode:: U+02192

###################
Processing Failures
###################

The tables below trace the number of data products that are successfully processed at each step of the DRP. The tables are organized by grouping together products that are produced together, and final output data products are highlighted with links to their documentation page.

Single-visit products
=====================

These are products that contain data from a single visit:

- :ref:`images-visit-image`
- :ref:`catalogs-source`

.. rst-class:: technote-wide-content


+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
|   Tasks                                | Dataset counts                                    | Failures                                               |
+========================================+===================================================+========================================================+
|   **Precursor Datasets:**              | raw: 5289060                                      |                                                        |
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
|   **Single-frame processing:**         |                                                   |                                                        |
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
| - **isr**                              | | post_isr_image: 5288244                         | - Data too bad to process: 816                         | 
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
| - **calibrateImage**                   | | preliminary_visit_image: 5288221                | - ObjectSizeNoGoodSourcesError: 28694                  | 
|                                        | | single_visit_star_footprints: 5219551           | - NormalizedCalibrationFluxError: 1139                 | 
|                                        | | preliminary_visit_image_background: 5288221     | - AllCentroidsFlaggedError: 2436                       | 
|                                        | | preliminary_visit_mask: 5211275                 | - MatcherFailure: 9594                                 | 
|                                        | |                                                 | - BadAstrometryFit: 6383                               | 
|                                        | |                                                 | - MeasureApCorrError: 9296                             | 
|                                        | |                                                 | - NonfinitePsfShapeError: 2159                         | 
|                                        | |                                                 | - PsfexTooFewGoodStarsError: 17237                     | 
|                                        | |                                                 | - NoPsfStarsToStarsMatchError: 90                      | 
|                                        | |                                                 | - PsfexNoGoodStarsError: 22                            | 
|                                        | |                                                 | - TooManyCosmicRays: 1                                 | 
|                                        | |                                                 | - AstrometryError: 2                                   | 
|                                        | |                                                 | - ObjectSizeNoSourcesError: 2                          | 
|                                        | |                                                 | - RuntimeError: 23                                     | 
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
|   **Recalibration:**                   |                                                   |                                                        |
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
| - **refitPsfModelDetector**            | | refit_psf_star_detector: 5218106                | - No work found: 52075                                 | 
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
| - **consolidateRefitPsfModelDetector** | | refit_psf_star: 29680                           |                                                        | 
|                                        | |     (5284048 visit/detector dataIds)            |                                                        | 
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
| - **fgcmOutputProducts**               | | fgcmPhotoCalibCatalog: 28945                    |                                                        | 
|                                        | |     (5194385 visit/detector dataIds)            |                                                        | 
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
| - **gbdesHealpix3AstrometricFit**      | | gbdesHealpix3AstrometricFit_fitStars: 1619      | - RuntimeError: 145                                    | 
|                                        | | gbdesHealpix3AstrometricFitSkyWcsCatalog: 72369 | - CholeskyError: 17                                    | 
|                                        | |     (5215558 visit/detector dataIds)            | - No work found: 40                                    | 
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
| - **gaussianProcessesTurbulenceFit**   | | turbulenceCorrectedSkyWcsCatalog: 72115         | - SingularMatrixError: 253                             | 
|                                        | |     (5268032 visit/detector dataIds)            | - NotPositiveDefiniteMatrixError: 1                    | 
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
| - **updateVisitSummary**               | | visit_summary: 28698                            |                                                        | 
|                                        | |     (5289060 visit/detector dataIds)            |                                                        | 
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
|   **Image Reprocessing:**              |                                                   |                                                        |
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
| - **reprocessVisitImage**              | | :ref:`images-visit-image`: 4988654              | - TooManyCosmicRays: 31                                | 
|                                        | |                                                 | - One or more upstream datasets was incomplete: 115955 | 
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
| - **standardizeSource**                | | source_detector: 4988623                        |                                                        | 
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
| - **consolidateSource**                | | source_all: 28589                               |                                                        | 
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+
| - **splitPrimarySource**               | | :ref:`catalogs-source`: 28589                   |                                                        | 
+----------------------------------------+---------------------------------------------------+--------------------------------------------------------+


Image Coaddition Products
=========================


Forced Source
=============

- :ref:`catalogs-forced-source`

.. rst-class:: technote-wide-content

+--------------------------------------+------------------------------------------------+---------------------+
|   Tasks                              | Dataset counts                                 | Failures            |
+======================================+================================================+=====================+
|   **Precursor Datasets:**            | visit_image: 4988654                           |                     |
|                                      | difference_image: 3552364                      |                     |
|                                      | object_patch: 195366                           |                     |
|                                      | See *visit_image*, *difference_image*,         |                     |
|                                      | and *object* failure tracing                   |                     |
+--------------------------------------+------------------------------------------------+---------------------+
|   **Forced Photometry:**             |                                                |                     |
+--------------------------------------+------------------------------------------------+---------------------+
| - **forcedPhotObjectDetector**       | | object_forced_source_unstandardized: 6208791 |                     | 
+--------------------------------------+------------------------------------------------+---------------------+
|   **Table Reformatting:**            |                                                |                     |
+--------------------------------------+------------------------------------------------+---------------------+
| - **standardizeObjectForcedSource**  | | object_forced_source_all: 192151             | - No work found: 30 | 
+--------------------------------------+------------------------------------------------+---------------------+
| - **splitPrimaryObjectForcedSource** | | :ref:`catalogs-forced-source`: 192151        |                     | 
+--------------------------------------+------------------------------------------------+---------------------+

Stellar Motions Products
========================

- :ref:`isolated-star-stellar-motions`

.. rst-class:: technote-wide-content

+-----------------------------+----------------------------------------------+-------------------+
|   Tasks                     | Dataset counts                               | Failures          |
+=============================+==============================================+===================+
|   **Precursor Datasets:**   | preliminary_visit_summary: 29474             |                   |
|                             | recalibrated_star: 28698                     |                   |
|                             | preliminary_visit_table: 1                   |                   |
|                             | See *visit_image* failure tracing            |                   |
+-----------------------------+----------------------------------------------+-------------------+
|   **Source Association:**   |                                              |                   |
+-----------------------------+----------------------------------------------+-------------------+
| - **associateIsolatedStar** | | isolated_star: 8006                        |                   | 
|                             | | isolated_star_association: 8006            |                   | 
+-----------------------------+----------------------------------------------+-------------------+
|   **Stellar Motion Fit:**   |                                              |                   |
+-----------------------------+----------------------------------------------+-------------------+
| - **fitStellarMotion**      | | :ref:`isolated-star-stellar-motions`: 7809 | - RuntimeError: 8 | 
+-----------------------------+----------------------------------------------+-------------------+



