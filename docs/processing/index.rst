.. _processing:

###############
Data processing
###############

.. important::

   Data Preview 2 has not yet been released. This website is currently under development.


A high-level overview of the Data Release Processing (DRP) steps which generated the data products.
All processing was done with the `LSST Science Pipelines <https://pipelines.lsst.io/>`_.

For processing details see |dp2_paper| [:download:`BibTeX </bib/paper.bib>`].


Summary
=======

A summary of the DRP processing stages.

.. toctree::
    :maxdepth: 1
    :glob:

    summary/index


Instrument signature removal (ISR)
==================================

Corrects the raw images for the effects of the telescope and detector.

.. toctree::
    :maxdepth: 1
    :glob:

    isr/index


Calibration
===========

Generates the science-ready processed visit images.

.. toctree::
    :maxdepth: 1
    :glob:

    calibration/index


Coaddition
==========

Generates the deep coadd and template images.

.. toctree::
    :maxdepth: 1
    :glob:

    coaddition/index


Source detection and measurement
================================

Generates catalogs of detected sources and measurements of their properties.

.. toctree::
    :maxdepth: 1
    :glob:

    detection/index


Difference image analysis
=========================

Runs image subtraction to generate difference images and associated detection catalogs.

.. toctree::
    :maxdepth: 1
    :glob:

    dia/index


Moving objects processing
=========================

Links detected sources into moving objects and generates Solar System catalogs.

.. toctree::
    :maxdepth: 1
    :glob:

    moving/index

