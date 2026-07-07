.. _moving-association:

################################
Source detection and association
################################

Moving objects are detected as part of :doc:`/processing/dia/index`.
Difference-image detections with signal-to-noise ratio of at least 5, ``DiaSources``, are associated with either a static-sky time-domain object (``DiaObject``) or a moving object (``SSObject``).
In both cases, source association uses a radius of 1 arcsecond, without taking positional uncertainties into account.
In DP2 this yields approximately 4 million associations.

Because a single known moving object predicted position may fall within the 1-arcsecond association radius for multiple nearby ``DiaSources``, a ``diaDistanceRank`` field is included in the ``ssSource`` table with the rank of the ``diaSourceId`` -identified source in terms of its closeness to the predicted SSO position.
If the ``diaSourceId`` is the nearest ``DiaSource`` to this SSO prediction, ``diaDistanceRank`` = 1 would be set.
If it is the second nearest, it would be 2, etc.
In this framework, multiple ``DiaSources`` may include the ``ssSource`` record for the same known solar system object, but each would have a different value assigned for the ``diaDistanceRank`` according to their proximity to the predicted position of that SSO.
To find the best association match between a given ``diaSourceId`` to the predicted position of a known solar system object, filter on ``diaDistanceRank`` == 1 in the ``SSSource`` table.
In addition, because ``SSSources`` (detections) are attributed to ``SSObjects``, a given ``ssObjectId`` may have ``SSSources`` associated to that ``ssObjectId`` that include different ``diaDistanceRank`` values.
Similarly in this case, to find the best association match between a given ``ssObjectId`` to the predicted position of a known SSO, filter on ``diaDistanceRank`` == 1 in the ``SSSource`` table.

Also included in the ``ssSource`` catalog is the ephemeris offset (``ephOffset``) that measures the total observed versus predicted angular separation on the sky between the observed ``DiaSource`` and the predicted position of a known solar system object.
This field is also useful for quality control, association confidence, and filtering uncertain matches.
The along-track and cross-track ephemeris offsets (``ephOffsetAlongTrack`` and ``ephOffsetCrossTrack``, respectively) as well as the RA and Dec ephemeris offsets (``ephOffsetRa`` and ``ephOffsetDec``, respectively) between the observed ``DiaSource`` and the predicted position of a known solar system object are also included in the ``ssSource`` catalog.

To decipher ambiguous associations, the ``diaDistanceRank``, ephemeris offsets, and multiple associated detections can be used.
Orbit-fitting, performed by the IAU Minor Planet Center (MPC), after ``DiaSource`` detections associated with known solar system objects in the above ranked system are submitted to the MPC and subsequently ingested by the Solar System Processing pipelines in the ``mpc_orbits`` catalog, will provide the most concrete solar system object association.