############
Observations
############

.. important::

   Data Preview 2 has not yet been released. This website is currently under development.


The sky coverage (fields), filters, and number of visits (cadence).

.. _observations-fields:

Sky coverage
============

.. figure:: images/observations-1-dp2map-visits.png
    :name: dp2_map
    :alt: DP2 coverage on the sky.

    Figure 1: A map of the DP2 coverage. Blue represents a density map of the number of visits; the quadrilaterals are 19-sided HEALPix, similar in size to the LSSTCam 9.6 square degree field of view. Open black squares mark the locations of tracts in the skymap, within which deep coadd images were made if there were a sufficient number of input visits.


.. figure:: images/observations-2-dp2map-depth.png
    :name: dp2_depth_map
    :alt: DP2 deep coadd image depth over the sky.

    Figure 2: A map of the i-band PSF limiting magnitude for DP2 deep coadd images. Deep coadd images are only made within the pre-defined tracts for which sufficient input visits exist. The total area covered by deep coadd images is approximatley 3000 square degrees.


Wide-fast-deep region
---------------------

The region of sky covered somewhat similarly to the LSST's future "wide-fast-deep" (WFD) stretches from RA 240-30 deg, over 20 deg span of Declination.
It is similar in that it includes low-dust and dusty regions within the plane of the Milky Way, where the depth is shallower, as seen in Figure 2.
There are three small fields within this WFD region, as labeled in Figure 1, making it particularly non-uniform in depth.
However this is still somewhat similar to the future LSST sky coverage, which won't be uniform in their depth either (especially years with rolling cadence).
Learn more about the WFD and the LSST strategy in the `Survey strategy documentation <https://survey-strategy.lsst.io/>`_.


Small fields
------------

Selected to provide LSSTCam commissioning-era images for science validation across variety of environments (crowded, dusty, galactic, extragalactic, clustering, nebulosity, and so on).
The small fields include M49 and Trifid-Lagoon, which produced the Rubin First Look images in June 2025.

.. csv-table:: Table 1: Small field survey area central coordinates.
   :header: "Field Name", "RA, Dec", "RA, Dec [deg]"
   :widths: 30, 30, 20
   :align: left

    "Abell 2764", "00h22m00s -49d00m00s", "5.5 -49"
    "DESI SV3 R1", "12h02m00s -00d18m00s", "180.5 -0.3"
    "M49", "12h25m12s +06d54m00s", "186.3 6.9"
    "Prawn", "16h54m00s -41d00m00s", "253.5 -41"
    "Trifid-Lagoon", "18h06m48s -23d54m00s", "271.7 -23.9"
    "New Horizons", "19h17m36s -20d12m00s", "289.4 -20.2"
    "Rubin SV 212 -7", "14h00m48s -06d06m00s", "210.2 -6.1"
    "Rubin SV 216 -17", "14h24m24s -16d42m00s", "216.1 -16.7"
    "Rubin SV 225 -40", "15h00m00s -39d30m00s", "225 -39.5"
    "Rubin SV 280 -48", "18h40m24s -48d00m00s", "280.1 -48"
    "Rubin SV 300 -41", "20h01m12s -41d00m00s", "300.3 -41"
    "Rubin SV 320 -15", "21h20m48s -15d06m00s", "320.2 -15.1"



Deep drilling fields
--------------------

The LSST Deep Drilling Field (DDF) program includes 5 single-pointing fields, chosen to maximize multi-wavelength coverage with pre-existing surveys.
The Euclid Deep Field South (EDFS) is larger and is split between an "a" and a "b" pointing which share the DDF visits.
While these fields were covered during commissioning, observations did not follow the `baseline DDF strategy <https://survey-strategy.lsst.io/baseline/ddf.html>`_ (primarily, the seasons were short).

.. csv-table:: Table 2: Deep drilling field central coordinates.
   :header: "Field Name", "RA, Dec", "RA, Dec [deg]"
   :widths: 30, 30, 20
   :align: left

    "DDF ELAIS S1", "00h38m00s -44d00m00s", "9.5 -44"
    "DDF XMM LSS", "02h22m24s -04d48m00s", "35.6 -4.8"
    "DDF ECDFS", "03h32m00s -28d06m00s", "53 -28.1"
    "DDF EDFS a", "03h56m48s -49d12m00s", "59.2 -49.2"
    "DDF EDFS b", "04h12m48s -47d48m00s", "63.2 -47.8"
    "DDF COSMOS", "10h00m24s +02d06m00s", "150.1 2.1"


.. _observations-filters:

Filters
=======



.. csv-table:: Table 3: Visit filter distribution by field.
   :header: "Field Name", "u", "g", "r", "i", "z", "y", "Total"
   :widths: 30, 10, 10, 10, 10, 10, 10, 10
   :align: left

    "DDF COSMOS", "105", "87", "173", "162", "110", "77", "714"
    "DDF ECDFS", "10", "61", "50", "96", "61", "11", "289"
    "DDF EDFS a", "7", "31", "33", "59", "33", "7", "170"
    "DDF EDFS b", "5", "37", "33", "57", "33", "7", "172"
    "DDF ELAIS S1", "57", "153", "128", "232", "141", "59", "770"
    "DDF XMM LSS", "42", "15", "8", "47", "10", "10", "132"
    "Abell 2764", "0", "6", "0", "2", "0", "0", "8"
    "DESI SV3 R1", "0", "0", "0", "3", "0", "0", "3"
    "M49", "234", "280", "378", "213", "0", "0", "1105"
    "New Horizons", "37", "53", "77", "116", "80", "33", "396"
    "Prawn", "193", "159", "139", "91", "30", "0", "612"
    "Rubin SV 212 -7", "0", "139", "231", "123", "0", "0", "493"
    "Rubin SV 216 -17", "0", "27", "28", "59", "0", "0", "114"
    "Rubin SV 225 -40", "307", "524", "391", "381", "239", "104", "1946"
    "Rubin SV 280 -48", "30", "29", "26", "30", "29", "0", "144"
    "Rubin SV 300 -41", "0", "8", "0", "0", "30", "0", "38"
    "Rubin SV 320 -15", "12", "47", "104", "259", "211", "77", "710"
    "Trifid-Lagoon", "231", "201", "129", "119", "9", "6", "695"
    "All", "1964", "4166", "4856", "7959", "5809", "3944", "28698"


.. _observations-epochs:

Epochs (nights)
===============

.. list-table:: Table 4: Number of nights, mean visits per night.
   :widths: 3 1 1
   :header-rows: 1

   * - Sky region
     - Epochs (nights)
     - Visits/epoch
   * - WFD
     - x
     - x
   * - M49
     - x
     - x
   * - COSMOS
     - x
     - x
   * - Add new here
     - x
     - x



.. _observations-tracts:

Coadd tracts
============

.. list-table:: Table 5: Coadd tract IDs for each field
   :widths: 3 3
   :header-rows: 1

   * - Field name
     - Tract IDs
   * - M49
     - x, x
   * - COSMOS
     - x
   * - Add new here
     - x
