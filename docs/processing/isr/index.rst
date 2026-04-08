##################################
Instrument signature removal (ISR)
##################################

ISR is the first step of image processing, which removes instrumental effects introduced in the raw images by the telescope and detectors and produces an accurate representation of the incoming light.

Known issues
============

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

These changes result in three calibration epochs (2025-04-24 to 2025-07-02, 2025-07-03 to 2025-09-21, and 2025-10-24 to 2026-01-06) with different noise characteristics. Flat fields improved with each successive epoch, and the y-band flats are notably better for the third epoch due to the installation of a new 1050 nm LED.

Flat fielding
-------------

The flat-field screen is not uniformly illuminated, nor does it uniformly fill the focal plane.
For each filter except u, two "anaglyph" flats are taken (one with a bluer LED and one with a redder LED), with special gradient-removal code to correct for gradients and centering.
This correction does not work well in the outer partly vignetted region of the focal plane.

Camera temperature variations (particularly during the second calibration epoch) and focal plane restarts also changed gains slightly, resulting in additional amplifier offsets.

