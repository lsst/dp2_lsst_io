.. _images-template-coadd:

##############
Template coadd
##############

The combination of processed images with the best seeing, for a patch of sky and for each of the six LSST filters, to be used as a template for difference imaging.

|template_coadd_doi|


.. important::

   Early Data Preview 2 (EDP2) has limited image data products, deep coadd images only. The raw, visit, difference, and template images will be added in the "Full" Data Preview 2 release expected Oct-Dec 2026. The metadata and source measurements for all images are available in the catalogs. See `RTN-011 <https://rtn-011.lsst.io/>`_ for details.


Access
======

The template coadd images are accessible via the Butler, SIA, and TAP services.

Butler
------

* Dataset type: ('template_coadd', {**band**, **skymap**, **tract**, **patch**}, ExposureF)
* Format: FITS
* Number of Butler datasets: |template_coadd_butler_count|

SIA and TAP
-----------

Schema: `ObsCore table <https://sdm-schemas.lsst.io/ivoa_obscore.html>`_

IVOA calibration level: 3

Dataproduct subtype: ``lsst.template_coadd``


Description
===========

The LSST all-sky map is divided into "tracts".
One tract is one square region of LSST's all-sky tesselation ("skymap"), $~1.66$ deg per side.
Tracts are subdivided into 100 overlapping patches, and one template coadd image is created per patch.

Processing
----------

The template coadd images are the result of :doc:`/processing/coaddition/index`, and they are used in :doc:`/processing/dia/index`.

Pixel data
----------

The template coadd images have three planes of pixel data.

Image: sky pixel data in flux units of nJy.

Variance: uncertainty (noise) in the flux in units of nJy^2.

Mask: an integer bitmask of representative flag values that indicate processing status or issues.
See :doc:`/products/images/deep_coadd_mask_planes`.

Metadata
--------

The metadata for template coadd images retrieved from the Butler include, e.g., the PSF, the WCS, and a list of input images.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on template coadd images.

