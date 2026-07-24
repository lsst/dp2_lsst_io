######################
Background subtraction
######################

The background subtraction algorithms in the LSST Science Pipelines estimate and remove large-scale background signals from science imaging.
Such signals include sky brightness from airglow, moonlight, scattered light, zodiacal light, and diffuse astrophysical emission.
In so doing, true astrophysical sources are isolated to allow for accurate detection and measurement.
For a detailed description of the DP2 background subtraction implementation, see `RTN-115 <https://rtn-115.lsst.io/>`_.


Overview
========

Background subtraction in `CalibrateImageTask <https://pipelines.lsst.io/modules/lsst.pipe.tasks/tasks/lsst.pipe.tasks.calibrateImage.CalibrateImageTask.html>`_ estimates and removes large-scale background signals from science imaging prior to source detection and measurement.
The background is estimated twice: an initial model (``psf_subtract_background``) is subtracted before PSF characterization, and a second model (``star_background``) is fit on the updated source-masked image before the final detection pass.

Each post-:doc:`ISR </processing/isr/index>` image is divided into 128×128 pixel superpixels.
The iterative 3σ-clipped mean of unmasked pixels in each bin is fit with a sixth-order two-dimensional Chebyshev polynomial, evaluated at native pixel resolution via Akima spline interpolation.
Before fitting ``star_background``, the ``DETECTED`` mask plane is dilated by up to 10 pixels to suppress source-wing flux from leaking into background bins.

Additionally, an extra iterative detection round is run to converge on an optimal detection fraction of pixels in the image (a "Goldilocks Zone" where the background is neither over- nor under-masked).
The detection mask plane used for this step is discarded before the final source catalog detection pass.

After ``star_background`` subtraction, a zeroth-order pedestal correction is estimated iteratively over bin sizes starting at 32×32 pixels, doubling each step until the cumulative pedestal level changes by less than 5% or 0.5 counts between steps.
The ``star_background`` model and pedestal corrections together constitute the output background model (``preliminary_visit_image_background``).

This procedure is designed to mitigate the over-subtraction tendency to which background estimates are typically prone, especially when trying to accommodate a wide range of scenes from sparse to crowded fields.

Following background subtraction, the median and standard deviation of all unmasked pixels and of *sky source* fluxes are recorded in the task metadata as diagnostics of background subtraction quality.


Changes relative to DP1
========================

Compared to DP1, several changes were made to background subtraction for DP2 (`RTN-115 <https://rtn-115.lsst.io/>`_):

- The inline re-estimation performed after each detection pass in DP1 (``reEstimateBackground = True``) has been replaced by the dedicated ``star_background`` subtask.
- Source mask dilation and a pedestal correction have been added.
- Adaptive threshold determination and diffraction spike masking are new features for DP2.
- The per-step illumination correction used in DP1 has been disabled.


Known limitations
=================

**Crowded fields.**
PSF residuals in DP2 show structure that correlates with stellar density, with the largest biases appearing in the Milky Way, the LMC, and the SMC.
The stellar-density dependence also explains observed differences in PSF residuals between bands.
The underlying cause is believed to be related to background subtraction in dense fields (`RTN-115 <https://rtn-115.lsst.io/>`_).

**Fringing in z and y bands.**
Fringing is an interference pattern produced when narrow-band emission reflects within the thin, back-illuminated silicon of LSSTCam CCDs.
It is driven primarily by night-sky emission lines and becomes appreciable in the z and y bands, where the silicon is increasingly transmissive and OH and other atmospheric lines contribute strongly to the background.
Residual fringe structure is visible at low surface brightness in z- and y-band visit images and can bias wide-aperture photometry.
The DP2 ISR pipeline does not include a dedicated fringe-subtraction step (`RTN-117 <https://rtn-117.lsst.io/>`_).
