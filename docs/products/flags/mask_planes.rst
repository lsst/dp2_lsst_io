.. _flag-mask-planes:

###############################
Connection to image mask planes
###############################

Catalog ``pixelFlags_*`` columns are derived directly from the image :ref:`mask planes <images-mask-planes>`.
Each relevant mask-plane bit set in the pixels of a source's footprint propagates to the corresponding pixel flag in the catalog.

.. note::

   **Two different naming conventions.**
   DP2 images use the new ``lsst.images`` format, which renames several mask planes relative to the legacy ``lsst.afw.image`` names (e.g. ``SAT`` → ``SATURATED``, ``CR`` → ``COSMIC_RAY``, ``INTRP`` → ``INTERPOLATED``, ``EDGE`` → ``DETECTION_EDGE``).
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
     - ``pixelFlags_interpolated``
     - Pixel value interpolated from neighbors, anywhere in the footprint.
   * - ``INTERPOLATED``
     - ``INTRP``
     - ``pixelFlags_interpolatedCenter``
     - Interpolated pixel in the central 3×3 box.
   * - ``COSMIC_RAY``
     - ``CR``
     - ``pixelFlags_cr``
     - Cosmic ray on ≥1 input (interpolated over), anywhere in the footprint.
   * - ``COSMIC_RAY``
     - ``CR``
     - ``pixelFlags_crCenter``
     - Cosmic ray in the central 3×3 box.
   * - ``SATURATED``
     - ``SAT``
     - ``pixelFlags_saturated``
     - >10% of potential inputs saturated here; implies ``REJECTED``.
   * - ``SATURATED``
     - ``SAT``
     - ``pixelFlags_saturatedCenter``
     - Saturation in the central 3×3 box.
   * - ``CLIPPED``
     - ``CLIPPED``
     - ``pixelFlags_clipped``
     - Probable artifact rejected in coaddition; implies ``REJECTED``.
   * - ``CLIPPED``
     - ``CLIPPED``
     - ``pixelFlags_clippedCenter``
     - Clipping occurred in the central 3×3 box.
   * - ``REJECTED``
     - ``REJECTED``
     - (no dedicated catalog flag)
     - An input visit was left out at this pixel due to masking; implied by ``CLIPPED`` and ``INEXACT_PSF``.
   * - ``INEXACT_PSF``
     - ``INEXACT_PSF``
     - ``pixelFlags_inexact_psf``
     - PSF may be inexact; covers a large area, **not** recommended as a general cut.
   * - ``INEXACT_PSF``
     - ``INEXACT_PSF``
     - ``pixelFlags_inexact_psfCenter``
     - Inexact PSF in the central 3×3 box.
   * - ``DETECTED``
     - ``DETECTED``
     - (no quality flag; see ``detect_*`` columns)
     - Pixel is part of a detected source footprint.

.. warning::

   **Omitted from the table above: flags deprecated in the Object table.**
   The ``DETECTION_EDGE`` plane (legacy ``EDGE``) maps to the catalog column ``pixelFlags_edge``, which is deprecated on the coadd Object table.
   Use ``pixelFlags_sensor_edge`` / ``pixelFlags_sensor_edgeCenter`` for coadd edges instead.
   ``pixelFlags_edge`` remains valid on the Source, ForcedSource, DiaSource, and ForcedSourceOnDiaObject tables.
   The same applies to ``pixelFlags_bad``, ``pixelFlags_suspect`` / ``pixelFlags_suspectCenter``, and ``pixelFlags_offimage``.

.. note::

   The ``SENSOR_EDGE`` mask plane is **not** present in the EDP2 ``deep_coadd`` (cell coadd) mask schema — it is a non-cell/template-coadd plane — so the image-side provenance of the ``pixelFlags_sensor_edge`` / ``pixelFlags_sensor_edgeCenter`` catalog columns for the cell coadds is not verified here.

Footprint versus center: flags without a ``Center`` suffix are set if *any* pixel in the source footprint carries the mask bit; ``Center`` flags are set only if a pixel in the central 3×3 box carries it.
For quality filtering, center flags are usually the more important because they affect the core photometry and shape.

Single-epoch and difference-image flags
=======================================

The Source, ForcedSource, DiaSource, and ForcedSourceOnDiaObject catalogs carry additional ``pixelFlags_*`` columns derived from the visit-image and difference-image mask planes — for example ``pixelFlags_bad``, ``pixelFlags_suspect``, ``pixelFlags_edge`` (all valid on these tables, unlike on the coadd Object table), and, for DiaSource, ``pixelFlags_streak`` and the injection flags ``pixelFlags_injected`` / ``pixelFlags_injected_template``.

See :ref:`deep and template coadd mask planes <images-deep-coadd-mask-planes>` for the full coadd mask-plane descriptions.
