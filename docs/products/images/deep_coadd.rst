.. _images-deep-coadd:

##########
Deep coadd
##########

The combination of multiple processed, calibrated, and background-subtracted images, for a patch of sky, for each of the six LSST filters.

|deep_coadd_doi|


Access
======

The deep coadd images are accessible via the Butler, SIA, and TAP services.

Butler
------

* Dataset type: ('deep_coadd', {**band**, **skymap**, **tract**, **patch**}, CellCoadd)
* Format: FITS
* Number of Butler datasets: |deep_coadd_butler_count|

SIA and TAP
-----------

Schema: `ObsCore table <https://sdm-schemas.lsst.io/ivoa_obscore.html>`_

IVOA calibration level: 3

Dataproduct subtype: ``lsst.deep_coadd``

Description
===========

The LSST all-sky map is divided into "tracts" (see :doc:`/products/maps/skymap`).
One tract is one square region of LSST's all-sky tesselation ("skymap"), approximately 1.66 deg per side, and tracts are subdivided into 100 overlapping patches.
One deep coadd image is created per patch.

Cell-based coaddition
---------------------

The deep coadds used to build the Object table are built with cell-level input selection: a ``{visit, detector}`` must fully overlap a cell to be included in that cell.
Cells are 150x150 pixels, and the PSF model for each cell is the weighted average of the PSFs of the input images, assumed constant over the cell.
These "edge-free" cell coadds eliminate detector-edge discontinuities, but there are discontinuities between cells on the coadd, and these may be more pronounced than the old discontinuities at detector edges.

Processing
----------

The deep coadd images are the result of :doc:`/processing/coaddition/index`.

Pixel data
----------

The deep coadd images have three planes of pixel data.

Image: sky pixel data in flux units of nJy.

Variance: uncertainty (noise) in the flux in units of nJy^2.

Mask: an integer bitmask of representative flag values that indicate processing status or issues.
See :doc:`/products/images/deep_coadd_mask_planes`.

Metadata
--------

The metadata for deep coadd images retrieved from the Butler include, e.g., the PSF, the WCS, and a list of input images.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on deep coadd images.

