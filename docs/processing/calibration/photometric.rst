#######################
Photometric calibration
#######################

Photometric calibration derives the conversion of raw "counts" recorded by the camera into fluxes calibrated onto a standard system.
This includes correcting for effects in the detectors themselves (discussed in :doc:`/processing/isr/index`), filter throughputs, and atmospheric transparency which varies with airmass and time.
These components all contribute to the photometric uncertainties reported in the catalog flux measurements.

**System characterization:** Initial steps *not requiring on-sky data* to characterize the instrumental and overall system properties included:

* Measuring the instrumental response as a function of wavelength and position on the focal plane (mostly measured in the lab prior to going on sky).
* Measuring the throughput of the filters (also as a function of position; this came from measurements performed by the vendors who manufactured the filters).
* Measuring the optical system throughput as a function of wavelength using a collimated beam projector (CBP) and a tunable laser.
* Measuring the pixel-to-pixel response (flat-fielding) using multi-band images of an illuminated calibration screen in the dome.

**Calibration using on-sky data:** On-sky images are used to characterize the system, and of course must be calibrated themselves to measure atmospheric and other time-varying effects.
After :doc:`/processing/isr/index` has been performed on the images, additional photometric calibration steps include:

* Initial calibration is performed by detecting sources, estimating the point-spread function (PSF), then fitting an initial solution to :doc:`/processing/calibration/monster` reference catalog. The main purpose of this step is to provide a good enough photometric and astrometric calibration to enable associating multiple observations of stars.
* The final, global photometric calibration uses the Forward Global Calibration Method (FGCM; `Burke et al. 2018 <https://ui.adsabs.harvard.edu/abs/2018AJ....155...41B/abstract>`_). FGCM takes stars with S/N > 10 plus their matched counterparts from The Monster, and forward models repeated measurements of stars to determine parameters describing the atmospheric transmittance and instrumental response that minimize the scatter in the stars' magnitudes. The fits result in a measure of the wavelength-dependent observational passband at the time of each measurement.
* Observations of overlapping spectrophotometric flux standard standard stars are used to evaluate the accuracy of the absolute system throughputs.

Additional resources
====================

For a description of the photometric calibration steps and hardware to enable them, refer to the "Rubin Baseline Calibration Plan" (`sitcomtn-086.lsst.io <https://sitcomtn-086.lsst.io/>`_).

