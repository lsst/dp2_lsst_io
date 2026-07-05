.. _access_butler:

###########################
Butler tips and terminology
###########################

The Butler is the system used to organize Rubin datasets both for users and internally, by Data Management.

`Go to the pipelines middleware documentation <https://pipelines.lsst.io/middleware/index.html>`_.


Catalog query tips
==================

**Required dimensions:** the "Butler Dataset type" entry on the page for each of the :doc:`/products/catalogs/index` is of the format ('datasetTypeName', {dimension1, **dimension2**, **dimension3**}, StorageClass), where dimensions in bold are *required* dimensions for retrieving datasets of this type.

**Efficient data retrieval:** when reading catalogs with the butler, it can be *much* more efficient to load just a few columns, by adding::

    parameters={"columns": list_of_columns}

to a `Butler.get <lsst.daf.butler.Butler.get>` call (where ``list_of_columns`` is a Python `list` of `str` column names).


Image query tips
================

**Required dimensions:** the "Butler Dataset type" entry on the page for each of the :doc:`/products/images/index` is of the format ('datasetTypeName', {dimension1, **dimension2**, **dimension3**}, StorageClass), where dimensions in bold are *required* dimensions for retrieving datasets of this type.

**Efficient data retrieval:** when reading images with the butler, it can be *much* more efficient to read just the pixels of interest, by passing::

    parameters={
        "bbox": lsst.images.Box(
            x=lsst.images.Interval(x1, x2),
            y=lsst.images.Interval(y1, y2)
        )
    }

as a keyword argument to `butler.get <lsst.daf.butler.Butler.get>`.


Datasets and dataset types
==========================

A *dataset* in the butler is a singular entity, usually corresponding to a single file and a single in-memory Python object.
For example, the ``deep_coadd`` image for a single ``{tract, patch, band}`` or an ``object`` table for a single ``{tract, patch}`` are each a dataset.

A category of datasets is a *dataset type*.
Dataset types have a name (e.g. ``deep_coadd``, ``object``) that uniquely identifies them, a *storage class*, and a set of *dimensions*.

The *storage class* connects the dataset type with both the Python type used to represent datasets in memory, and an abstract specification of how it is stored.
Storage classes can sometimes be converted on read or write.
For example, Butler dataset types stored as Parquet files are almost always defined with ``ArrowAstropy`` as their storage class, which reads in as an `astropy.table.Table` instance, but passing ``storageClass="DataFrame"`` to `Butler.get <lsst.daf.butler.Butler.get>` will make it return a `pandas.DataFrame` instead.

The *dimensions* of a dataset type are the keys of the mapping-like *data IDs* (``dataId``) that identify datasets of that type.
In addition to being used as keys in data IDs, dimensions are associated with metadata (*dimension records*) in the butler database, and can have other dimensions as *required* and *implied* dependencies that often allow data IDs to be written more concisely.
