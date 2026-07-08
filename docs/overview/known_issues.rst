.. _products_known_issues:

############
Known issues
############

.. important::

   This webpage contains some placeholder information from Data Preview 1 and is currently under development.


The purpose of this page is to document and provide guidance on known issues with the dataset.
Each issue should be quantified and accompanied by practical user-facing information, such as the size of the resulting uncertainties, plots, and mitigation strategies.
Facts and descriptions of data products and processing are documented on the relevant :doc:`/products/index` and :doc:`/processing/index` pages.


.. _issues_crowded_fields:

Crowded fields
==============

Deblending quality
------------------

Poor deconvolution in crowded fields leaves rings that connect large regions, resulting in blends that take up most or all of a patch.
There are missing measurements for several patches in dense stellar fields.

.. note::

   TODO (pair-documenting): quantify this issue and add practical guidance.
   Which patches/tracts and stellar densities are affected? How many objects have missing measurements? Add example images and mitigation strategies (e.g., flags or selections to identify affected regions).


PSF modeling issues in crowded fields
-------------------------------------

PSF residuals in DP2 show structure that correlates with stellar density, with the largest biases appearing in the Milky Way, the LMC, and the SMC.
This stellar-density dependence also explains observed differences in PSF residuals between bands (bands with more crowding show larger residuals).
The underlying cause is believed to be related to background subtraction in dense fields.

DP2 has an order of magnitude more visits than previous processing campaigns, revealing a new variety of PSF-related issues, including annealing patterns in E2V sensors visible in u and g-band stacks.

.. note::

   TODO (pair-documenting): quantify this issue and add practical guidance.
   What is the size of the PSF residuals as a function of stellar density and band? Add plots of the residuals and advice on the impact for PSF-sensitive science (e.g., PSF photometry and shape measurement).


.. _issues_astrometry:

Astrometry
==========

Visit-level astrometry
----------------------

Known astrometric issues include:

- Not yet understood behavior in the per-visit median offset from Gaia metrics.
- Stacking residuals in focal plane coordinates reveals unmodeled camera behavior, including tree ring signatures and other small-scale effects.

.. note::

   TODO (pair-documenting): quantify these issues.
   What is the typical size (e.g., in mas) of the per-visit median offset from Gaia and of the unmodeled small-scale residuals, and what is the impact on user astrometry?

Coadd astrometry: galaxy RA bias
--------------------------------

Galaxy Right Ascension (but not Declination) has a magnitude-dependent bias, seen when compared to external catalogs (Euclid, DES, and others) and confirmed in injection runs.
This may be related to the way the aperture used in ``SdssCentroid`` grows with source brightness.
Exponential and Sersic model centroids, which have a free centroid parameter, are generally more accurate and have a smaller bias.
This issue is tracked in `DM-54726 <https://rubinobs.atlassian.net/browse/DM-54726>`__.

.. note::

   TODO (pair-documenting): add a plot quantifying the bias as a function of magnitude so users have context, and state the size of the bias and the recommended mitigation (e.g., use Exponential or Sersic model centroids).


.. _issues_photometry:

Photometry
==========

Aperture corrections
--------------------

The aperture correction scheme used in Rubin processing (see :doc:`/processing/calibration/photometric`) has several known problems:

- These aperture corrections are well-defined for point sources only, but they are still applied for most of the galaxy-focused photometry algorithms (the ``sersic_*`` fluxes are the sole exception), since this at least makes them well-calibrated for poorly-resolved galaxies.

- Coadding apertures with the same weights as the images is only correct in the limit that the images have the same PSF.
  For fixed-aperture photometry a different combination should be used (and will be used in future data releases, if this scheme is used at all), and for PSF-dependent photometry no formally correct combination is possible.

- Ratios of fluxes on even bright stars can be very noisy, and in some cases the aperture correction is a significant fraction of the error budget.

.. note::

   TODO (pair-documenting): quantify the end-user impact for each problem.
   For each affected flux measure, state the size of the effect (e.g., "when using flux X, the issue is Y and the effect is Z%").

Photometric calibration: background oversubtraction
----------------------------------------------------

Photometrically-derived background offset QA plots reveal systematic oversubtraction in all bands, worsening toward redder bands and in the densest regions.
DP2 includes many dense regions, making this effect more pronounced.
This oversubtraction can affect photometry and PSF estimation in crowded fields.

.. note::

   TODO (pair-documenting): quantify what the user needs to care about.
   What is the size of the oversubtraction per band and as a function of density, what is the impact on fluxes (e.g., in mmag), and how can affected regions be identified?

Aperture flux uncertainties
---------------------------

The ``{band}_ap12FluxErr`` column in the Object table is sometimes NaN even when the uncertainty for larger apertures (e.g., ``{band}_ap17FluxErr``) is finite.
This is likely a problem with the sinc interpolation used for smaller apertures, and may also affect ``apFlux_12_0_instFluxErr`` in the Source table.
This issue is tracked in `DM-54658 <https://rubinobs.atlassian.net/browse/DM-54658>`__.

Sersic and Exponential model outputs
------------------------------------

Improvements to the Sersic and Exponential model outputs are tracked in `DM-53939 <https://rubinobs.atlassian.net/browse/DM-53939>`__.

.. note::

   TODO (pair-documenting): a full description of the ellipse parameterizations and units is owed on the :doc:`/products/catalogs/object` page.


.. _issues_backgrounds:

Background subtraction
=======================

The ``SkyCorrectionTask`` that performs full-focal-plane background correction is not designed to accommodate detector-to-detector offsets in the input data.
These offsets arise from several sources, including E2V/ITL sensor differences, offset detectors, and offset REBs, and are more pronounced in the u and y bands.
A new full-focal-plane background fitter is under development but was not ready for DP2.

.. note::

   TODO (pair-documenting): add more information.
   What is the typical size of the detector-to-detector offsets, which data are most affected, and what is the impact on user photometry and surface brightness measurements?


.. _issues_coadds:

Coadds
======

Empty cells
-----------

Coadd cells will have no data if none of the input warps is below the masked-pixel threshold, even though a fallback (such as including the warp with the largest mask fraction) could have been implemented.
This is not so much a bug as a case where the best solution has not yet been chosen.

.. note::

   TODO (pair-documenting): quantify how often this occurs and add guidance on identifying affected cells.


.. _issues_object_catalog:

Object catalog
==============

Objects spanning multiple cell footprints
-----------------------------------------

There is no column to identify objects whose cells span multiple footprints, and which therefore effectively have ``INEXACT_PSF`` for their measurements.
What to do about this has not yet been decided.

Duplicate or missing objects near patch boundaries
--------------------------------------------------

A tiny fraction of objects with centroids near patch boundaries may appear in the Object table zero times (if their reference-band centroids happen to shift outside the inner area of every patch) or multiple times (up to four).

Similarly, the ``exponential_ra``/``exponential_dec`` and ``sersic_ra``/``sersic_dec`` centroids may not correspond to the same patch/tract as ``coord_ra``/``coord_dec``.

Matching to the object_shear_all table
--------------------------------------

The ``object_shear_all`` table has a completely different set of rows from the Object table: it includes rows with ``is_tract_inner == False``, but not rows with ``is_patch_inner == False``.
Users will need to be careful when comparing or matching the two tables.


.. _issues_dia:

Difference imaging
===================

DIA source reliability
----------------------

The reliability model for DIA sources shows improved performance relative to DP1, but does not perform well on bad stellar subtractions.

.. note::

   TODO (pair-documenting): add details on what "does not perform well" means.
   What reliability values do bad stellar subtractions receive, what threshold is recommended for clean transient samples, and what fraction of DIA sources is affected?

Diffraction spike masks and bright-star halos
---------------------------------------------

The ``SPIKE`` masks are excessively wide at the bases.
In practice, where the bases intersect they mask out a polygon that is typically (possibly always) larger than the saturated (``SAT``) mask.

Despite the large spike masks, there is usually some residual flux from the halos of bright stars, beyond where the ``SPIKE`` masks end, that is not subtracted off.
Detections in these regions are more likely to be bogus, and even if real, the measurements are likely unreliable.
In the simplest case, where the diffraction spikes all line up, these detections are clustered around the four points where the spike-mask triangles intersect, making a square/diamond shape around the center of the star.


.. _issues_solarsystem:

Solar system processing
========================

Three-night discovery candidates are not sufficiently pure (approximately 1 in 500), while four-night candidates achieve much higher purity (approximately 1 in 10,000).

.. TODO (pair-documenting): confirm whether the previously-reported 30 mas astrometric bias
   for solar system objects (compared to orbit catalogs, with a dependence on asteroid
   properties but not on observation parameters) still exists. It is believed this was
   chased down (Jake) and Rubin astrometry was correct, i.e., there is no bias.
   Re-add here with quantification only if confirmed to be real.
