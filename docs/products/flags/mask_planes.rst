.. _flag-mask-planes:

###############################
Connection to image mask planes
###############################

Catalog ``pixelFlags_*`` columns are derived directly from the image :ref:`mask planes <images-mask-planes>`.
Each relevant mask-plane bit set in the pixels of a source's footprint propagates to the corresponding pixel flag in the catalog.

**New for DP2:**
Several mask plane names have been updated to be more descriptive; the legacy (old) and new mask names are listed below.
Note that the catalog ``pixelFlags_*`` columns use the legacy names.

* ``SAT`` → ``SATURATED``
* ``CR`` → ``COSMIC_RAY``
* ``INTRP`` → ``INTERPOLATED``
* ``EDGE`` → ``DETECTION_EDGE``


Coadd image mask planes
=======================

The mapping below connects the mask planes for the deep coadd images and the Object catalog, which contains detections and measurements in the deep coadd images.

**Key points:**
Flags without a ``Center`` suffix are set if *any* pixel in the source footprint carries the mask bit; ``Center`` flags are set only if a pixel in the central 3×3 box carries it.
For quality filtering, center flags are usually the more important because they affect the core photometry and shape.

**New for DP2:**
For cell-based coadds, pixels at the edge of the input detector images do not contribute to the coadded image.
The relevant coadd mask plane (``DETECTION_EDGE``) and corresponding Object catalog flags (e.g., ``pixelFlags_edge``) are not included in the table below.


.. list-table::
   :header-rows: 1
   :widths: 24 16 32 28

   * - Mask plane (new and legacy)
     - Object catalog flag
     - Notes
   * - | ``NO_DATA``
       | ``NO_DATA``
     - ``pixelFlags_nodata``
     - No usable data at this location; check for coverage.
   * - | ``INTERPOLATED``
       | ``INTRP``
     - ``pixelFlags_interpolated``
     - Pixel value interpolated from neighbors, anywhere in the footprint.
   * - | ``INTERPOLATED``
       | ``INTRP``
     - ``pixelFlags_interpolatedCenter``
     - Interpolated pixel in the central 3×3 box.
   * - | ``COSMIC_RAY``
       | ``CR``
     - ``pixelFlags_cr``
     - Cosmic ray on ≥1 input (interpolated over), anywhere in the footprint.
   * - | ``COSMIC_RAY``
       | ``CR``
     - ``pixelFlags_crCenter``
     - Cosmic ray in the central 3×3 box.
   * - | ``SATURATED``
       | ``SAT``
     - ``pixelFlags_saturated``
     - >10% of potential inputs saturated here; implies ``REJECTED``.
   * - | ``SATURATED``
       | ``SAT``
     - ``pixelFlags_saturatedCenter``
     - Saturation in the central 3×3 box.
   * - | ``CLIPPED``
       | ``CLIPPED``
     - ``pixelFlags_clipped``
     - Probable artifact rejected in coaddition; implies ``REJECTED``.
   * - | ``CLIPPED``
       | ``CLIPPED``
     - ``pixelFlags_clippedCenter``
     - Clipping occurred in the central 3×3 box.
   * - | ``REJECTED``
       | ``REJECTED``
     - (no dedicated catalog flag)
     - An input visit was left out at this pixel due to masking; implied by ``CLIPPED`` and ``INEXACT_PSF``.
   * - | ``INEXACT_PSF``
       | ``INEXACT_PSF``
     - ``pixelFlags_inexact_psf``
     - PSF may be inexact; covers a large area, **not** recommended as a general cut.
   * - | ``INEXACT_PSF``
       | ``INEXACT_PSF``
     - ``pixelFlags_inexact_psfCenter``
     - Inexact PSF in the central 3×3 box.
   * - | ``DETECTED``
       | ``DETECTED``
     - (no quality flag; see ``detect_*`` columns)
     - Pixel is part of a detected source footprint.


Visit and difference image mask planes
======================================

The mask planes for the visit and difference images are connected with the Source, ForcedSource, DiaSource, and ForcedSourceOnDiaObject catalogs.

These catalogs contain ``pixelFlags_*`` columns derived from the visit-image and difference-image mask planes — for example ``pixelFlags_bad``, ``pixelFlags_suspect``, ``pixelFlags_edge``.

For DiaSource, there are additional derived columns such as ``pixelFlags_streak``.
