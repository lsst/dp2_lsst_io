#############################
Solar System processing (SSP)
#############################

The Solar System processing (SSP) pipeline links together detections of new and known solar system objects, and interfaces with the Minor Planet Center (MPC) for physical parameter derivation and orbit fitting.

The bulk of SSP runs as part of `Solar System Prompt Processing <https://prompt-products.lsst.io/processing/moving/ss_prompt.html>`_.
For DP2, a re-processing was performed that is similar to what will be done for the future annual Data Release processing (DRP).
This includes detection, association, and measurement on the processed and calibrated visit and difference images, which results in a single, static, well-characterized software release.
The DP2 SSP data products are thus more suitable for population studies of objects detected by LSST with orbits estimated using only LSST data, e.g., large-scale Solar System population studies and model debiasing.


.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    ss_association
    ss_linking