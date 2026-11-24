.. _object-scarlet-models:

#####################
Object Scarlet models
#####################

An archive of Scarlet-deblended models as JSON objects.

Access
======

The `object_scarlet_models` data set is only accessible via the Butler service.

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

`object_scarlet_models` is an archive of the multi-band deblending models generated for each `deep_coadd` for detected objects in the `object` catalog. `object_scarlet_models` stores the parameters including the LSST bands used in the deblending process, model PSF, dictionary of blends for each parent object and other metadata required to reconstruct blend models and footprints on the fly. 

Processing
----------

TBD

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for notebook tutorial 206.2 on accessing deblender footprints.


