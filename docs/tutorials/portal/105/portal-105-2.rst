.. _portal-105-2:

#################################################
105.2. Use the Firefly image viewer (Coming Soon)
#################################################

For the Portal Aspect of the Rubin Science Platform at data.lsst.cloud.

**Data Release:** DP2

**Last verified to run:** 2026-09-17

**Learning objective:** Use the components of the Firefly image viewer.

**LSST data products:**

**Credit:** Originally developed by the Rubin Community Science team.
Please consider acknowledging them if this tutorial is used for the preparation of journal articles, software releases, or other tutorials.

**Get Support:** Everyone is encouraged to ask questions or raise issues in the `Support Category <https://community.lsst.org/c/support/6>`_ of the Rubin Community Forum.
Rubin staff will respond to all questions posted there.

----

**1. Execute an ADQL query for deep coadd images.**
Log in to the Portal Aspect, select the "DP1 & DP2 Images" tab and click on "Edit ADQL" at upper right.
Enter the following ADQL statement and click "Search" at lower left.
This query will return all *r*-band deep coadd images that overlap coordinates RA, Dec = 53.0, -28.0 degrees.

.. code-block:: SQL

  SELECT dataproduct_type,dataproduct_subtype,calib_level,
           lsst_band,em_min,em_max,lsst_tract,lsst_patch,
           lsst_filter,s_ra,s_dec,s_fov,obs_id,obs_collection,
           o_ucd,facility_name,instrument_name,obs_title,
           s_region,access_url,access_format
  FROM ivoa.ObsCore
  WHERE obs_collection = 'LSST.DP2'
        AND dataproduct_subtype = 'lsst.deep_coadd'
        AND lsst_band = 'r'
        AND CONTAINS(POINT('ICRS', 53.0, -28.0), s_region)=1

**2. Examine the various image planes.**
Hide the active chart window.
Cycle through the available image planes by using the arrows at the upper left of the image, as seen in Figure 1.

.. figure:: images/portal-105-2-1.png
    :name: portal-105-2-1
    :alt: The buttons to cycle through image planes.

    Figure 1: The buttons to cycle through image planes in Firefly. They appear above the displayed image.

By default the "IMAGE" plane (HDU #1) will be displayed. By pressing the left and right arrow buttons, one can toggle through additional image planes that are associated with the image.

Advance to the third plane, the VARIANCE. Using the stretch drop down tool, change the image stretch to "Z Scale Linear". The result should look like Figure 2.

.. figure:: images/portal-105-2-2.png
    :name: portal-105-2-2
    :alt: The VARIANCE image plane, showing pixel variances in nJy-squared units.

    Figure 2: The VARIANCE image plane with "Z Scale Linear" stretch applied. Features corresponding to the size of cells are seen, showing where the noise properties change due to different numbers of input visit images contributing to different cells.

The coadd image planes are:

- IMAGE (HDU #1) -- the deep coadd image.
- MASK (HDU #2) -- the mask plane showing pixels that have been masked for various reasons.
- VARIANCE (HDU #3) -- the variance plane corresponding to the pixel variance (in nJy<sup>2</sup>).
- MASK_FRACTION (HDU #4) -- the fraction of input visit images that were masked.
- NOISE_REALIZATION (HDU #5) -- a noise realization to be used for shear measurements.
- BACKGROUND/FIELDS/PRETTY/DATA (HDU #10) -- the difference between the deep coadd background and the "pretty coadd" background. Restoring this will roughly reproduce the "pretty coadds" that are used in the HiPS maps.
- BACKGROUND/FIELDS/OBJECT/DATA (HDU #11) -- the background that was subtracted from the deep coadd image.

Notice that the background images are binned to lower resolution than the full images.