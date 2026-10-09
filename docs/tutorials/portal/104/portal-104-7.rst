.. _portal-104-7:

#################################################
104.7. Saving portal visualizations as JSON files
#################################################

For the Portal Aspect of the Rubin Science Platform (RSP) at data.lsst.cloud.

**Data Release:** Data Preview 2

**Last verified to run:** 2026-10-09

**Learning objective:** Make a color-magnitude diagram and save it as JSON file.

**LSST data products:** ``Object`` table

**Credit:** Originally developed by the Rubin Community Science team.
Please consider acknowledging them if this tutorial is used for the preparation of journal articles, software releases, or other tutorials.
DOI: `10.11578/rubin/dc.20250909.20 <https://doi.org/10.11578/rubin/dc.20250909.20>`_

**Get Support:** Everyone is encouraged to ask questions or raise issues in the `Support Category <https://community.lsst.org/c/support/6>`_ of the Rubin Community Forum.
Rubin staff will respond to all questions posted there.

**Advisory:** Saving plots as JSON files offers one primary advantage over standard static image formats. It preserves the “live” state of the visualization rather than flattening it into static pixels. Because the file is saved as structured text rather than a “dead” image, it can be fully reconstructed in a Notebook later and use Python to programmatically change titles, adjust styles, or add annotations without needing to regenerate the plot from scratch.

**Related tutorials:** The 300-level Notebook tutorial demonstrates how to enhance JSON-based visualizations.

----

**1. Log in to the Portal aspect of the Rubin Science Platform and execute a query.** Go to the Portal’s "DP1 & DP2 Catalogs" tab. Switch to the ADQL interface. Copy-paste the query below into the box, will retrieve PSF photometry in *g* and *r* bands for the point-like objects with signal-to-noise ratio > 5 in both bands around a galaxy, NGC 6822. Click "Search".

.. code-block:: SQL

        SELECT g_psfMag, r_psfMag
        FROM dp2.Object
        WHERE CONTAINS(POINT('ICRS', coord_ra, coord_dec),
        CIRCLE('ICRS', 296.2, -14.8, 1))=1
        AND (refExtendedness =0)
        AND g_psfFlux/g_psfFluxErr > 5
        AND r_psfFlux/r_psfFluxErr > 5

**2. Plot a color-magnitude diagram.**
Add a new chart (click on the plus sign) and select the "Heatmap" plot type. Use color (``g_psfMag``-``r_psfMag``) on the x-axis and magnitude (``r_psfMag``) on the y-axis. Select 200 bins in X and 200 bins in Y. Set the X Min, X Max values to -0.5, 2, and the Y Min, Y Max values to 15, 26. Select "reverse" under "Chart Options" for the y-axis to display brighter magnitudes (i.e., lower numbers) toward the top of the plot.

.. figure:: images/portal-104-7-1.png
    :name: portal-104-7-1
    :width: 500
    :alt: g-r versus r color-magnitude diagram for stars

    Figure 1: *g*-*r* versus *r* color-magnitude diagram for stars in the NGC 6822 field.

**3. Save the plot as a JSON file.**
First, close the default (``g_psfMag`` vs. ``r_psfMag``) plot, and then click the floppy disk icon at the top of the plot interface, then select "JSON" from the format options to save the plot. The file will be saved to your local computer.

.. figure:: images/portal-104-7-2.png
    :name: portal-104-7-2
    :alt: Screenshot demonstrating how to save the plot as a JSON file

    Figure 2: Screenshot demonstrating how to save the plot as a JSON file.