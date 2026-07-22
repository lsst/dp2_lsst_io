.. _images-visit-image:

###########
Visit image
###########

Individual processed and calibrated sky images.

|visit_image_doi|


.. important::

   Early Data Preview 2 (EDP2) has limited image data products, deep coadd images only. The raw, visit, difference, and template images will be added in the "Full" Data Preview 2 release expected Oct-Dec 2026. The metadata and source measurements for all images are available in the catalogs. See `RTN-011 <https://rtn-011.lsst.io/>`_ for details.


Access
======

The visit images are accessible via the Butler, SIA, and TAP services.

Butler
------

* Dataset type: ('visit_image', {band, **instrument**, day_obs, **detector**, physical_filter, **visit**}, VisitImage)
* Format: FITS
* Number of Butler datasets: |visit_image_butler_count|

SIA and TAP
-----------

Schema: `ObsCore table <https://sdm-schemas.lsst.io/ivoa_obscore.html>`_

IVOA calibration level: 2

Dataproduct subtype: ``lsst.visit_image``


Description
===========

Raw images from the camera undergo processing that includes instrument signature removal (ISR), image calibration (photometric and astrometric), and PSF estimation.
The result is a fully calibrated visit image.

Each individual visit image contains data from one of the camera's detectors.
Visit images are rotated, and are not aligned north-up east-left, such that ``RA, Dec`` do not correspond to ``x, y``.

Processing
----------

The visit images are the result of :doc:`/processing/calibration/index`.

Pixel data
----------

The visit images have three planes of pixel data.

Image: Sky pixel data in flux units of nJy.

Variance: Uncertainty (noise) in the flux in units of nJy^2.

Mask: An integer bitmask of representative flag values that indicate processing status or issues.
See :doc:`/products/images/visit_image_mask_planes`.


Metadata
--------

The metadata for visit images retrieved from the Butler include the derived PSF and the WCS.

Pixel scale
"""""""""""

The pixel scale in a visit image varies with position.
To use the pixel scale it is recommended to extract it using the "getPixelScale" method associated with the visit images' WCS.
This function should *always* be given an argument with x, y coordinates, so that a typical call (for an x, y position of 1000, 1400) may look like: ``wcs.getPixelScale(lsst.geom.Point2D(1000, 1400)).asArcseconds()`` (where "wcs" is the WCS associated with a visit image, the "Point2D" function comes from the "lsst.geom" package, and the final portion converts the result to arcseconds).

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on visit images.


