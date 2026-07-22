.. _images-raw:

############
Raw exposure
############

Unprocessed exposure from camera readout.

|raw_doi|


.. important::

   Early Data Preview 2 (EDP2) has limited image data products, deep coadd images only. The raw, visit, difference, and template images will be added in the "Full" Data Preview 2 release expected Oct-Dec 2026. See `RTN-011 <https://rtn-011.lsst.io/>`_ for details.


Access
======

The raw exposures are accessible via the Butler, SIA, and TAP services.

Butler
------

* Dataset type: ('raw', {band, **instrument**, day_obs, **detector**, group, physical_filter, **exposure**}, Exposure)
* Format: FITS
* Number of Butler datasets: |raw_butler_count|

SIA and TAP
-----------

Schema: `ObsCore table <https://sdm-schemas.lsst.io/ivoa_obscore.html>`_

IVOA calibration level: 1

Dataproduct subtype: ``lsst.raw``


Description
===========

Raw images from the camera are available exactly as they were read out of the camera.
The raw images should not be used for scientific analysis, and users should not attempt to rerun ISR.
A description of the file format can be found in `CTN-004 <https://ctn-004.lsst.io/>`_.

Processing
----------

The raw exposures are first processed with the :doc:`/processing/isr/index` pipeline.

Pixel data
----------

The raw exposures have only an image plane, with no variance or mask planes like the visit images, because those planes are a result of image processing and calibration.

Image: sky pixel data in units of ADU (analog-digital units).

Metadata
--------

The metadata for raw exposures retrieved from the Butler include information about the observation (e.g., pointing, weather) and an initial WCS estimated from the telescope boresight.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on raw exposures.

