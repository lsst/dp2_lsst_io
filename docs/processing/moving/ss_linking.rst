.. _moving-linking:

#############################################
Small body tracklet linking and orbit fitting
#############################################

.. important::

   The tracklet linking and orbit fitting discovery campaign described on this page runs as part of `Prompt Processing <https://prompt-products.lsst.io/processing/prompt-processing/index.html>`_, but was not used as a pipeline algorithm for DP2 data processing.
The process is presented here to give a general idea of how tracklet linking was used for DP2 and an overview of how Rubin discovers asteroids on a continuous basis.

.. image:: images/LSST-HelioLinC3D-Infographic.png

Small body tracklet linking and orbit fitting is done with the `HelioLinC3D <https://github.com/lsst-dm/heliolinc2>`_ package.

The goal of the HelioLinC3D software package is to discover previously unknown asteroids amid the millions of new difference-image detections every night.
The algorithm identifies and links together little sequences of typically 6-20 sources that could comprise repeated detections of a new asteroid moving in its orbit around the Sun.

These sets of detections (called 'linkages') are formed in two stages.
First, ‘tracklets’ of observations are identified, where a tracklet comprises at least two images within a single night.
Next, tracklets from multiple nights are linked together.
LSST specifications state that a valid linkage must include at least three tracklets, each from a different night, and all within a 14-day period.
Each linkage meeting these criteria constitutes a candidate asteroid discovery.

After the full set of candidate linkages has been produced, they are culled and refined through orbit fitting and other analyses.
The final product is a purified set of thousands of non-overlapping linkages, each of which has an orbit-fit with sub-arcsecond astrometric residuals.
These linkages -- each comprising a probable new asteroid discovery -- are submitted to the `Minor Planet Center <https://minorplanetcenter.net>`_ (MPC) for confirmation and publication.
The tracklet linking and orbit fitting procedure is illustrated in the infographic provided above.
