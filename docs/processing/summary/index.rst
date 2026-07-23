.. _processing_summary:

#######
Summary
#######

Pipeline graphs visualize the four stages of Data Release Processing (DRP).
Pipeline tasks (i.e., the code that is executed) are shown in blue boxes, while data products that are produced by the tasks are in light gray boxes.
Arrows represent the connections between tasks and data products, including inputs to tasks as well as their outputs.
Each stage finishes with all the analysis needed to validate that it is complete, and move onto the next stage.
Note that not every product in the graph is a final user-facing image or catalog (many are intermediate products).


Stage 1
=======

:doc:`/processing/isr/index` applies the input :ref:`calibration data products <calibrations>` to :ref:`raw images <images-raw>` and produces "post_isr_images."
Source detection and measurement tasks are run on these initial images, and the resulting catalogs are matched to the :doc:`/processing/calibration/monster` to derive initial single-detector calibrations.
Then, analysis is performed on the initial calibrated single-visit images in preparation for stage 2.

.. figure:: images/DP2-stage1-figure.png
  :alt: Pipeline graph of DP2 DRP stage 1, showing single visit processing steps

  **Figure 1:** Pipeline graph of DP2 DRP Stage 1, showing single visit processing steps.

:download:`Download the PDF for Stage 1 <images/DP2-stage1-figure.pdf>`.



Stage 2
=======

Multi-visit and full-visit recalibration, including :doc:`/processing/calibration/photometric`, :doc:`/processing/calibration/astrometric`, and measurement of proper motions and parallaxes for isolated stars.

.. figure:: images/DP2-stage2-figure.png
  :alt: Pipeline graph of DP2 DRP Stage 2, showing recalibration steps

  **Figure 2:** Pipeline graph of DP2 DRP Stage 2, showing recalibration steps.


:download:`Download the PDF for Stage 2 <images/DP2-stage2-figure.pdf>`.



Stage 3
=======

The coaddition of single-visit images to create :ref:`deep_coadd <images-deep-coadd>` images.
These coadds are then processed through detection, deblending, and measurement algorithms, which results in the :ref:`Object <catalogs-object>` table.
This stage also produces template coadds that are used for difference imaging.

.. figure:: images/DP2-stage3-figure.png
  :alt: Pipeline graph of DP2 DRP Stage 3, showing coaddition steps

  **Figure 3:** Pipeline graph of DP2 DRP Stage 3, showing coaddition steps.


:download:`Download the PDF for Stage 3 <images/DP2-stage3-figure.pdf>`.


Stage 4
=======

Single-visit images are reprocessed to apply the final photometric and astrometric calibrations, resulting in :ref:`visit_images <images-visit-image>`.
The :ref:`Source <catalogs-source>` catalogs are created (measurements for detections in single-visit images), and :doc:`/processing/dia/index` and :ref:`detection-forcephot` are performed.

.. figure:: images/DP2-stage4-figure.png
  :alt: Pipeline graph of DP2 DRP Stage 4, showing variability measurement steps

  **Figure 4:** Pipeline graph of DP2 DRP Stage 4, showing variability measurement steps.


:download:`Download the PDF for Stage 4 <images/DP2-stage4-figure.pdf>`.
