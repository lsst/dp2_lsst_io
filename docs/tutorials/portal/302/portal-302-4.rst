.. Review the README on instructions to contribute.
.. Review the style guide to keep a consistent approach to the documentation.
.. Static objects, such as figures, should be stored in the _static directory. Review the _static/README on instructions to contribute.
.. Do not remove the comments that describe each section. They are included to provide guidance to contributors.
.. Do not remove other content provided in the templates, such as a section. Instead, comment out the content and include comments to explain the situation. For example:
	- If a section within the template is not needed, comment out the section title and label reference. Do not delete the expected section title, reference or related comments provided from the template.
    - If a file cannot include a title (surrounded by ampersands (#)), comment out the title from the template and include a comment explaining why this is implemented (in addition to applying the ``title`` directive).

.. This is the label that can be used for cross referencing this file.
.. Recommended title label format is "Directory Name"-"Title Name" -- Spaces should be replaced by hyphens.
.. _Tutorials-Examples-DPX-Aspect-Y:
.. Each section should include a label for cross referencing to a given area.
.. Recommended format for all labels is "Title Name"-"Section Name" -- Spaces should be replaced by hyphens.
.. To reference a label that isn't associated with an reST object such as a title or figure, you must include the link and explicit title using the syntax
.. A warning will alert you of identical labels during the linkcheck process.


###########################################
302.4. Faint proper motion source selection
###########################################

For the Portal Aspect of the Rubin Science Platform at data.lsst.cloud.

**Data Release:** DP2

**Last verified to run:** 2026-11-14

**Learning objective:** Use the ``IsolatedStarStellarMotions`` table to help identify high proper motion sources beyond Gaia's depth.

**LSST data products:** ``IsolatedStarStellarMotions``

**Credit:** Originally developed by the Rubin Community Science team.
Please consider acknowledging them if this tutorial is used for the preparation of journal articles, software releases, or other tutorials.
DOI: `10.11578/rubin/dc.20250909.20 <https://doi.org/10.11578/rubin/dc.20250909.20>`_
The NOIRLab Source Catalog is described in `Nidever et al. 2018 <https://iopscience.iop.org/article/10.3847/1538-3881/aad68f>`_. and `Nidever et al. 2021 <https://iopscience.iop.org/article/10.3847/1538-3881/abd6e1>`_. This research used services or data provided by the Astro Data Lab, which is part of the Community Science and Data Center (CSDC) Program of NSF NOIRLab (`Fitzpatrick et al. 2014 <https://doi.org/10.1117/12.2057445>`_; `Nikutta et al. 2020 <https://doi.org/10.1016/j.ascom.2020.100411>`_). NOIRLab is operated by the Association of Universities for Research in Astronomy (AURA), Inc. under a cooperative agreement with the U.S. National Science Foundation.

**DOI:** `10.11578/rubin/dc.20250909.20 <https://doi.org/10.11578/rubin/dc.20250909.20>`_

**Get Support:** Everyone is encouraged to ask questions or raise issues in the `Support Category <https://community.lsst.org/c/support/6>`_ of the Rubin Community Forum. Rubin staff will respond to all questions posted there.


.. _DPX-Aspect-Y-Intro:

Introduction
============


**Related tutorials:** See the Portal 106-series tutorial regarding user-uploaded tables.


.. _DPX-Aspect-Y-1:

1. The first section
====================

:download:`Download the table of high proper motion candidates <../../../../_static/tutorials/dp2_302_4_NSC_candidates.csv>`.

**1.1. Log in to the Portal Aspect of the Rubin Science Platform.**
In a browser, go to the URL `data.lsst.cloud <https://data.lsst.cloud>`_ and select the Portal Aspect.
Follow the process to log in.

**1.2. Select the X tab in the Portal.**
*Explanation of the setup, reference to Figure 1.*

**1.3. Execute the ADQL query.**
*Explanation of the query, reference to the code block below.*

.. code-block:: SQL

    SELECT g_H, r_H, i_H, z_H
    FROM dp03_catalogs_10yr.SSObject
    WHERE ssObjectId > 8660000000000000000



.. _DPX-Aspect-Y-2:

2. The second section
=====================

**2.1.** *And so on.*



.. _DPX-Aspect-Y-exercises:

X. Exercises for the learner
============================

**X.1. A clear, achievable task.**

**X.2. Another clear, achievable task.**
