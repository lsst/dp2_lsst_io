.. _moving-association:

################################
Source detection and association
################################

Moving objects are detected as part of :doc:`/processing/dia/index`.
For the DP2 Solar System products, ``DiaSource`` detections were associated with the predicted positions of previously known small bodies.

Association input
=================

The association input was a 2026 March 13 snapshot of the MPC orbit database, limited to objects whose observational arcs exceeded two days.
Small bodies discovered after that date, including those found in Rubin data, are therefore absent from the delivered ``SSSource`` table.
The input orbit catalog did not contain comets, so no comets were available for association in DP2.

Ephemerides at the visit epochs were generated with `Sorcha <https://sorcha.space/>`_ (see `Merritt et al. 2025 <https://ui.adsabs.harvard.edu/abs/2025AJ....170..100M/abstract>`_ and `Holman et al. 2025 <https://ui.adsabs.harvard.edu/abs/2025AJ....170...97H/abstract>`_).
The same MPC snapshot is delivered as the auxiliary DP2 ``mpc_orbits`` table.

Matching algorithm
==================

Associations were made independently for each detector and visit.
Predicted objects were first restricted to the detector footprint.
For each predicted position, the algorithm found all ``DiaSources`` within 1 arcsecond.
For each visit-detector, all candidate pairs were considered in order of increasing separation and accepted only if neither the predicted object nor the detection had already been assigned.
The result is therefore a one-to-one positional match.

The matching did not use positional uncertainties, brightness, color, morphology, motion rate, or a probabilistic association score.
The delivered ``diaDistanceRank`` is 1 for every ``SSSource`` row and must not be interpreted as a ranking of alternative matches.
Each ``diaSourceId`` occurs at most once in ``SSSource``; unassociated ``DiaSource`` detections have no corresponding row.

Contents and interpretation
===========================

Each accepted ``SSSource`` row contains selected measured quantities from the ``DiaSource`` and the predicted ephemeris at the observation epoch.
These include measured astrometry and PSF photometry, predicted position and apparent Johnson-V magnitude, observing geometry, rates, and Cartesian position and velocity components.
The ephemeris coordinates and state vectors are predictions from the input MPC orbit, not a Rubin-derived orbital solution.
Users needing the complete detection record or detection-quality fields should join ``SSSource`` to ``DiaSource`` on ``diaSourceId``.

The ``ephOffset`` column gives the total measured-minus-predicted angular separation.
The coordinate, along-track, and cross-track components are available in ``ephOffsetRa``, ``ephOffsetDec``, ``ephOffsetAlongTrack``, and ``ephOffsetCrossTrack``.
These residuals are useful diagnostics, but the hard 1-arcsecond selection radius truncates their distribution.

Chance associations remain possible, especially in regions with high ``DiaSource`` density, because the match does not use brightness or detection reliability.
Users constructing a high-purity sample should consider detection reliability, angular and cross-track residuals, consistency with the predicted magnitude, and the uncertainty of the input orbit.

At this time, most of the associations delivered here have not yet been reported to the Minor Planet Center (MPC).
Rubin will submit the astrometric measurements represented in ``SSSource`` to the MPC in stages, so that association quality can be validated before each submission and the risk of reporting misassociations can be minimized.
