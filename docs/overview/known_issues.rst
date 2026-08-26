.. _products_known_issues:

############
Known issues
############


For questions related to any of the issues listed on this page, please ask in the `Support category on community.lsst.org <https://community.lsst.org/c/support>`_, where Rubin staff will follow up.


.. _issues_crowded_fields:

Crowded fields
==============

Deblending quality
------------------

Poor deconvolution in crowded fields leaves rings that connect large regions, resulting in blends that take up most or all of a patch.
There are missing measurements for several patches in dense stellar fields.

PSF modeling in crowded fields is also affected; see :ref:`issues_psf`.


.. _issues_psf:

PSF modeling
============

A complete analysis of the DP2 PSF characterization will be presented in a forthcoming technote ("PSF Characterization using DP2", in preparation); see also the DP2 paper (`RTN-115 <https://rtn-115.lsst.io/>`__).
The key findings are:

- The PSF shape correlates with height variations across the focal plane (`RTN-108 <https://rtn-108.lsst.io/>`__).
  This is why ITL sensors are harder to model than E2V sensors, and why a fourth-order model is warranted.
- The LSSTCam PSF is chromatic, in a way that appears consistent with differential chromatic refraction (`SITCOMTN-174 <https://sitcomtn-174.lsst.io/>`__).
  This can be modeled, following the approach of the DES Y6 PSF analysis, but the modeling was not enabled for DP2 processing.
- With an order of magnitude more visits than previous campaigns, DP2 reveals new PSF-related artifacts, including annealing patterns in E2V sensors (visible in the u- and g-band stacks) and fringing in the z and y bands, all expected to arise from background subtraction.
- PSF residuals show structure that correlates with stellar density, with the largest biases in the Milky Way, the LMC, and the SMC.
  This also explains the band-to-band differences in the residuals, since more crowded bands show larger residuals.
  The underlying cause is again believed to be related to background subtraction in dense fields.


.. _issues_astrometry:

Astrometry
==========

Visit-level astrometry
----------------------

Known visit-level astrometric effects include:

- Not yet understood behavior in the per-visit median offset from Gaia (the ``AA1`` metric).
- Stacking residuals in focal plane coordinates reveals unmodeled camera behavior, including tree ring signatures and other small-scale effects.

These effects are all on the scale of approximately 1-2 mas, and are illustrated in the figures below.
This part of the analysis is expected to improve in future releases (for a description of the astrometric calibration, see the DP2 paper, `RTN-115 <https://rtn-115.lsst.io/>`__).
In addition, differential chromatic refraction (DCR) is not yet corrected for in the astrometric calibration.

.. figure:: images/astrometry-aa1.png
   :alt: Sky map of the per-visit median astrometric offset from Gaia in Right Ascension for the i band.

   The ``AA1`` metric (per-visit median astrometric offset relative to Gaia) in Right Ascension, in mas, for the i band across the DP2 footprint.

.. list-table::
   :widths: 50 50

   * - .. figure:: images/astrometry-focal-plane-x.png
          :alt: Focal plane map of stacked astrometric residuals in x.

          Stacked astrometric residuals in focal plane coordinates (x component, in mas), revealing unmodeled camera behavior such as tree ring signatures.
     - .. figure:: images/astrometry-focal-plane-y.png
          :alt: Focal plane map of stacked astrometric residuals in y.

          Stacked astrometric residuals in focal plane coordinates (y component, in mas).

Coadd astrometry: galaxy RA bias
--------------------------------

Galaxy Right Ascension (but not Declination) has a magnitude-dependent bias, seen when compared to external catalogs (Euclid, DES, and others) and confirmed in injection runs.
This may be related to the way the aperture used in ``SdssCentroid`` grows with source brightness.
Exponential and Sersic model centroids, which have a free centroid parameter, are generally more accurate and have a smaller bias.


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

Photometric calibration: background oversubtraction
----------------------------------------------------

Photometrically-derived background offset QA plots reveal systematic oversubtraction in all bands, worsening toward redder bands and in the densest regions.
DP2 includes many dense regions, making this effect more pronounced.
This oversubtraction can affect photometry and PSF estimation in crowded fields.

Absolute calibration: AB offsets
--------------------------------

DP2 provides photometry that is spatially and temporally uniform to an exceedingly high degree (< 2 milli-mags RMS) across the DP2 footprint, meeting the primary calibration goals for this release.
The DP2 photometric system is defined by the relative passbands derived from FGCM fits to DP2 observations.
These passbands ensure internal consistency but are not yet the final Rubin standard passbands that will be adopted for Data Release 1 (DR1).
As a result, synthetic photometry generated using spectrophotometry and the DP2 relative passbands will not perfectly match the observed DP2 magnitudes.

A small but measurable offset remains between the DP2 photometric system and the AB magnitude system.
These AB magnitude offsets are derived from DP2 observations of the HST CalSpec standard star C26202 (see the `CalSpec archive <https://www.stsci.edu/hst/instrumentation/reference-data-for-calibration-and-tools/astronomical-catalogs/calspec>`_), which lies in the ECDFS field and is unsaturated in LSST images.
The offsets are generally modest, ranging from 0.001 to 0.023 mag in grizy, but reach 0.071 mag in the u band (see table below).
These offsets are smaller than those discovered and corrected prior to DP1, but there was insufficient time to update the absolute calibration before DP2 was released.

Users who compute synthetic photometry using the DP2 relative passbands should therefore expect the resulting synthetic magnitudes to differ from observed DP2 photometry by the offsets listed below.



The offset is defined as

.. math::

   m_{offset} = m_{obs} - m_{AB}

Thus, to place observed DP2 photometry onto the AB system, subtract the offsets:

.. math::

   m_{AB} = m_{obs} - m_{offset}.



These offsets will be removed for Rubin DR1, when the final standard passbands, constrained by LSSTCam in-situ throughput measurements and an updated absolute calibration of The Monster reference catalog, are adopted.


AB Magnitude Offsets for the LSST DP2 Photometric System
--------------------------------------------------------


+------+--------+-----------------+----------------+-------------+
| band | n_band |m_obs\*          |m_AB\*\*        |m_offset     |
+======+========+=================+================+=============+
| u    | 9      | 17.649          | 17.578         | 0.071       |
+------+--------+-----------------+----------------+-------------+
| g    | 40     | 16.712          | 16.689         | 0.023       |
+------+--------+-----------------+----------------+-------------+
| r    | 43     | 16.363          | 16.362         | 0.001       |
+------+--------+-----------------+----------------+-------------+
| i    | 82     | 16.249          | 16.260         | -0.011      |
+------+--------+-----------------+----------------+-------------+
| z    | 53     | 16.242          | 16.244         | -0.001      |
+------+--------+-----------------+----------------+-------------+
| y    | 10     | 16.237          | 16.238         | -0.002      |
+------+--------+-----------------+----------------+-------------+


\* Observed LSST DP2 magnitude for the HST spectrophotometric standard
C26202, computed as the median of all source-table magnitude measurements
in the corresponding band.

\*\* Synthetic AB magnitude derived from the HST/STIS reference spectrum
``c26202_stiswfcnic_007`` of C26202 and integrated through the DP2 relative bandpass for the corresponding filter provided by FGCM.



Aperture flux uncertainties
---------------------------

The ``{band}_ap12FluxErr`` column in the Object table is sometimes NaN even when the uncertainty for larger apertures (e.g., ``{band}_ap17FluxErr``) is finite.
This is likely a problem with the sinc interpolation used for smaller apertures, and may also affect ``apFlux_12_0_instFluxErr`` in the Source table.

Sersic and Exponential model outputs
------------------------------------

Improvements to the Sersic and Exponential model outputs are planned for future data releases.
A full description of the ellipse parameterizations and units will be provided on the :doc:`/products/catalogs/object` page.

Photometric zeropoints in non-photometric conditions
----------------------------------------------------

FGCM derives per-visit photometric zeropoints by fitting a model of atmospheric and instrumental throughput to stellar observations distributed across the focal plane.
This approach works reliably when observing conditions are photometric — that is, when the atmosphere is stable and spatially uniform across the field of view.
However, some visits included in DP2 coadds were taken under non-photometric conditions, where clouds introduce spatially variable throughput losses across the field of view.

In such cases, the zeropoint derived from the cloud-free portion of the focal plane cannot be reliably extrapolated to the cloud-affected portion.
This is particularly relevant for LSSTCam data: cloud structure in LSSTCam images tends to be spatially complex and variable, making interpolation or extrapolation of zeropoints across the focal plane less reliable than in previous surveys.
As a result, objects located in cloud-affected regions of an exposure may carry an inaccurate photometric zeropoint.
Since these visits are combined into coadds, the impact is most visible as spatially correlated photometric offsets, which can vary from coadd cell to coadd cell.

Users performing precision photometry — especially on faint or extended sources — should be aware that the photometric uniformity of DP2 may be locally degraded in regions observed predominantly under non-photometric conditions.


.. _issues_backgrounds:

Background subtraction
=======================

The ``SkyCorrectionTask`` that performs full-focal-plane background correction is not designed to accommodate detector-to-detector offsets in the input data.
These offsets arise from several sources, including E2V/ITL sensor differences, offset detectors, and offset REBs, and are more pronounced in the u and y bands.
A new full-focal-plane background fitter is under development but was not ready for DP2.


.. _issues_coadds:

Coadds
======

Empty cells
-----------

Coadd cells will have no data if none of the input warps is below the masked-pixel threshold, even though a fallback (such as including the warp with the largest mask fraction) could have been implemented.
This is not so much a bug as a case where the best solution has not yet been chosen.

Bad input visits
----------------

There are 15 visits included in coadd assembly that ran through the Science Pipelines despite being visually obviously bad.
These include images that are very out of focus or had the shutter open while the telescope was in motion.
The affected visits are listed below.
These are a subset of the visits listed in ``bad.ecsv`` for LSSTCam in `excluded_visits <https://github.com/lsst-dm/excluded_visits>`__ , and were identified after DP2 processing had already begun.
They are also a subset of the visits listed in :ref:`Bad visits <issues_badvisits>`.

.. code-block::

   Visits from DP2 deep_coadd_input_summary in excluded list: 15
   [2025060400117 2025062000652 2025062900348 2025070300278 2025070300493
    2025070400108 2025070400290 2025070400291 2025071100194 2025071700678
    2025071800299 2025071800360 2025071800368 2025071800445 2025071800518]


.. _issues_object_catalog:

Object catalog
==============

Objects spanning multiple cell footprints
-----------------------------------------

There is no column to identify objects whose cells span multiple footprints, and which therefore effectively have ``INEXACT_PSF`` for their measurements.
What to do about this has not yet been decided.

Duplicate or missing objects near patch boundaries
--------------------------------------------------

A tiny fraction of objects, of the order of 10 per tract, with centroids near patch boundaries may appear in the Object table zero times (if their reference-band centroids happen to shift outside the inner area of every patch) or multiple times (up to four).

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

The machine-learned reliability ("real/bogus") model for DIA sources shows improved performance relative to DP1, but does not perform well on bad stellar subtractions: detections matched to Gaia stars, both real variables and subtraction artifacts, receive characteristically low reliability scores.

Other known weaknesses of the model are:

- A spurious peak in the reliability distribution at approximately 0.3, produced when the science or difference image cutouts contain rows or columns of NaN values.
- A bias against low signal-to-noise sources: detections with ``|psfFlux/psfFluxErr|`` below approximately 5 are unlikely to receive a reliability above 0.6.
- An excess of reliability scores above 0.5 for negative-flux detections.

On test data, a purity and completeness of 93.1% are both achieved at a reliability threshold of 0.596 (97.5% at a threshold of 0.301 for injected point sources); users can adjust the threshold to trade purity against completeness.
See `DMTN-337 <https://dmtn-337.lsst.io/>`__ for a full description of the model, its training data, and its performance.

**DP2 reliability scores were accidentally calculated with the v0.1 model.** This is the same model as used for DP1.

Diffraction spike masks and bright-star halos
---------------------------------------------

The ``SPIKE`` masks are excessively wide at the bases.
In practice, where the bases intersect they mask out a polygon that is typically (possibly always) larger than the saturated (``SAT``) mask.

Despite the large spike masks, there is usually some residual flux from the halos of bright stars, beyond where the ``SPIKE`` masks end, that is not subtracted off.
Detections in these regions are more likely to be bogus, and even if real, the measurements are likely unreliable.
In the simplest case, where the diffraction spikes all line up, these detections are clustered around the four points where the spike-mask triangles intersect, making a square/diamond shape around the center of the star.


.. _issues_solarsystem:

Solar system
============

Astrometry
----------

Astrometric comparisons show different behavior for different object samples in DP2.
For long-observed objects discovered before 2000, median measured-minus-predicted coordinate residuals are below 1 mas, with approximately 13--14 mas scatter.
For objects discovered during 2000--2020, offsets relative to JPL Horizons are spatially coherent across the DP2 footprint: in 5 by 5 degree sky cells, the median offset-vector length is approximately 19 mas, the 95th percentile is approximately 30 mas, and the largest values approach 42 mas.
The current hypothesis is that orbit catalogs can retain 20--40 mas systematic errors in predicted positions, especially for objects constrained primarily by northern, pre-Gaia astrometry.
This interpretation remains under investigation and will be discussed in detail in a subsequent paper.

Rubin First Look Solar System objects
-------------------------------------

The DP2 release includes the M49 and Trifid-Lagoon fields (see :doc:`small fields <overview/observations>`), which produced the Rubin First Look (RFL) images in June 2025. However, not all of the >2,000 Solar System objects detected/discovered and released via the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC) as part of the RFL media event are included in the DP2 release due to differences in quality cuts between the RFL and DP2 datasets. For more information on accessing the RFL Solar System objects through the MPC, see the tutorial on `Rubin First Look Solar System object discoveries <https://prompt-products.lsst.io/tutorials/notebook/notebook-mpc.html>`_.

MPC_orbits table
----------------

Several fields in the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC)-derived data contained in the DP2 ``mpc_orbits`` table (2026 March 13 snapshot obtained from the MPC) contain inconsistent information or issues related to upstream failures, including:
- Missing semimajor axes
- Incorrect number of oppositions
- Incorrect number of observations
- Incorrect arc lengths
- Missing orbit type integers

Overall, the orbital elements in the DP2 ``mpc_orbits`` table are generally reliable, though occasional upstream failures may have occurred and be included in the table.

Current_identifications table
-----------------------------

The MPC-derived data contained in the DP2 ``current_identifications`` table (2026 March 13 snapshot obtained from the MPC) include many missing object type integers due to upstream failures.


.. _issues_badvisits:

Bad visits
==========

There are 39 visits in DP2 that processed through at least some stages of the Science Pipelines despite being visually obviously bad.
As described in :ref:`Coadds <issues_coadds>`, only 15 of these were used as inputs to coadds.
They include images that are very out of focus or had the shutter open while the telescope was in motion.
The affected visits are listed below.
These are a subset of the visits listed in ``bad.ecsv`` for LSSTCam in `excluded_visits <https://github.com/lsst-dm/excluded_visits>`__ , and were identified after DP2 processing had already begun.

.. code-block::

   Visits from DP2 visit_summary in excluded list: 39
   [2025050600762 2025052500596 2025052900166 2025060100605 2025060400085
    2025060400117 2025061900271 2025062000652 2025062900348 2025063000597
    2025070300278 2025070300493 2025070400108 2025070400290 2025070400291
    2025070400293 2025070800409 2025071100194 2025071100273 2025071100319
    2025071100365 2025071100501 2025071100819 2025071300612 2025071600479
    2025071700678 2025071800104 2025071800110 2025071800129 2025071800151
    2025071800299 2025071800360 2025071800368 2025071800382 2025071800445
    2025071800518 2025072000352 2025072200096 2025072200207]
