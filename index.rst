:tocdepth: 1

.. sectnum::

.. Metadata such as the title, authors, and description are set in metadata.yaml

.. TODO: Delete the note below before merging new content to the main branch.

.. note::

   **This technote is a work-in-progress.**

Abstract
========

Linked with: SITCOM-798 

This is the technote for the settling time after a slew  analysis on the TMA with M1M3. We measured the mirror cell settling time (position and rotation) after a slew. 

Currently the test fails marginally looking at movements for a 4 hour long soak test. The IMS repeatability and precision are measured on a specific data set and the requirements for it are PASSED too.

Requirements
------------

**LTS-88-REQ-0051**

Related SITCOM tickets
======================

SITCOM-1172: `M1M3 - analyze settling times after a slew statistically  <https://jira.lsstcorp.org/browse/SITCOM-1172>`__

LVV-11258: `LTS-88-REQ-0051-V-01: 3.12.1.5 Settling Time After a Slew_1 <https://jira.lsstcorp.org/browse/LVV-11258>`__


SITCOM-798: M1M3 - Settling time after a slew
=============================================

Requirement verified
--------------------

**LTS-88-REQ-0051**: The positioning system SHALL be able to
meet all its requirements within 3 seconds of ending a short
slew (3.5 degrees in 2 seconds)

*The requirement is for the system to have settled to the same (within precision) position after a short slew, within 3 seconds.*

Test Case
---------
`LVV-11258 <https://github.com/lsst-sitcom/notebooks_vandv/tree/tickets/SITCOM-798/notebooks/tel_and_site/subsys_req_ver/m1m3>`__ 

Plot the settling time of the M1M3 for X, Y, Z, RX, RY, and RZ.

The data comes from the EFD: imsData. The IMS is the
Independent Measurement System, a set of electronic
micrometers that measure the displacement of the M1M3 mirror
with respect to the cell. According to LTS-88 it has a 4 um
accuracy in XYZ and 3e-5 degree accuracy in RXRYRZ. 



Test Data
---------

- dayObs = 2023-12-20
- block = 146, soak tests (operations like)
- selected all events of type = SLEW ending in TRACKING, a total of 227 events

Results
-------

Out of the 227 events, 167 had at least

IMS XYZ position with azimuth and elevation reference. Vertical line denotes reference time (slew stop):


.. figure:: /_static/xyz_vs_azel.png
   :name: fig-xyzvsazel

   IMS XYZ during the slew, compared to azimuth and elevation from mount information. 

RXRYRZ rotation with azimuth and elevation reference. Vertical line denotes reference time (slew stop):

.. figure:: /_static/rxryrz_vs_azel.png
   :name: fig-rxryrzvsazel

   IMS RXRYRZ during the slew, compared to azimuth and elevation from mount information.

Settling behavior from the IMS measurements. IMS values during and after slew, with RMS behavior with respect to end of the plot, with a 3 second requirement window .

.. figure:: /_static/xsettle.png
   :name: fig-xsettle

   IMS x residual compared to value at slew stop, RMS, in mm.

.. figure:: /_static/ysettle.png
   :name: fig-ysettle

   IMS y residual compared to value at slew stop, RMS, in mm.

.. figure:: /_static/zsettle.png
   :name: fig-zsettle

   IMS z residual compared to value at slew stop, RMS, in mm.

.. figure:: /_static/xrotsettle.png
   :name: fig-xrotsettle

   IMS rotation in x residual compared to value at slew stop, RMS, in degrees.

.. figure:: /_static/yrotsettle.png
   :name: fig-yrotsettle

   IMS rotation in y residual compared to value at slew stop, RMS, in degrees.

.. figure:: /_static/zrotsettle.png
   :name: fig-zrotsettle

   IMS rotation in z residual compared to value at slew stop, RMS, in degrees.

Additional verification
-----------------------

Requirements
^^^^^^^^^^^^

**LTS-88-REQ-0128**: The IMS shall be able to measure the position of the mirror relative to the
mirror cell to an accuracy of +/- 4 micro m, repeatability of +/- 2 micro m and a resolution of
+/- 0.5 micro m in all three directions.

**LTS-88-REQ-0129**: The IMS SHALL have a minimum rotational accuracy of +/- 6 e-5 degrees,
repeatability of +/- 3 e-5 degrees and a resolution of +/- 8 e-6 degrees about all three axes.

**LTS-88-REQ-0131**: The IMS sampling rate SHALL be at least 5 Hz.

Test Data
^^^^^^^^^
*Looked for a sample range with visually stable behaviour between two slews.*

- dayObs = 2023-07-18
- time between UTC 05:03:00 and 05:03:30
- Duration = 30s
- Motion status TBC

Results
^^^^^^^

Calculated numpy.std over all measurements in range, for the six columns. Results are:

xPosition 1.10e-03 microns

yPosition 1.94e-01 microns

zPosition 5.63e-02 microns

xRotation 7.78e-07 degrees

yRotation 1.32e-06 degrees

zRotation 7.76e-07 degrees

Which verifies the repeatability (precision) requirements 0128 and 0129. Also the sampling rate 0131 is verified with data at 40 Hz. According to data recovered from EFD, the positional data has a resolution of 0.01 micro m and 1e-6 degrees.

Conclusions
===========

The requirement is passed in all 6 variables but in restrained conditions (50% speed) which are not nominal, so final test is TBD.

The M1M3 system IMS passes the repeatability and precision requirements.

Related documents
=================
`M1M3 Mirror Support Design Requirement Document LTS-88 <https://docushare.lsst.org/docushare/dsweb/Get/LTS-88/LTS-88.pdf>`__

.. Make in-text citations with: :cite:`bibkey`.
.. Uncomment to use citations
.. .. rubric:: References
.. 
.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa

