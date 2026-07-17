.. _products_lsdb:

####
LSDB
####

LSDB is a python tool for scalable analysis of large catalogs (query and cross-match).
Built on top of `Dask <https://docs.dask.org/en/stable/>`__, LSDB uses the `HATS <https://hats.readthedocs.io/en/stable/>`__ (Hierarchical Adaptive Tiling Scheme) data format to efficiently perform spatial operations.

Why use LSDB
============

LSDB complements the TAP and Butler data access services.
Whereas TAP is designed for retrieving subsets of the catalogs, LSDB enables analyses that operate on most or all of a catalog at once.

Catalogs are opened lazily: only the schema is read at first, and data are read on demand.
Computations are parallelized with Dask, enabling queries and column-based filtering over the full catalogs.

LSDB efficiently cross-matches the DP2 catalogs against external catalogs distributed in HATS format (e.g., Gaia, ZTF, Pan-STARRS).

Lightcurves are stored as nested columns alongside each object, so per-object time-series computations (e.g., variability statistics, period finding) can be applied across the full catalog without expensive joins.

DP2 LSDB data products
======================

The DP2 catalogs are provided in HATS format as LSDB collections.
A collection groups related catalogs together (main, margin, and index catalogs), but can be opened and used just like a standard catalog — the extra catalogs simply enable operations such as cross-matching and fast lookups by identifier.

**Object collection:**
The object catalog, with the multi-band (*ugrizy*) forced-source photometry nested per object as lightcurves.

**DIA object collection:**
The DIA object catalog, with the difference-image forced photometry nested per object as lightcurves.

**Photo-z catalog:**
Photometric redshift estimates (mean, median, mode, and best) from multiple pz algorithms, provided as a HATS catalog with ``objectId``, ``ra``, and ``dec`` columns.
See :doc:`/products/user/photoz`.

Tutorials
=========

To get started with LSDB in the RSP, follow the LSDB notebook in the "Catalog access" series of the :doc:`/tutorials/index`.

For further LSDB resources, see:

* `lsdb.io/dp2 <https://lsdb.io/dp2>`__ for more information on using LSDB with DP2 data
* `data.lsdb.io <https://data.lsdb.io/>`__ for descriptions of the DP2 LSDB data products
* `docs.lsdb.io <https://docs.lsdb.io/>`__ for general LSDB documentation and tutorials

