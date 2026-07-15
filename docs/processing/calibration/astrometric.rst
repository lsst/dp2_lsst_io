#######################
Astrometric calibration
#######################

The astrometric calibrations determine the World Coordinate System (WCS) of an image, the relation between pixel coordinates and sky coordinates (Right Ascension and Declination).
Astrometric calibration uses the ``gbdes`` package (`Bernstein et al. 2017 <https://ui.adsabs.harvard.edu/abs/2017PASP..129g4503B/abstract>`_).
For a description of the implementation of ``gbdes``, see "Astrometric Calibration in the LSST Pipeline" (`dmtn-266.lsst.io <https://dmtn-266.lsst.io/>`_).

Overview
========

Bright, isolated stars detected in the post-ISR images are used to obtain an initial astrometric solution.

The final astrometric calibration of DP2 follows the method used in DP1, with some key additions described below (see also the DP2 paper, `rtn-115.lsst.io <https://rtn-115.lsst.io/>`_).
A joint calibration is performed on all visits in a given band that overlap a given region of the sky.
By associating the isolated point sources shared among the overlapping visits, and matching them to the Gaia DR3 reference catalog, the astrometric solution is refined beyond the level achieved in single-frame processing using only a reference catalog.
Unlike in DP1, where the solution was fit per tract, in DP2 the region used for the individual fits is an order-3 HEALPix pixel.
This reduces repetitive calculations: tracts are small compared to the LSSTCam field of view, so neighboring tracts often share identical overlapping visits, which would lead to many identical fits.

The astrometric model consists of a static map from pixel-space to an intermediate frame (the per-detector model), followed by a per-visit map from the intermediate frame to the plane tangent to the telescope boresight (the per-visit model), then finally a deterministic mapping from the tangent plane to the sky.
The per-detector model is intended to capture quasi-static characteristics of the telescope and camera, and account for changes in the camera due to warming and cooling and other discrete events.
The per-visit model attempts to account for time-varying effects on the path of a photon from both atmospheric sources and those dependent on the telescope position.
The proper motion and parallax of the objects used in the calibration are fit as part of the solution; the reference epoch for the fit positions is 2024.9.
Differential chromatic refraction and lateral color are not yet included in the model, and will be added in future data releases.

Gaussian Processes fit of atmospheric turbulence
================================================

The main difference from the DP1 astrometric calibration is an additional step that models the effect of atmospheric turbulence on the astrometry.
The per-visit model captures large-scale variation between visits, including some atmospheric effects, but higher-order oscillations remain visible in the astrometric residuals and can be attributed to atmospheric turbulence (see "Early astrometric residuals characterization of LSSTCam", `dmtn-324.lsst.io <https://dmtn-324.lsst.io/>`_).
These wave-like astrometric effects can be modeled by a Gaussian Process (`Fortino et al. 2021 <https://ui.adsabs.harvard.edu/abs/2021AJ....162..106F/abstract>`_, `Léget et al. 2021 <https://ui.adsabs.harvard.edu/abs/2021A%26A...650A..81L/abstract>`_).
In DP2, the astrometric residuals remaining after the ``gbdes`` fit are grouped by visit and modeled with the Gaussian Process package ``treegp``.
The Gaussian Process prediction of the turbulence is evaluated on a grid, which is then turned into a two-dimensional spline that is concatenated with the ``gbdes`` model.
Including this model of the higher-order atmospheric turbulence significantly improves the astrometric solution.

Isolated star stellar motions
=============================

A side product of the astrometric calibration is a catalog of the proper motions and parallaxes of the sources used in the calibration.
Since the ``gbdes`` fit is done per band, and then improved by the turbulence fit, an additional final fit of the proper motion and parallax is performed, using the final astrometric model and all available bands.
The resulting catalog, :doc:`/products/catalogs/isolated_star_stellar_motions`, is included in the DP2 data release.
It only includes isolated point sources; future data releases will be extended to include proper motion and parallax for all objects.
