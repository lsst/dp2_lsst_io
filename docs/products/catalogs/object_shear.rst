.. _object-shear:

############
Object shear
############

Descriptions of shear objects detected and measured on coadds with metadetection.

Schema: `ShearObject table <https://sdm-schemas.lsst.io/dp2.html#ShearObject>`_

Access
======

The object shear table is accessible via the TAP and Butler services.

**Recommended access service:** TAP

TAP
---

* |ShearObject_doi|
* Table name: ``ShearObject``
* Columns: |ShearObject_columns|
* Rows: |ShearObject_rows|

Butler
------

* |object_shear_doi|
* Dataset type: ('object_shear_all', {**skymap**, **tract**}, ArrowAstropy)
* Format: Parquet
* Number of Butler datasets: |object_shear_butler_count|

Description
===========

Metadetection is a novel approach that combines source detection with metacalibration to produce weak lensing shear estimates that are robust to selection effects.
The ``ShearObject`` table contains measurements of objects detected and measured on coadded images with metadetection.
The metadetection algorithm involves applying small artificial shear to images of small regions of sky and performing detection on the sheared images, as well as measurements that are used to calculate a shear response (`Sheldon et al. 2020 <https://ui.adsabs.harvard.edu/abs/2020ApJ...902..138S/abstract>`_, `Sheldon et al. 2023 <https://ui.adsabs.harvard.edu/abs/2023OJAp....6E..17S/abstract>`_, `Yamamoto et al. 2025 <https://ui.adsabs.harvard.edu/abs/2025MNRAS.543.4156Y/abstract>`_).
The shape measurements are performed by Gaussian weighted moments and need to be converted into shear estimates via the response.
In addition to the shape and shear measurements, the positions, fluxes, gaussian moments, and PSF shapes are also measured.

The ``ShearObject`` table is completely independent from the ``Object`` table.

Processing
----------

The shear object table is the result of :doc:`/processing/detection/shear`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the object shear table.

