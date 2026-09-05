.. _images-deep-coadd-mask-planes:

Deep and template coadd mask planes
===================================

This page documents the mask planes carried by DP2 coadd images.
Each plane corresponds to a bit in the coadd mask and reflects the propagation or summary of pixel conditions from the contributing single-visit exposures.

.. note::

   DP2 images use the new ``lsst.images`` format, in which the mask-plane names and descriptions are stored in the mask's ``schema`` and read at runtime.
   Several plane names differ from the legacy ``lsst.afw.image`` names used in DP1: ``SAT`` → ``SATURATED``, ``CR`` → ``COSMIC_RAY``, ``INTRP`` → ``INTERPOLATED``, ``EDGE`` → ``DETECTION_EDGE``, and ``UNMASKEDNAN`` → ``UNMASKED_NAN``.
   The names below are the current ``lsst.images`` names; the legacy name is given in parentheses where it differs, since the catalog ``pixelFlags_*`` columns are still derived from the legacy names (see :doc:`/products/flags/mask_planes`).

Deep coadd (cell coadd) mask planes
-----------------------------------

The Early Data Preview 2 (EDP2) ``deep_coadd`` images are **cell-based coadds**.
The planes below are those present in the EDP2 ``deep_coadd`` mask schema, in bit order.
The pipeline enforces the implication chain ``SATURATED`` ⇒ ``REJECTED`` ⇒ ``INEXACT_PSF``, and ``CLIPPED`` ⇒ ``REJECTED``.

``NO_DATA``
    No data were available for this pixel.
    Common at tract/patch edges or in areas not covered by any input visit, and also where all inputs were masked (``SATURATED`` is often a reason for ``NO_DATA``).
    These pixels should be ignored in analysis.

``INTERPOLATED`` (legacy ``INTRP``)
    The pixel value is the result of interpolating nearby good pixels.

``COSMIC_RAY`` (legacy ``CR``)
    A cosmic ray affected this pixel on at least one input image (and was interpolated over).

``SATURATED`` (legacy ``SAT``)
    More than 10% of the potential input visits had a saturated pixel at this location ("potential" because saturated pixel values are not actually propagated to the coadd).
    ``SATURATED`` always implies ``REJECTED``, and is often a reason for ``NO_DATA``.

``DETECTION_EDGE`` (legacy ``EDGE``)
    Pixel was too close to the edge of the patch to be considered for detection, due to the finite size of the detection kernel.

    .. note::

       For quality selection on the **Object catalog**, the corresponding coadd-edge flag to use is ``pixelFlags_sensor_edge`` (derived from the ``SENSOR_EDGE`` plane on non-cell/template coadds); the catalog ``pixelFlags_edge`` column (from ``DETECTION_EDGE``) is deprecated on the Object table. See :doc:`/products/flags/flag_definitions`.

``CLIPPED``
    The region was identified as a probable artifact when comparing multiple single-visit warps and was excluded from the coadd at this pixel.
    ``CLIPPED`` always implies ``REJECTED``.

``REJECTED``
    At least one input visit was left out of the coadd for this pixel due to masking.
    ``REJECTED`` always implies ``INEXACT_PSF``.

``DETECTED``
    Pixel was part of a detected source footprint on the coadd.

``INEXACT_PSF``
    The set of visits contributing to this pixel differs from the set of visits contributing to the PSF model for its cell, so the PSF at this pixel may be inexact.
    Because ``REJECTED`` implies ``INEXACT_PSF``, this bit covers a large fraction of the coadd and is not recommended as a general quality cut (see :doc:`/products/flags/flag_definitions`).

Template (non-cell) coadd mask planes
-------------------------------------

Template coadds and other non-cell (``assemble_coadd``) coadds carry the visit-level planes propagated from their input warps in addition to the coadd-specific planes above, and flag chip edges with ``SENSOR_EDGE``.

.. important::

   The additional planes in this subsection are taken from the ``lsst.images`` pipeline definitions.
   They have not been verified against released EDP2 data (EDP2 serves the ``deep_coadd`` cell coadds); treat this list as provisional pending the full DP2 release.

``BAD``
    Bad pixel in the instrument, including bad amplifiers.

``SUSPECT``
    Pixel was close to the saturation level (above the PTC turnoff but not fully saturated). Unlike ``SATURATED``, ``SUSPECT`` is not dilated.

``CROSSTALK``
    Pixel was affected by crosstalk and corrected accordingly.

``DETECTED_NEGATIVE``
    Pixel was part of a detected source with negative flux.

``NOT_DEBLENDED``
    Pixel belonged to a detection that was not deblended, usually due to size limits.

``UNMASKED_NAN`` (legacy ``UNMASKEDNAN``)
    Pixel was found to be NaN unexpectedly (a safety net for unexpected invalid values).

``SENSOR_EDGE``
    Pixel is near the edge of a contributing sensor/chip, so the coadd PSF is discontinuous there.
