
########################
Data Quality: Astrometry
########################

The following plots show the astrometric offset between calibration stars and the reference catalog.

Delta RA
========


.. raw:: html

   <h4 style="text-align:center; margin-bottom:10px;">
     Astrometric RA σ(MAD) by Filter
   </h4>

   <p style="text-align:center; font-size:0.9em; margin-top:-6px;">
     (interactive carousel)
   </p>

<style>
  /* Make carousel arrows black */
  #astrometryCarousel .carousel-control-prev-icon,
  #astrometryCarousel .carousel-control-next-icon {
    filter: invert(0) brightness(0);
  }
</style>

<style>
  /* Bigger, darker indicator dots */
  #astrometryCarousel .carousel-indicators button {
    width: 14px;
    height: 14px;
    background-color: #666;
    border-radius: 50%;
    border: none;
    opacity: 0.9;
  }

  #astrometryCarousel .carousel-indicators .active {
    background-color: #fff;
    opacity: 1;
  }

  /* Ensure indicator container is positioned correctly */
  #astrometryCarousel .carousel-indicators {
    bottom: 10px;
  }
</style>

   <div id="astrometryCarousel" class="carousel slide"
        style="border: 1px solid #ccc; box-shadow: 0 0 8px rgba(0,0,0,0.2);"
        data-bs-ride="carousel">

     <div class="carousel-indicators">
       <button type="button" data-bs-target="#astrometryCarousel" data-bs-slide-to="0" class="active"></button>
       <button type="button" data-bs-target="#astrometryCarousel" data-bs-slide-to="1"></button>
       <button type="button" data-bs-target="#astrometryCarousel" data-bs-slide-to="2"></button>
     </div>

     <div class="carousel-inner">

       <div class="carousel-item active">
         <img src="../_images/astromDiffRA_u_sigmaMAD.png" class="d-block w-100" alt="RA u-band sigma MAD">
       </div>

       <div class="carousel-item">
         <img src="../_images/astromDiffRA_g_sigmaMAD.png" class="d-block w-100" alt="RA g-band sigma MAD">
       </div>

       <div class="carousel-item">
         <img src="../_images/astromDiffRA_r_sigmaMAD.png" class="d-block w-100" alt="RA r-band sigma MAD">
       </div>

     </div>

     <button class="carousel-control-prev" type="button" data-bs-target="#astrometryCarousel" data-bs-slide="prev">
       <span class="carousel-control-prev-icon" aria-hidden="true"></span>
     </button>

     <button class="carousel-control-next" type="button" data-bs-target="#astrometryCarousel" data-bs-slide="next">
       <span class="carousel-control-next-icon" aria-hidden="true"></span>
     </button>

   </div>

   <p style="text-align:center; font-style:italic; margin-top:6px;">
     Astrometric scatter (σ(MAD)) in Right Ascension between calibration stars and the reference catalog, in u, g, and r-bands.
   </p>



Delta Dec
=========

.. figure:: images/astromDiffDec_u_sigmaMAD.png
    :width: 600
    :name: images/astromDiffDec_u_sigmaMAD
    :alt: Astrometric sigma MAD in Declination for calibration stars compared to the reference catalog, in u band.


    Figure 1: Astrometric scatter (sigma MAD) in Declination between calibration stars and the reference catalog, in u band.

.. figure:: images/astromDiffDec_g_sigmaMAD.png
    :width: 600
    :name: images/astromDiffDec_g_sigmaMAD
    :alt: Astrometric sigma MAD in Declination for calibration stars compared to the reference catalog, in g band.

    Figure 2: Astrometric scatter (sigma MAD) in Declination between calibration stars and the reference catalog, in g band.

.. figure:: images/astromDiffDec_r_sigmaMAD.png
    :width: 600
    :name: images/astromDiffDec_r_sigmaMAD
    :alt: Astrometric sigma MAD in Declination for calibration stars compared to the reference catalog, in r band.

    Figure 3: Astrometric scatter (sigma MAD) in Declination between calibration stars and the reference catalog, in r band.


.. raw:: html

   <div id="astrometryCarousel" class="carousel slide" data-bs-ride="carousel">
     <div class="carousel-inner">

       <div class="carousel-item active">
         <img src="../_images/astromDiffDec_u_sigmaMAD.png" class="d-block w-100" alt="DEC u-band sigma MAD">
       </div>

       <div class="carousel-item">
         <img src="../_images/astromDiffDec_g_sigmaMAD.png" class="d-block w-100" alt="DEC g-band sigma MAD">
       </div>

       <div class="carousel-item">
         <img src="../_images/astromDiffDec_r_sigmaMAD.png" class="d-block w-100" alt="DEC r-band sigma MAD">
       </div>

     </div>

     <button class="carousel-control-prev" type="button" data-bs-target="#astrometryCarousel" data-bs-slide="prev">
       <span class="carousel-control-prev-icon" aria-hidden="true"></span>
     </button>

     <button class="carousel-control-next" type="button" data-bs-target="#astrometryCarousel" data-bs-slide="next">
       <span class="carousel-control-next-icon" aria-hidden="true"></span>
     </button>
   </div>

