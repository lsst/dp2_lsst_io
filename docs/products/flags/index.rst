.. _flags:

####################
Catalog flag columns
####################

Catalog tables contain extensive boolean flag columns that indicate quality issues, processing failures, or special conditions identified during Science Pipelines processing.
**These flags are provided for users to apply as quality filters based on their science requirements.**
Catalog data is delivered largely "as measured" with minimal a priori filtering, since the appropriate flag criteria depend on the specific science case.

.. toctree::
   :maxdepth: 1
   :titlesonly:

   flag_definitions
   flag_recommendations
   mask_planes


What is a flag column?
======================

A flag column is a boolean (True/False) column in a catalog table that indicates whether a specific condition, issue, or failure occurred during measurement or processing.

The key characteristics of flag columns are the following.

- Data type: Boolean (``bool`` or ``boolean`` in the schema).
- Naming convention: Most flag columns have ``flag`` in the column name (e.g., ``psfFlux_flag``, ``pixelFlags_saturated``). Some flags, such as those related to calibration (``calib_*``), do not include "flag" in their name. The naming pattern is typically ``{band}_{measurement}_flag`` for the Object table and ``{measurement}_flag`` for the single-epoch and difference-image tables.
- Interpretation: ``1`` (True) indicates the condition occurred or the measurement failed; ``0`` (False) indicates success or absence of the issue.
- Usage: Users filter out rows where critical flags are set to ``1`` to build science-quality samples, adapting the specific cuts to their science case.

The measurement algorithms that populate the catalogs typically have both a "general" failure flag that is set for any failure and one or more detailed flags that can be used to identify exactly what happened.
There is also a suite of "pixel flags" (``pixelFlags_*``) that report on the image mask planes in the neighborhood of a source or object.

Science users are expected to filter their samples using both types of flags explicitly; almost no filtering has already been performed, and appropriate flag cuts depend greatly on the science case.

.. note::

   **Which flags exist and which are meaningful depends on the table and the data release.**
   DP2 is not identical to DP1: some flags were removed, some are deprecated (e.g. several coadd Object ``pixelFlags_*``), and new flags were added (e.g. free/unforced photometry flags, new difference-image flags, and a weak-lensing ShearObject table).
   For the authoritative, per-column list, always consult the `DP2 schema browser <https://sdm-schemas.lsst.io/dp2.html>`_.

.. note::

   **Free versus forced measurements.**
   In the Object table, most fluxes are measured in "forced" mode at the reference-band position and shape so that colors are consistent across bands, while ``{band}_free_*`` columns provide independent per-band ("free") measurements.
   Apply the failure flag that matches the flux you use. See :doc:`/products/flags/flag_definitions` for details.
