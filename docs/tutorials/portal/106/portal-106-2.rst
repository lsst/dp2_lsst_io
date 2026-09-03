.. _portal-106-2:

###############################################
106.2. Upload a table and spatially cross-match
###############################################

For the Portal Aspect of the Rubin Science Platform at data.lsst.cloud.

**Data Release:** Data Preview 2

**Last verified to run:** 2026-09-03

**Learning objective:** How to upload a table and cross-match by coordinate.

**LSST data products:** ``Object`` table

**Credit:** Originally developed by the Rubin Community Science team.
Please consider acknowledging them if this tutorial is used for the preparation of journal articles, software releases, or other tutorials.
DOI: `10.11578/rubin/dc.20250909.20 <https://doi.org/10.11578/rubin/dc.20250909.20>`_

**Get Support:** Everyone is encouraged to ask questions or raise issues in the `Support Category <https://community.lsst.org/c/support/6>`_ of the Rubin Community Forum.
Rubin staff will respond to all questions posted there.

----

**1. Log in to the RSP and enter the Portal Aspect.**
In a web browser go to data.lsst.cloud, select the Portal Aspect, and log in.

**2. Select DP2 Catalogs tab.**
Navigate to the "DP1 & DP2 Catalogs" tab in the Portal UI to create an ADQL query from the DP2 catalogs.

**3. Enter Constraints.**
Check the box to the left of the "Spatial" section (uncheck the other two if checked), and click on the "Multi-object" button next to "Spatial Type". This will make a pop-up window appear with the interface to upload a table.


.. figure:: images/portal-106-1-1.png
    :name: portal-106-1-1
    :alt: The interface to upload a table.

    Figure 1: The interface to upload a table.

**4. Create a table to upload.**
Copy the example table below and save it as a CSV file called ``dp2_106_1_user_table.csv``. Avoid using the "+" prefix for positive infinity (i.e., "+inf") in any of your columns. These values are not recognized as valid float values, and the entire column will be interpreted as a character type.

.. code-block::

    SDSS_objid,ra,dec
    1237680065347649938, 344.872589288903, -5.29412953356062
    1237680065347649939, 344.877186896095, -5.18342606769438
    1237680065347649940, 344.877794530448, -5.17340032770887
    1237680065347649937, 344.873602385501, -5.26745861467814
    1237680065347649936, 344.873036222136, -5.26977387877352
    1237680065347649935, 344.876528103134, -5.128174209936
    1237680065347649934, 344.874508490362, -5.18819130379492
    1237680065347649933, 344.874446935363, -5.13784242229324
    1237680065347649932, 344.871377794791, -5.25018994406145
    1237680065347649931, 344.870053717524, -5.29090967498696

