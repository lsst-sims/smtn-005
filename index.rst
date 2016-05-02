..
  Content of technical report.

  See http://docs.lsst.codes/en/latest/development/docs/rst_styleguide.html
  for a guide to reStructuredText writing.

  Do not put the title, authors or other metadata in this document;
  those are automatically added.

  Use the following syntax for sections:

  Sections
  ========

  and

  Subsections
  -----------

  and

  Subsubsections
  ^^^^^^^^^^^^^^

  To add images, add the image file (png, svg or jpeg preferred) to the
  _static/ directory. The reST syntax for adding the image is

  .. figure:: /_static/filename.ext
     :name: fig-label
     :target: http://target.link/url

     Caption text.

   Run: ``make html`` and ``open _build/html/index.html`` to preview your work.
   See the README at https://github.com/lsst-sqre/lsst-report-bootstrap or
   this repo's README for more info.

   Feel free to delete this instructional comment.

:tocdepth: 1

Introduction
============

LSST has an off-the-shelf Canon camera with a fisheye lens operating at the site. This camera takes 30 second exposures throughout the night and twilight time. The data is archived at NCSA and can be accessed from lsst-dev.ncsa.illinois.edu:/lsst/all-sky.  Each night generates about 30 gigabytes of data (gzipped).  The Canon simultaneously takes images in R, G, B filters.

Photometry with Canon
=====================

XXX-There is a standard astronomy pipline for reducing the all-sky data (i.e., a random hodge-podge of scripts).  This pipeline converts each Canon cr2 file to three .fits files, runs SExtractor, and fits the coordinate system.  Stellar magnitudes of ~2000 known birght stars are recorded along with the sky brightness measured in an anulus around each star.

We attempted to use the stellar photometry catalog to run ubercal on the system.  Unfortunatly, in cloudy conditions the coordinate solution fails and the stars become mis-identified. Ubercal is not robust against this type of error.

Limits of Canon Photometry
--------------------------

XXX--spot checking the results of apeture photometry, 

If we want to generate a transparency map to pass to the LSST scheduler, we need to decide on the following parameters:
* Spatial resolution on the celestial sphere
* Temporal resolution (setting the possible exposure time)
* Maximum extinction the maps should reliably extend to
* Precision of the extinction maps (e.g., plus-or-minus 0.1 mags of extinction at 1 mag of extinction)

These parameters combined with the stellar luminosity function at the galactic poles set the requirements on the all-sky camera photometry.

Cloud Observations With Median Filtered All-Sky Images
======================================================

Having established that photometry of bright stars is not adequate, we turn to the possibility of using the all-sky data to at least make masks of cloudy regions of the sky.

For this, we re-reduced the all-sky frames of all of 2015 and the first 4 months of 2016.  We use a single coordinate fit assigning each pixel to a fixed altitude and azimuth. We then convert to RA,Dec for the given exposure and take a median value for all the image pixels in each HEALpixel with an nside=32 (XXX--blah resolution).  This median filtering eliminates stars from the images, and compresses the data so that it only takes XXX Gb for the all the maps.

