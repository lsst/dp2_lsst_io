.. _images-visit-mask-planes:

Visit and difference image mask planes
======================================

.. important::

   This page is not yet available for DP2.

   The Early Data Preview 2 (EDP2) release contains the coadded images (cell-based coadds) and the catalogs, but **not** the single-epoch ``visit_image`` or ``difference_image`` products.
   Because those images are not part of EDP2, their mask-plane definitions have not been verified against released data and are not documented here yet.

   This page will be populated when the visit and difference images are released with the full DP2.
   In the meantime, see the :ref:`deep and template coadd mask planes <images-deep-coadd-mask-planes>` page for the mask planes carried by the EDP2 coadds, and the :doc:`/products/flags/mask_planes` page for how mask planes connect to the catalog ``pixelFlags_*`` columns.

.. note::

   DP2 images use the new ``lsst.images`` format, in which the mask-plane names and descriptions are stored in the mask's ``schema`` and read at runtime (e.g. ``print(image.mask.schema)``).
   Several plane names differ from the legacy ``lsst.afw.image`` names (for example ``SAT`` → ``SATURATED``, ``CR`` → ``COSMIC_RAY``, ``INTRP`` → ``INTERPOLATED``, ``EDGE`` → ``DETECTION_EDGE``).
   The authoritative definitions live in the ``lsst.images`` package.
