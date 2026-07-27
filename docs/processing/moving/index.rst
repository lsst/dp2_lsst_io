#############################
Solar System processing (SSP)
#############################

The DP2 Solar System products concentrate on associations between Rubin ``DiaSource`` observations and previously known objects.
They are known-object products, not a blind moving-object search or an orbit catalog derived from DP2 observations alone.
DP2 delivers the associated observations in ``SSSource`` and Rubin-derived object-level photometry and summary quantities in ``SSObject``.
It also provides three auxiliary snapshots adopted from the Minor Planet Center (MPC) PostgreSQL database.

Rubin searched for and discovered new Solar System objects during :doc:`Pre-DP2 Solar System Processing <ss_linking>`.
That campaign preceded the official DP2 Data Release processing, so objects discovered by Rubin and reported to the MPC before the association cutoff can occur in the DP2 known-object products.
DP2 does not, however, deliver a standalone catalog of Rubin discoveries.
The Prompt Processing discoveries will be described separately, and future Data Releases will deliver discovery catalogs.

DP2 provides five Solar System tables:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Table
     - Purpose
   * - :doc:`SSSource </products/catalogs/ss_source>`
     - One row per association between a ``DiaSource`` and a known object's predicted position, with selected measured and ephemeris quantities.
   * - :doc:`SSObject </products/catalogs/ss_object>`
     - One row per recovered known object, with observation summaries, per-band absolute-magnitude fits, and selected orbit-derived quantities.
   * - :doc:`mpc_orbits </products/catalogs/mpc_orbits>`
     - An MPC orbit-catalog snapshot containing orbital elements and associated fit information.
   * - :doc:`current_identifications </products/catalogs/current_identifications>`
     - An MPC mapping of primary and secondary provisional designations for the same physical object.
   * - :doc:`numbered_identifications </products/catalogs/numbered_identifications>`
     - An MPC mapping between permanent numbers and primary provisional designations.

The three MPC tables retain the upstream MPC field semantics rather than being recomputed from DP2 observations.
The delivered ``mpc_orbits`` reference product is the same 2026 March 13 MPC snapshot that was used as input to association.


.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    ss_association
    ss_linking
