#############################
Solar System processing (SSP)
#############################

The DP2 Solar System processing and data products concentrate on associations between Rubin difference image detections (``DiaSources``) and previously known small bodies, and do _not_ include a blind moving object search or catalogs derived from Rubin observations alone. For more information, see :doc:`Solar System source detection and association <ss_association>`.

Rubin searched for and discovered new Solar System objects during a period of pre-DP2 :doc:`small body tracklet linking and orbit fitting <ss_linking>`.
That campaign preceded the official DP2 release processing, so that objects discovered by Rubin and reported to the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC) before the association cutoff for DP2 processing occurred, were able to be included in the DP2 known/associated object data products.
DP2 does not, however, deliver a standalone catalog of Rubin discoveries; these Rubin-discovery-only catalogs will be delivered in future Data Releases.
The `Solar System Prompt Processing <https://prompt-products.lsst.io/processing/moving/ss_prompt.html>`_ discoveries will be described once they are made available by Rubin. Currently, all Solar System objects discovered by Rubin get reported to and are made available via the `Minor Planet Center <https://minorplanetcenter.net/>`_ and the `B612 Foundation's Asteroid Institute <https://b612.ai/rubin-mpc-downloads/>`_ (see `Prompt Products Data Access <https://prompt-products.lsst.io/access/index.html#minor-planet-center-mpc`_ for more information).

The DP2 data products deliver the associations between Rubin difference image detections (``DiaSources``) and previously known small bodies in ``SSSource`` and Rubin-derived object-level photometry and summary quantities in ``SSObject``. It also provides three auxiliary snapshots adopted from the `Minor Planet Center (MPC) PostgreSQL database <https://data.minorplanetcenter.net/mpcops/documentation/replicated-tables-schema/>`_ (see :doc:`DP2 catalogs <products/catalogs/index>` for details):

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

The three MPC tables retain the upstream MPC field semantics rather than being recomputed from Rubin DP2 observations.
The delivered ``mpc_orbits`` reference product is the same 2026 March 13 MPC orbit snapshot that was used as input to association.


.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    ss_association
    ss_linking
