.. _images-mask-planes:

###########
Mask planes
###########

In the LSST Science Pipelines, each processed image includes not only the measured flux values but also a companion bit mask image that records the condition of every pixel.
These mask planes encode information about detector defects, cosmic rays, saturation, missing data, and other effects that influence data quality.
Each named mask plane corresponds to a specific bit flag that can be set independently or in combination with others on a given pixel.
Bit values are assigned dynamically and may change.

Visit and difference images
===========================

.. toctree::
   :maxdepth: 1
   :titlesonly:

   visit_image_mask_planes

Deep and template coadds images
===============================

.. toctree::
   :maxdepth: 1
   :titlesonly:

   deep_coadd_mask_planes
