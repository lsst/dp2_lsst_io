
########################
Data Quality: Photometry
########################

The following plots show the photometric offset between calibration stars and the reference catalog.


.. raw:: html

   <h4 style="text-align:center; margin-bottom:10px;">
     Photometric offset by Filter
   </h4>

   <style>
     /* Make carousel arrows black */
     #photometryCarousel .carousel-control-prev-icon,
     #photometryCarousel .carousel-control-next-icon {
       filter: invert(0) brightness(0);
     }

     /* Bigger, darker indicator dots */
     #photometryCarousel .carousel-indicators button {
       width: 14px;
       height: 14px;
       background-color: #666;
       border-radius: 50%;
       border: none;
       opacity: 0.9;
     }

     #photometryCarousel .carousel-indicators .active {
       background-color: #333;   /* visible active dot */
       opacity: 1;
     }

     /* Anchor dots to the image width */
     #photometryCarousel .carousel-inner {
       position: relative;
     }

     /* Move dots BELOW the image and center them */
     #photometryCarousel .carousel-indicators {
       position: absolute;
       bottom: -55px;            /* lower the dots more */
       left: 50%;
       transform: translateX(-50%);
       display: flex;
       gap: 10px;
       width: auto;
       padding: 0;
       margin: 0;
       justify-content: center;  /* force centering */
     }

     /* Perfect vertical centering for arrows */
     #photometryCarousel .carousel-control-prev,
     #photometryCarousel .carousel-control-next {
       top: 50%;
       transform: translateY(-50%);
     }

     /* Horizontal arrow placement */
     #photometryCarousel .carousel-control-prev {
       left: -30px;
     }

     #photometryCarousel .carousel-control-next {
       right: -30px;
     }

     /* Add spacing below carousel */
     #photometryCarousel {
       margin-bottom: 60px;
     }
   </style>

   <div id="photometryCarousel" class="carousel slide"
        style="border: 1px solid #ccc; box-shadow: 0 0 8px rgba(0,0,0,0.2);"
        data-bs-ride="carousel">

   <div class="carousel-indicators">
     <button type="button" data-bs-target="#photometryCarousel" data-bs-slide-to="0" class="active"></button>
     <button type="button" data-bs-target="#photometryCarousel" data-bs-slide-to="1"></button>
     <button type="button" data-bs-target="#photometryCarousel" data-bs-slide-to="2"></button>
     <button type="button" data-bs-target="#photometryCarousel" data-bs-slide-to="3"></button>
     <button type="button" data-bs-target="#photometryCarousel" data-bs-slide-to="4"></button>
     <button type="button" data-bs-target="#photometryCarousel" data-bs-slide-to="5"></button>
   </div>


     <div class="carousel-inner">

       <div class="carousel-item active">
         <img src="../_images/photomDiff_u_offset.png" class="d-block w-100" alt="Photometric offset between calibration stars and the reference catalog, in u band">
       </div>

       <div class="carousel-item">
         <img src="../_images/photomDiff_g_offset.png" class="d-block w-100" alt="Photometric offset between calibration stars and the reference catalog, in g band">
       </div>

       <div class="carousel-item">
         <img src="../_images/photomDiff_r_offset.png" class="d-block w-100" alt="Photometric offset between calibration stars and the reference catalog, in r band">
       </div>

       <div class="carousel-item">
         <img src="../_images/photomDiff_i_offset.png" class="d-block w-100" alt="Photometric offset between calibration stars and the reference catalog, in i band">
       </div>

       <div class="carousel-item">
         <img src="../_images/photomDiff_z_offset.png" class="d-block w-100" alt="Photometric offset between calibration stars and the reference catalog, in z band">
       </div>

       <div class="carousel-item">
         <img src="../_images/photomDiff_y_offset.png" class="d-block w-100" alt="Photometric offset between calibration stars and the reference catalog, in y band">
       </div>

     </div>

     <button class="carousel-control-prev" type="button" data-bs-target="#photometryCarousel" data-bs-slide="prev">
       <span class="carousel-control-prev-icon" aria-hidden="true"></span>
     </button>

     <button class="carousel-control-next" type="button" data-bs-target="#photometryCarousel" data-bs-slide="next">
       <span class="carousel-control-next-icon" aria-hidden="true"></span>
     </button>

   </div>

   <p style="text-align:center; font-style:italic; margin-top:6px;">
     Photometric offset in u, g, r, i, z, and y-bands.
   </p>
