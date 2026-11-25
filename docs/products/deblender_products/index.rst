.. _object-scarlet-models:

#####################
Object Scarlet models
#####################

An archive of Scarlet-deblended models as JSON objects.

Access
======

The ``object_scarlet_models`` data set is only accessible via the Butler service.

TAP
---

This dataset is not available via TAP.

Butler
------

* |object_scarlet_models_doi|
* Dataset type: ('object_scarlet_models', {**skymap**, **tract**, **patch**}, LsstScarletModelData)
* Format: Parquet
* Number of Butler datasets: |object_scarlet_models_butler_count|

Description
===========

The deblending of objects in the ``deep_coadd`` images is performed by the scarlet lite implementation of the scarlet deblender algorithm (`Melchior et al. 2018 <https://scixplorer.org/abs/2018A%26C....24..129M/abstract>`_). The ``object_scarlet_models`` dataset is an archive of the multi-band deblending models generated for each ``deep_coadd`` for detected objects in the ``object`` catalog. ``object_scarlet_models`` stores the parameters including the LSST bands used in the deblending process, model PSF, dictionary of blends for each parent object and other metadata required to reconstruct blend models and footprints on the fly. 

Processing
----------

Objects are first detected from the ``deep_coadd`` imaging as (possibly blended) footprints of flux with S/N > 5 and detections are merged across all available LSST
bands to identify overlapping objects. As a first step to subdivide parent blends ``DeconvolveExposureTask`` is run on each deep_coadd to partially deconvolve the images to a target Gaussian PSF with sigma=0.8, the model seeing used in scarlet lite. ``DeblendCoaddSourcesMultiTask`` then invokes ``ScarletDeblendTask``, breaks large parent footprints into smaller sub-blends using the deconvolved images,then runs scarlet lite to model each child source simultaneously across all available bands.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for notebook tutorial 206.2 on accessing deblender footprints.


