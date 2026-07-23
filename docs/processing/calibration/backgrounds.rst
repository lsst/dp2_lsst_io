######################
Background subtraction
######################

The background subtraction algorithms in the LSST Science Pipelines estimate and remove large-scale background signals from science imaging.
Such signals may include sky brightness from airglow, moonlight, scattered light, zodiacal light, instrumental effects, and diffuse astrophysical emission.
In so doing, true astrophysical sources are isolated to allow for accurate detection and measurement.
For a detailed description of the DP2 background subtraction implementation, see `RTN-115 <https://rtn-115.lsst.io/>`_.


Overview
========

Each post-:doc:`ISR </processing/isr/index>` image is processed by the `CalibrateImageTask <https://pipelines.lsst.io/modules/lsst.pipe.tasks/tasks/lsst.pipe.tasks.calibrateImage.CalibrateImageTask.html>`_, which performs image characterization including background subtraction and produces the preliminary science image (``preliminary_visit_image``) and the associated preliminary background model (``preliminary_visit_image_background``).

Background subtraction is performed in two passes.
An initial background model (``psf_subtract_background``) is subtracted before PSF characterization.
A second model (``star_background``) is then fit on the updated source-masked image before the final detection pass.

To generate a background model, each post-ISR image is divided into superpixels of 128×128 pixels.
Pixels with a mask flag indicating no useful science data or flux from a preliminary source detection are masked.
The iterative 3σ-clipped mean of the remaining unmasked pixels is calculated for each superpixel, constructing a background statistics image.
A sixth-order two-dimensional Chebyshev polynomial is fit to these values and evaluated at native pixel resolution via Akima spline interpolation.
Before fitting ``star_background``, the ``DETECTED`` mask plane is dilated by up to 10 pixels to suppress source-wing flux from leaking into background bins.

After ``star_background`` subtraction, a zeroth-order pedestal correction is estimated iteratively over bin sizes starting at 32×32 pixels, doubling each step until the cumulative pedestal level changes by less than 5% or 0.5 counts between steps.
The ``star_background`` model and pedestal corrections together constitute the output background model (``preliminary_visit_image_background``).

Following background subtraction, the median and standard deviation of all unmasked pixels and of *sky source* fluxes are recorded in the task metadata as diagnostics of background subtraction quality.

Using ``preliminary_visit_image_background`` as input, `ReprocessVisitImageTask <https://pipelines.lsst.io/v/d_2024_11_01/modules/lsst.drp.tasks/tasks/lsst.drp.tasks.reprocess_visit_image.ReprocessVisitImageTask.html>`_ applies calibration models produced by upstream tasks to the single-visit image.
It then performs the final round of source detection, deblending, and measurement that ultimately populate the ``Source`` table.
One of the outputs of ``ReprocessVisitImageTask`` is the refined, final visit-level background model, ``visit_image_background``, along with the final background-subtracted visit image, ``visit_image``.


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
