##################################
Instrument signature removal (ISR)
##################################

ISR is the first step of image processing, which removes instrumental effects introduced in the raw images by the telescope and detectors and produces an accurate representation of the incoming light.

**Users should not attempt to recreate or rerun ISR**, and a full understanding of the components and subtleties of ISR is not necessary in order to use the data products.

.. figure:: images/isr_model.png
    :width: 600
    :name: isr_model
    :alt: A schematic diagram of a model with various detector signatures relevant for LSSTCam images.

    Figure 1: The model of the detector and readout electronics board (REB) components, labeled with the effects that they impart on signal (from |dp1_paper|).


.. figure:: images/isr_steps.png
    :width: 600
    :name: isr_steps
    :alt: A schematic diagram with Instrument Signature Removal steps relevant for LSSTCam images.

    Figure 2: Instrument Signature Removal steps derived from the detector model in Figure 1 (from `Plazas Malagón et al. 2025 <https://ui.adsabs.harvard.edu/abs/2025JATIS..11a1209P/abstract>`_).



Components
==========

The steps of ISR include (from `Plazas Malagón et al. 2025 <https://ui.adsabs.harvard.edu/abs/2025JATIS..11a1209P/abstract>`_):

**Dithering of digitized counts**: applies a small random offset in the range [−0.5, 0.5) analog-to-digital units (ADU) to mitigate quantization bias introduced by analog-to-digital conversion.

**Serial overscan subtraction**: removes row-wise electronic bias using the serial overscan region, typically via a per-row median excluding the initial columns affected by deferred charge.

**Masking of saturated and suspect pixels**: flags pixels that exceed defined saturation levels or exhibit anomalies, ensuring they are excluded from subsequent calibration steps.

**Gain scaling**: converts pixel values from ADU to electrons using temperature-corrected gain factors derived from photon transfer curve (PTC) measurements.

**Correction of crosstalk in parallel overscan**: removes signal leakage from high-charge pixels into the parallel overscan regions of other amplifiers before performing the overscan subtraction.

**Parallel overscan subtraction**: subtracts column-wise bias using the parallel overscan region, after crosstalk and saturation artifacts have been corrected.

**Correction of crosstalk between amplifiers**: removes crosstalk between amplifier channels using a pre-measured crosstalk matrix, correcting both intra- and inter-CCD effects.

**Linearity correction**: corrects for non-linear detector response at medium and high signal levels.

**Serial charge transfer inefficiency (CTI) correction**: removes trailing charge artifacts from incomplete charge transfer in the serial register using a flux- and position-dependent correction model.

**Image assembly and trimming**: combines the 16 amplifier segments into a single CCD image and trims the overscan regions.

**Bias subtraction**: subtracts a combined bias frame created from multiple zero-time exposure images, correcting for static readout structure and electronic offsets.

**Dark subtraction**: removes the thermal dark current and any residual bias structure using a combined dark frame measured with closed-shutter exposures.

**Brighter-fatter correction**: corrects for the "brighter-fatter" effect (where brighter sources appear larger due to electrostatic interactions in the detector) using a convolution kernel calibrated from flat-field pixel correlations.

**Defect masking and interpolation**: flags and interpolates over known bad pixels or columns identified from flat and dark exposures as statistical outliers.

**Variance plane construction**: computes the variance per pixel from the Poisson noise and read noise, creating a map for uncertainty propagation in later processing.

**Flat fielding**: applies a background and reference flat to convert images to fluence units (e−/pixel), correcting for illumination non-uniformities.


Overview
========

Each sensor and its readout amplifiers can vary slightly in performance, causing images of even a uniformly illuminated focal plane to exhibit discontinuities and shifts due to detector effects.
Figure 1 illustrates the model of detector components and their impact on the signal, tracing the process from photons incident on the detector surface to the final quantized values recorded in the image files.
Based on this model, a series of Instrument Signature Removal steps are implemented to eliminate camera-induced effects (Figure 2).

The ISR pipeline essentially “works backward” through the signal chain, correcting the integer analog-to-digital units (ADU) raw camera output back to a floating-point number of photoelectrons created in the silicon.
The physical detector, shown on the left in Figure 1, is the source of effects that arise from the silicon itself, such as the dark current and the brighter-fatter effect (`Broughton et al. 2024 <https://ui.adsabs.harvard.edu/abs/2024PASP..136d5003B/abstract>`_, `Gruen et al. 2015 <https://ui.adsabs.harvard.edu/abs/2015JInst..10C5032G/abstract>`_).

After the image has integrated, the charge is shifted to the serial register and read out, which can introduce charge transfer inefficiencies and a clock-injected offset level.
The signals for all amplifiers are transferred via cables to the Readout Electronics Board (REB), during which crosstalk between the amplifiers may occur.
The Analog Signal Processing Integrated Circuit (ASPIC) on the REB converts the analog signal from the detector into a digital signal, adding both quantization and a bias level to the image.
Although the signal chain is designed to be stable and linear, the presence of numerous sources of non-linearity reveals its complexity.

Following this model, the sequence of ISR corrections is structured to reverse the detector and electronics effects in the order opposite to their introduction.
For example, quantization artifacts are addressed first through dithering and differential non-linearity correction, followed by serial overscan subtraction, saturation masking, and gain normalization.
Crosstalk is then corrected to prevent its contamination of later steps like parallel overscan subtraction and linearity correction.
CTI is corrected next, just before assembling the amplifier segments into full CCD images.
The final steps include bias and dark subtraction, brighter-fatter effect correction, defect masking, variance plane construction, and flat-fielding.
Each of these steps is tied to specific elements in the detector readout chain, and their ordering ensures that each correction builds upon a cleaner, more physically meaningful image (`Plazas Malagón et al., 2025 <https://ui.adsabs.harvard.edu/abs/2025JATIS..11a1209P/abstract>`_).


Subtleties
==========

Amplifier offsets
-----------------

Through much of DP2 processing, there were amplifier offsets that propagated through to photometry and PSF residuals.
The "amp offset correction" code was enabled to mitigate this issue.
While this correction is not fully principled, it appears to work on average with no evidence of pathological behavior so far.

Camera sequencer changes
------------------------

Three different camera sequencer configurations were used during the DP2 observing period:

- **"2s_v30" sequencer** (April 2025 through July 2, 2025): 2+ second readout, with extra noise in some E2V sensors and most ITL amplifiers, and random gain jumps (~0.02% level) for some ITL amplifiers. Basic calibrations were derived from clean-room data.

- **"3s_v1" sequencer** (July 2, 2025 through January 23, 2026): 3 second readout, with significantly reduced noise in both E2V and ITL amplifiers, though some unexpected even/odd pixel correlations in E2V noise remained. Random gain jumps (~0.02% level) for some ITL amplifiers persisted.

- **"3s_v2" sequencer** (from January 23, 2026): not included in DP2.

These changes result in three calibration epochs (2025-04-24 to 2025-07-02, 2025-07-03 to 2025-09-21, and 2025-10-24 to 2026-01-06) with different noise characteristics.
Flat fields improved with each successive epoch, and the y-band flats are notably better for the third epoch due to the installation of a new 1050 nm LED.

Flat fielding
-------------

The flat-field screen is not uniformly illuminated, nor does it uniformly fill the focal plane.
For each filter except u, two "anaglyph" flats are taken (one with a bluer LED and one with a redder LED), with special gradient-removal code to correct for gradients and centering.
This correction does not work well in the outer partly vignetted region of the focal plane.

Camera temperature variations (particularly during the second calibration epoch) and focal plane restarts also changed gains slightly, resulting in additional amplifier offsets.


Additional resources
====================

For descriptions of the ISR steps and the generation, verification, certification, approval, and distribution of the calibration products necessary for ISR, refer to the following:

* `Instrument Signature Removal and Calibration Products for the Rubin Legacy Survey of Space and Time <https://ui.adsabs.harvard.edu/abs/2025JATIS..11a1209P/abstract>`_
* "Rubin Baseline Calibration Plan" (`SITCOMTN-086 <https://sitcomtn-086.lsst.io/>`_)
* "Verifying LSST Calibration Data Products" (`DMTN-101 <https://dmtn-101.lsst.io/>`_)
* "Calibration Generation, Verification, Acceptance, and Certification" (`DMTN-222 <https://dmtn-222.lsst.io/>`_)