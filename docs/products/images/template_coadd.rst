.. _images-template-coadd:

##############
Template coadd
##############

The combination of processed images with the best seeing, for a patch of sky and for each of the six LSST filters, to be used as a template for difference imaging.

|template_coadd_doi|

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

TBD

Processing
----------

The template coadd images are the result of :doc:`/processing/coaddition/index`,
and they are used in :doc:`/processing/dia/index`.

Pixel data
----------

The template coadd images have three planes of pixel data.

Image: sky pixel data in flux units of nJy.

Variance: uncertainty (noise) in the flux in units of nJy^2.

Mask: an integer bitmask of representative flag values that indicate processing status or issues.
See :doc:`/products/images/deep_coadd_mask_planes`.

Metadata
--------

The metadata for template coadd images retrieved from the Butler include a list of the input visit images, the derived PSF, and the WCS.

Tutorials
---------

TBD

