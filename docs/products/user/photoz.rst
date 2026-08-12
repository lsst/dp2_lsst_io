.. _products_photoz:

#################
Photo-z estimates
#################

As documented in the Rubin tech note "Photometric Redshifts for Data Preview 2" (`RTN-124 <https://rtn-124.lsst.io/>`_),
members of the Rubin Commissioning Science Unit for photometric redshifts have generated photo-z estimates for galaxies in DP2.

Access to these photo-z estimates from the Rubin Science Platform is available via the :doc:`/products/user/lsdb`.

To get started with LSDB photo-z in the RSP, follow the notebook in the “310. Photometric redshifts” series of the :doc:`/tutorials/index`.


.. _products_photoz_pzserver:

Photo-z Server
==============

The `LSST Photo-z Server <https://pzserver.linea.org.br/>`_ is an online service complementary to the Rubin Science Platform (RSP).
It hosts and produces photometric redshift–related lightweight data products and provides tools for data management, sharing, and provenance tracking.
Access is granted using RSP credentials.
See the Photo-z Server `User Guide <https://docs.linea.org.br/en/sci-platforms/pz_server.html>`_ for instructions on how to use it.

The DP2 :doc:`/products/catalogs/object` catalog is available as input data for the **Training Set Maker** pipeline.
A comprehensive collection of **Reference Redshift Catalogs** (mostly spectroscopic) from the literature is also available for users to build customized training sets.

A dedicated `documentation page <https://data.linea.org.br/en/sci_products/pzserver.html#data-preview-2>`_ includes links to datasets curated by the Photo-z Server administrators, such as the DP2 preliminary photo-z data products described in `RTN-124 <https://rtn-124.lsst.io/>`_ (see the Appendices).
