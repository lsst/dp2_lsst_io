.. _portal-104-2:

#######################################
104.2. The results coverage chart panel
#######################################

For the Portal Aspect of the Rubin Science Platform (RSP) at data.lsst.cloud.

**Data Release:** Data Preview 2

**Last verified to run:** 2026-08-17

**Learning objective:** View the coverage map and lightcurves in the results interface.

**LSST data products:** `Object` table

**Credit:** Originally developed by the Rubin Community Science team.
Please consider acknowledging them if this tutorial is used for the preparation of journal articles, software releases, or other tutorials.
DOI: `10.11578/rubin/dc.20250909.20 <https://doi.org/10.11578/rubin/dc.20250909.20>`_

**Get Support:** Everyone is encouraged to ask questions or raise issues in the `Support Category <https://community.lsst.org/c/support/6>`_ of the Rubin Community Forum.
Rubin staff will respond to all questions posted there.

**Terminology:**

* `HiPS <https://aladin.cds.unistra.fr/hips/>`_: Hierarchical Progressive Surveys
* `MOC <https://www.ivoa.net/documents/MOC/>`_: Multi-Order Coverage map
* `ADQL <https://www.ivoa.net/documents/ADQL>`_: Astronomy Query Data Language
* `HEALPix <https://healpix.sourceforge.io/>`_: Hierarchical Equal Area isoLatitude Pixelation of a sphere
* `regions <https://ds9.si.edu/doc/ref/region.html>`_ file: a standard format for marking regions in an image
* `WCS <https://fits.gsfc.nasa.gov/fits_wcs.html>`_: World Coordinate System (the convention that defines the coordinates per pixel)
* PNG: Portable Network Graphic

----

**1. Log in to the Portal Aspect of the RSP.**
Log in to the Portal, click on the "DP1 & DP2 Catalogs" tab, and switch to the ADQL interface.

**2. Execute a query.**
Enter the query below into the ADQL box, and click "Search".
This query returns coordinates and magnitudes for objects near the center of the ECDFS field that are brighter than 22 mag in *g* and *r*.

.. code-block:: SQL

  SELECT objectId, coord_dec, coord_ra, g_cModelMag, r_cModelMag
  FROM dp2.Object
  WHERE CONTAINS(POINT('ICRS', coord_ra, coord_dec),
        CIRCLE('ICRS', 53.0, -28.0, 1.0)) =1
        AND g_cModelMag < 22 AND r_cModelMag < 22


**3. View the coverage chart**.
The default view is a HEALPix grid showing the number of returned objects per grid region.
Small color squares mark individual objects outside the grid (bottom center).
The background is the DP2 *gri* ``pretty_coadd`` HiPS map.

.. figure:: images/portal-104-2-1.png
    :name: portal-104-2-1
    :width: 400
    :alt: The default view of the coverage chart.

    Figure 1: The coverage chart panel in the results interface.


**3. Mouse-over for pop-up notes.**
In the coverage chart panel (Figure 1) use the mouse to hover over the menus and icons to see pop-up explanations of the functionality.

**4. Explore menus and icons.**
In the coverage chart panel (Figure 1) click on each of the drop-down menus and icons listed below, and see the pop-up windows of options and tools.

* A: **HiPS/MOC** options to change the underlying HiPS map or add a MOC layer.
* B: **WCS** options to choose the orientation and projection of the underlying HiPS map.
* C: **Zoom** in, out, or to fit the window. (It is also possible to use the mouse to zoom.)
* D: **WCS Coordinates** of the mouse position. Click the box-and-arrow icon to view coordinates in a separate window.
* E: **Toggle lock** to "on" to lock the WCS Coordinates to the location of the last mouse click, and "off" for continuous position.
* F: **Tools** menu with options to save, reset, or orient the display; add compass, ruler, points, etc.
* G: **Color table** menu to choose a different color map.
* H: **Recenter** by entering coordinates for the desired display center.
* I: **Spatial selection** draw a box or circle to select marked objects.
* J: **Overlays** manipulation to change the options or color for the HEALPix and points overlay.
* K: **Image lock** and alignment (tools for when multiple images are displayed).
* L: **Expand panel** to have the coverage chart take the full browser window.
* M: **Data product**: switch to the forced-photometry lightcurve for the selected object.
* N: **Legend** an alternative interface to the Overlays menu.

**5. Zoom in.**
Above C in Figure 1, click the "zoom in" icon (magnifying glass with a + inside) or use the mouse to zoom in until individual object markers are displayed instead of the HEALPix grid (Figure 2).

**6. Select a single object.**
Click on any individual marker, and notice that it's row will be highlighted orange in the table panel and its symbol will be orange in the active chart (Figure 2).

.. figure:: images/portal-104-2-2.png
    :name: portal-104-2-2
    :alt: The new view of the zoomed-in coverage chart.

    Figure 2: The coverage chart panel, zoomed in to show individual markers, with one object selected. The same object is highlighted in the table and active chart.


**7. Switch to the object's lightcurve.**
Click on the "Data Products" tab to view the ``psfFlux`` (point-source forced flux measured on the visit image, in units of nanojanskies) vs. ``expMidptMJD`` (the modified julian date at the midpoint of the visit image, in days).
This is the lightcurve for the selected object.
Notice that all points for all filters currently appear as blue circles; work is in progress to use different colors and symbols as the default presentation of time-series data in the Portal.
Until then, click on "Table" and select a given filter, then switch back to the "Chart" to see a single-filter lightcurve which will provide a better impression of the true variability.

.. figure:: images/portal-104-2-3.png
    :name: portal-104-2-3
    :alt: The object's forced PSF flux lightcurve.

    Figure 3: The coverage chart panel, with the "Data Product" tab selected, shows the forced PSF flux lightcurve for the selected object, with all filters together as round blue markers.


**8. Select multiple objects.**
Switch back to the coverage chart by clicking the "Coverage" tab, and zoom-in on an interesting region of sky such as shown in Figure 4.
Under I in Figure 1, click on the "spatial selection" icon, select a cone selection, and draw a circle around objects of interest, as in Figure 4.
A new sub-menu of icons will appear above the image.

.. figure:: images/portal-104-2-4.png
    :name: portal-104-2-4
    :alt: The new view of the zoomed-in coverage chart.

    Figure 4: The coverage chart panel, zoomed in to show individual markers, with one object selected.


**9. Explore the spatial selection sub-menu.**

* O: **Mark data in area as selected**; corresponding rows in the table will be selected.
* P: **Filter in the selected area**; table and active chart will only contain selected objects.
* Q: **Zoom to fit selected area**; zooms-in on the coverage chart only (not the active chart).
* R: **Recenter image to selected area**; recenters the coverage chart only.
* S: **Search this area** opens a pop-up window with further options, not all of which are relevant to all datasets (e.g., the mention of "TruthSummary" is only relevant for Data Preview 0, which was based on simulated data).


**10. Option: save a PNG of the coverage chart.**
Click on the tools icon (F in Figure 1) and select the disk icon next to "Save...".
Leave the default selection of PNG file and click "Save".
An image of the coverage chart will automatically download.
Note the option to export the overlays as a regions file.

**11. Reset the coverage chart.**
Click on the tools icon (F in Figure 1) and select the circular arrow icon next to "Save..." to restore to default options.
