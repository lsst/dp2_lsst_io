#############
Data products
#############

Descriptions and schema for the science-ready data products.

.. important::

   Early Data Preview 2 (EDP2) has limited image data products, deep coadd images only, but has all catalogs including measurements made on the processed visit and difference images. It also has all the maps, supplementary information, and user-generated products listed below. See `RTN-011 <https://rtn-011.lsst.io/>`_ for details.


Catalogs
========

Tables of measurements made on detected sources, plus observational metadata.

.. toctree::
    :maxdepth: 1
    :glob:

    catalogs/index

Images
======

Individual, difference, and coadded images of the sky in the six LSST filters.

.. toctree::
    :maxdepth: 1
    :glob:

    images/index


Maps
====

All-sky maps of deep coadded images, survey depth, image quality, and more.

.. toctree::
    :maxdepth: 1
    :glob:

    maps/skymap
    maps/sp_maps
    maps/hips


Supplementary information
=========================

Additional data products and information.

.. toctree::
    :maxdepth: 1
    :glob:

    calibrations/index
    bandpasses/index
    filter_transformations/index
    time_measurements/index
    flags/index


User-generated products
=======================

User-generated data products for DP2, shared with all Rubin data rights holders via the RSP.

.. toctree::
    :maxdepth: 1
    :glob:

    user/lsdb.rst
    user/photoz.rst
