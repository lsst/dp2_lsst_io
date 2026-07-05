.. _access_tap:

#################
TAP and ADQL tips
#################

.. Important::
    If a query takes longer than expected, please report it in the `Rubin Community Forum <https://community.lsst.org/>`_.
    Rubin staff will investigate and help to optimize queries.


Use asynchronous queries
========================

In the Notebook Aspect, execute TAP queries "asynchronously".
This means submit the query job separately from fetching the results.
Where ``query`` is an ADQL statement, an asynchronous job can be run with the following code,
which will print the job phase and show the error traceback if the job did not complete.

.. code-block:: SQL

  job = service.submit_job(query)
  job.run()
  job.wait(phases=['COMPLETED', 'ERROR'])
  print('Job phase is', job.phase)
  if job.phase == 'ERROR':
      job.raise_if_error()


Then this code can be run separately to fetch the results, if the job completed.

.. code-block:: SQL

  assert job.phase == 'COMPLETED'
  results = job.fetch_result()


The RSP Portal automatically uses asynchronous mode for user queries.


Use spatial constraints
=======================

It is recommended to always include spatial constraints unless deliberately performing an all-sky analysis.
Even in that case it is strongly recommend to first prototype the query with a spatial constraint included.

Qserv stores catalog data sharded by coordinate (RA, Dec).
This can be thought of as the database being divided up by spatial region (shard) and distributed across multiple servers.
ADQL query statements that include constraints by coordinate do not requre a whole-catalog search, minimize the number of shards that have to be searched through, and are typically faster (and can be much faster) than ADQL query statements that have no (or very wide) spatial constraints, or only include constraints for other columns.

Use ADQL's spherical geometry operators to perform either a Cone Search or a Polygon Search.
These are automatically translated into efficient spatial operations in Qserv.
**Do not** use direct column constraints (e.g., ``ra < value``) or ``WHERE`` ... ``BETWEEN`` statements to set boundaries on RA and Dec.


Table indices
=============

The three columns, ``objectId``, ``diaObjectId``, and ``sourceId``, can be thought of as columns that encode information about which shard the object can be found in.
These columns appear in multiple tables (e.g., the ``objectId`` appears in the ``Object`` table and also in the ``ForcedSource`` table).
Queries that provide constraints on these columns (see below) are executed much faster, because they also minimize the number of shards that have to be accessed.


Retrieve forced photometry by identifier
========================================

The ``ForcedSource`` and ``ForcedSourceOnDiaObject`` tables must be queried by ``objectId`` and ``diaObjectId``, respectively, and **not by** coordinates.

The correct method for retrieving light curves is to first query the ``Object`` or ``DiaObject`` table with spatial constraints to obtain the ``objectId`` or ``diaObjectId`` for the target(s) of interest, and then to retrieve the forced photometry by the identifiers.
Or, both steps can be done with an ADQL table join.


Select only the necessary columns
=================================

It is recommended to only retrieve the columns that are needed for a given analysis.
Most queries should specify columns by name and should **not use** ``SELECT * FROM``.
The ``Object`` table, for example, has over 1000 columns.


Use caution if combining ORDER BY with TOP or MAXREC
====================================================

For debugging and testing queries, the recommended way to restrict the number of rows returned is to use very small spatial regions and selective column constraints instead of using the ``TOP`` or the ``MAXREC`` parameter.

The TAP service first applies ``WHERE`` constraints, then ``ORDER BY``, and then ``TOP`` or the ``MAXREC`` parameter.
If the query is not well constrained, i.e., if thousands or more objects meet the ``WHERE`` constraints, then they all must first be sorted before the subset is returned.

Combined use of ``ORDER BY`` with ``TOP`` or the ``MAXREC`` parameter in ADQL queries can be dangerous: it may take an unexpectedly long time because the database is trying to first sort, and then extract the top N elements.
It is best to only combine these ADQL functionalities if the query's ``WHERE`` statements significantly cut down the number of objects that would need to be sorted.


Avoid putting TAP queries in ``for...in`` loops
===============================================

For users that have a list of coordinates (Right Ascension, Declination) and need to cross-match them to Rubin source catalogs, it is recommended to avoid making multiple TAP queries in a ``for ... in`` loop (i.e., one TAP query for each coordinate), and instead use the TAP functionality for **table upload** and **cross-match** as demonstrated in the :doc:`/tutorials/index`.
