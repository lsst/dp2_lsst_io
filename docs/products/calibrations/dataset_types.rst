.. _calibrations-dataset-types:

#######################################
Calibration Butler dataset types
#######################################

.. important::

   Calibration images are not part of Early Data Preview 2 (EDP2); they will be delivered with the full DP2 release.
   For more information, see "Rubin Plans for an Early Science Program" (`RTN-011 <https://ls.st/rtn-011>`__).

The calibration products are accessed via the Butler as a number of dataset types.
The combined bias, dark, and flat frames described on the
:doc:`main calibrations page <index>` are the most commonly used, but the full set of
calibration dataset types and their descriptions is given below.

These calibrations are produced and consumed by the
:doc:`Instrument Signature Removal (ISR) </processing/isr/index>` pipeline.
For a detailed description of the calibration products and the ISR process, see
`Plazas Malagón et al. 2025 <https://ui.adsabs.harvard.edu/abs/2024arXiv240414516P/abstract>`_
and `RTN-117 <https://rtn-117.lsst.io>`_.

For each dataset type below, the required dimensions of the Butler ``dataId`` are shown in **boldface**.

Raw images
==========

``raw``
    ('raw', {band, **instrument**, day_obs, **detector**, group, physical_filter, **exposure**}, Exposure)

    The unprocessed images from the camera, which are the input to the ISR pipeline;
    see :doc:`/products/images/raw_exposure` for a full description.

Detector characterization
=========================

``defects``
    ('defects', {**instrument**, **detector**}, Defects, isCalibration=True)

    A mask identifying bad pixels, columns, and other detector defects (hot,
    dead, or otherwise unreliable pixels) that should be flagged or
    interpolated over; includes defects identified in bias, dark, and flat
    images, in addition to a set of manually marked pixels (e.g. vampires).

``crosstalk``
    ('crosstalk', {**instrument**, **detector**}, CrosstalkCalib, isCalibration=True)

    The crosstalk matrix containing coefficients of the coupling between all
    combination pairs of amplifiers in a detector, used to correct copycat
    images induced by bright sources on neighboring channels.

``linearizer``
    ('linearizer', {**instrument**, **detector**}, Linearizer, isCalibration=True)

    A correction that maps measured signal to a linear response, removing the
    detector's nonlinear behavior as a function of flux level.

``cti``
    ('cti', {**instrument**, **detector**}, IsrCalib, isCalibration=True)

    The deferred charge calibration. Though it is called CTI, it contains a set
    of model parameters for classical global charge transfer inefficiency (CTI)
    as well as serial traps and local electron offset drifts, characterizing
    and removing trailing/smearing of charge that occurs during detector
    readout.

``ptc``
    ('ptc', {**instrument**, **detector**}, PhotonTransferCurveDataset, isCalibration=True)

    The photon transfer curve, relating signal variance to mean signal to
    derive per-amplifier gain and read noise for variance plane estimation, and
    also contains a set of model fit parameters used to characterize detector
    response (incl. brighter-fatter).

Combined calibration frames
===========================

``bias``
    ('bias', {**instrument**, **detector**}, ExposureF, isCalibration=True)

    The final combined bias frame removing static electronic structure present
    in zero-exposure reads; built by combining many bias exposures with defect
    masking and preliminary corrections applied.

``dark``
    ('dark', {**instrument**, **detector**}, ExposureF, isCalibration=True)

    The final combined dark frame characterizing and removing dark current
    accumulated as a function of exposure time (measured from 30 s dark
    exposures).

``flat``
    ('flat', {band, **instrument**, **detector**, **physical_filter**}, ExposureF, isCalibration=True)

    The final normalized, combined flat fields per filter correcting QE
    variations in the detector's response to uniform illumination, and taking
    into account the (approximately flat) in-band SED of the illumination
    source from the in-dome calibration system.

Brighter-fatter correction
===========================

``electroBfDistortionMatrix``
    ('electroBfDistortionMatrix', {**instrument**, **detector**}, IsrCalib, isCalibration=True)

    The brighter-fatter distortion matrix (from
    `Astier & Regnault 2023 <https://ui.adsabs.harvard.edu/abs/2023A%26A...670A.118A/abstract>`_)
    describing the pixel area changes in response to a single charge. It
    contains a set of model parameters to solve for the electric field in a
    pixel due to a single charge, as well as a set of pixel distortion matrices
    that describe how pixel area changes in response to a charge in a pixel due
    to shifts in the north, south, east, and west boundaries.

    We are no longer using the Coulton correction
    (`Coulton et al. 2018 <https://ui.adsabs.harvard.edu/abs/2018AJ....155..258C/abstract>`_),
    so there are no brighter-fatter kernels; instead there is a single
    correction that combines both
    `Astier & Regnault 2023 <https://ui.adsabs.harvard.edu/abs/2023A%26A...670A.118A/abstract>`_
    and
    `Broughton et al. 2024 <https://ui.adsabs.harvard.edu/abs/2024PASP..136d5003B/abstract>`_.
    This brighter-fatter correction has been meeting the Y10 science
    requirements.
