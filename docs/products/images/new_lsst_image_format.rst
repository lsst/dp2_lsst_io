.. _images-new:

##########################
What's new with DP2 images
##########################

DP2 image data products are written in a new file format and are represented in Python by new classes in the ``lsst.images`` package.

The new format still FITS-based, but it is more self-documenting, better at handling versioning changes, and implemented in a way that opens the door to other (i.e. non-FITS) file types in the future, such as ASDF or HDF5.

The new Python types are built around many of the same concepts as the ``lsst.afw.image`` classes they are replacing, but with what we hope is a more intuitive and natural interface, especially for new users and those familiar with Astropy concepts.
The new `lsst.images` package is installable via ``pip`` and hence the new files can be read into Python code without having access to the full LSST Science Pipelines software stack.

The LSST pipeline code still uses the ``lsst.afw.image`` types for the most part; we are in the early stages of our own internal migration to the new types.
This means some advanced users will still encounter them when working with DP2 data (many `lsst.images` classes have a ``to_legacy`` method to convert to their counterpart ``lsst.afw`` type, for exactly this reason).

The new LSST FITS meta-format
=============================

The new file format still typically starts with the same sequence of (empty, metadata-only) PRIMARY, IMAGE, MASK, and VARIANCE Header Data Units (HDUs).
LSST files from DP1 and earlier stored the rest of their content (point spread function models, coordinate transforms, etc.) in mostly-inscrutable FITS binary table HDUs.
The next few HDUs depend on the data product - ``visit_image`` differs from ``deep_coadd`, for example - but generally hold at least **somewhat** interpretable data (e.g. a data cube or table with PSF basis images), and critically the file always ends with two special HDUs:

- a ``JSON`` binary table with a single variable-length byte column, holding a UTF-encoded JSON tree with structured metadata and links to the binary data in the rest of the file;

- an ``INDEX`` binary table that holds the byte-offsets and sizes of every other HDU, along with a few important header keywords.

The ``JSON`` tree is a human-and-machine-readable guide to the file as a whole, and it includes extra information about the IMAGE, MASK, and VARIANCE planes that doesn't have a standard FITS representation (or for which the standard FITS representation may be an approximation, as is the case for some of our coordinate transforms).
Together with the ``INDEX`` table, this improves our ability to do partial fast reads: we can quickly jump to the last two HDUs (thanks to offsets in the PRIMARY header), read those, and then quickly identify which byte ranges of the other HDUs need to be read to obtain a particular subimage or component.

.. TODO: link to schemas and detailed file-format docs in lsst.images once we have them.

The new `lsst.images` classes
=============================

As it's designed to stand on its own, `lsst.images` doesn't rely on any of the types in `lsst.afw` get attached to our images.
It has its own `Box`, `PointSpreadFunction`, `SkyProjection`, etc. (often backed by other packages, like Piff for PSFs, Starlink AST for coodinate transforms, and Shapely for polygons).

.. TODO: link to the "For afw users" docs in lsst.images.

.. TODO: add and link to a "For astropy users" doc page in lsst.images.  This is where we will discuss the difference in how LSST and Astropy approach subimage indexing and hence SkyProjection/WCS.

Public LSST data products generally have their own Python class in `lsst.images`, allowing us to tailor those classes better to how we have characterized them.
These almost always inherit from `~lsst.images.MaskedImage`, which combines the image itself with a multi-plane bitmask, and a variance image.

An `~lsst.images.Image` is conceptually just a `numpy.ndarray` with optional units (via `astropy.units`), a `~lsst.images.SkyProjection`, and an ``yx0`` offset that allows its pixel origin to be something other than ``(0, 0)``.

Cell-based coadds
=================

The DP2 ``deep_coadd`` data product differs from its predecessors in more than just its file format and associated Python type: we have also switched to cell-based coaddition, in which each patch is subdivided into a grid of 150x150 pixel cells and whether a visit contributes to the coadd is decided **mostly** on a cell-by-cell basis.
This makes the PSF model and other coadded properties of the coadd piecewise constant to good approximation (the cells are small enough that we can neglect their average spatial variation within a cell).
This `~lsst.images.cells.CellGrid`) is thus the main organizing principle for the `~lsst.images.cells.CellCoadd` class that is used to represent a ``deep_coadd``:

- `~lsst.images.cells.CellPointSpreadFunction` represents the per-cell constant PSF;
- `~lsst.images.cells.CellField` is used to represent the aperture correction for each photometry algorithm we run;
- `~lsst.images.cells.CoaddProvenance` has a table of the per-cell weights, overlap and mask fractions, and PSF shape measurements of the inputs to each cell (as well as a separate table of input `{visit, detector}` quantities that can be joined to it).

Other aspects of the coadd don't reflect the cell structure as strongly; the image, mask, and variance planes stitch all of the cells together into a seamless image, and it's moderately rare for the cell boundaries to be visible in the properties of the main image by eye.
The background models subtracted from and attached to the coadd are fit to these stitched images, and do not reflect cell structure at all.
