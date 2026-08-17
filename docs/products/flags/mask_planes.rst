.. _flag-mask-planes:

###############################
Connection to image mask planes
###############################

.. important::

   DP2 flag descriptions and guidance for their scientific applications are still being developed and validated.

Catalog ``pixelFlags_*`` columns are derived directly from the image :ref:`mask planes <images-mask-planes>`.
Each relevant mask-plane bit set in the pixels of a source's footprint propagates to the corresponding pixel flag in the catalog.

.. note::

   **Two different naming conventions.**
   DP2 images use the new ``lsst.images`` format, which renames several mask planes relative to the legacy ``lsst.afw.image`` names (e.g. ``SAT`` → ``SATURATED``, ``CR`` → ``COSMIC_RAY``, ``INTRP`` → ``INTERPOLATED``, ``EDGE`` → ``DETECTION_EDGE``).
   You will see the **new** names when you inspect an image mask (``print(image.mask.schema)``).
   The **catalog** ``pixelFlags_*`` columns, however, are still derived from and named after the **legacy** planes (``pixelFlags_saturated``, ``pixelFlags_cr``, …).
   The table below lists all three so you can move between the image and the catalog.

Coadd mask planes and Object-catalog flags
==========================================

The mapping below is verified against the EDP2 ``deep_coadd`` (cell coadd) mask schema.
It applies to the coadd-based **Object** catalog.

.. list-table::
   :header-rows: 1
   :widths: 24 16 32 28

   * - Mask plane (``lsst.images`` name)
     - Legacy name
     - Catalog flag
     - Notes
   * - ``NO_DATA``
     - ``NO_DATA``
     - ``pixelFlags_nodata``
     - No usable data at this location; check for coverage.
   * - ``INTERPOLATED``
     - ``INTRP``
     - ``pixelFlags_interpolated`` / ``…interpolatedCenter``
     - Pixel value interpolated from neighbors.
   * - ``COSMIC_RAY``
     - ``CR``
     - ``pixelFlags_cr`` / ``…crCenter``
     - Cosmic ray on ≥1 input (interpolated over).
   * - ``SATURATED``
     - ``SAT``
     - ``pixelFlags_saturated`` / ``…saturatedCenter``
     - >10% of potential inputs saturated here; implies ``REJECTED``.
   * - ``DETECTION_EDGE``
     - ``EDGE``
     - ``pixelFlags_edge``
     - **Deprecated on the Object table** — use ``sensor_edge`` (below) for coadd edges.
   * - ``CLIPPED``
     - ``CLIPPED``
     - ``pixelFlags_clipped`` / ``…clippedCenter``
     - Probable artifact rejected in coaddition; implies ``REJECTED``.
   * - ``REJECTED``
     - ``REJECTED``
     - (no dedicated catalog flag; implies ``CLIPPED``/``INEXACT_PSF``)
     - An input visit was left out at this pixel due to masking.
   * - ``INEXACT_PSF``
     - ``INEXACT_PSF``
     - ``pixelFlags_inexact_psf`` / ``…inexact_psfCenter``
     - PSF may be inexact; covers a large area, **not** recommended as a general cut.
   * - ``DETECTED``
     - ``DETECTED``
     - (no quality flag; see ``detect_*`` columns)
     - Pixel is part of a detected source footprint.
   * - ``SENSOR_EDGE`` / ``CELL_EDGE``
     - ``SENSOR_EDGE``
     - ``pixelFlags_sensor_edge`` / ``…sensor_edgeCenter``
     - Chip/cell boundary from an input; the edge flag to use on coadds. (Non-cell coadds use ``SENSOR_EDGE``; cell coadds use ``CELL_EDGE``.)

Footprint versus center: flags without a ``Center`` suffix are set if *any* pixel in the source footprint carries the mask bit; ``Center`` flags are set only if a pixel in the central 3×3 box carries it.
For quality filtering, center flags are usually the more important because they affect the core photometry and shape.

Single-epoch and difference-image flags
=======================================

The Source, ForcedSource, DiaSource, and ForcedSourceOnDiaObject catalogs carry additional ``pixelFlags_*`` columns derived from the visit-image and difference-image mask planes — for example ``pixelFlags_bad``, ``pixelFlags_suspect``, ``pixelFlags_edge`` (all valid on these tables, unlike on the coadd Object table), and, for DiaSource, ``pixelFlags_streak`` and the injection flags ``pixelFlags_injected`` / ``pixelFlags_injected_template``.

.. important::

   EDP2 provides the coadds and the catalogs, but **not** the single-epoch ``visit_image`` or ``difference_image`` products.
   The corresponding mask-plane definitions therefore cannot yet be verified against released images, and the :ref:`visit and difference image mask planes <images-visit-mask-planes>` page is a placeholder pending the full DP2 release.
   The catalog ``pixelFlags_*`` columns listed above still exist and are usable; only the image-side plane documentation is pending.

See :ref:`deep and template coadd mask planes <images-deep-coadd-mask-planes>` for the full coadd mask-plane descriptions.
