.. _images:

######
Images
######

Images of the sky in the six LSST filters with a variety of calibration levels.

The `schema browser <https://sdm-schemas.lsst.io/>`_ includes tables of image metadata (``ObsCore``).

.. important::

   Early Data Preview 2 (EDP2) has limited image data products, deep coadd images only. The raw, visit, difference, and template images will be added in the "Full" Data Preview 2 release expected Oct-Dec 2026. The metadata and source measurements for all images are available in the catalogs. See `RTN-011 <https://rtn-011.lsst.io/>`_ for details.

Image data products are available via the butler, SIA, and TAP services.
See the :doc:`/access/index` and the :doc:`/tutorials/index` pages to get started with these services.

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    new_lsst_image_format


Coadd and template images
=========================

Combinations of multiple calibrated images of the same region of the sky to achieve
greater depth (to detect fainter objects),
or for use as templates in difference image analysis.


.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    deep_coadd
    template_coadd



Visit and difference images
===========================

Individual observations are processed and calibrated to produce visit images, and templates are subtracted to produce difference images.

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    visit_image
    difference_image


Mask planes
===========

Image mask planes.

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    mask_planes


Raw exposures
=============

The unprocessed images received directly from the camera.

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    raw_exposure


