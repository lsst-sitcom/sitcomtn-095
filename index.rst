:tocdepth: 1

.. sectnum::

.. Metadata such as the title, authors, and description are set in metadata.yaml

.. TODO: Delete the note below before merging new content to the main branch.

.. note::

   **This technote is a work-in-progress.**

Abstract
========

This is the technote for the settling time after a slew  analysis on the TMA with M1M3. We measured the mirror cell settling time (position and rotation) after a slew. 

Currently the test fails marginally looking at movements for a 4 hour long soak test. The IMS repeatability and precision are measured on a specific data set and the requirements for it are PASSED too.

Requirements
------------

**LTS-88-REQ-0051**

Related SITCOM tickets
======================

SITCOM-798: `M1M3 - settling time after a slew  <https://jira.lsstcorp.org/browse/SITCOM-798>`__

SITCOM-1172: `M1M3 - analyze settling times after a slew statistically  <https://jira.lsstcorp.org/browse/SITCOM-1172>`__

LVV-11258: `LTS-88-REQ-0051-V-01: 3.12.1.5 Settling Time After a Slew_1 <https://jira.lsstcorp.org/browse/LVV-11258>`__


SITCOM-1172: M1M3 - Analyze settling times after a slew statistically
=====================================================================

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

Out of the 227 events, 167 had at least one failure, defined as one of the position or rotation columns from the IMS being above the repeatability requirement for the IMS at any point between the 1 second mark after the slew, and 15 seconds after it (checked for RMS and bias).


.. figure:: /_static/mean_position.png
   :name: fig-mean_position

   Value of the IMS position absolute bias for all axes combined at exactly 1 second after the slew stop. 

.. figure:: /_static/mean_position_xyz.png
   :name: fig-mean_position_xyz

   Value of the IMS position absolute bias for each axis at exactly 1 second after the slew stop. 

.. figure:: /_static/mean_rotation.png
   :name: fig-mean_rotation

   Value of the IMS rotation absolute bias for all axes combined at exactly 1 second after the slew stop. 

.. figure:: /_static/rms_position.png
   :name: fig-rms_position

   Value of the IMS position RMS for all axes combined at exactly 1 second after the slew stop. 

.. figure:: /_static/rms_rotation.png
   :name: fig-rms_rotation

   Value of the IMS position RMS for all axes combined at exactly 1 second after the slew stop. 

Conclusions
===========

The requirement is failed at 1 second after slew stop due to a failure in the yPosition column, on approximately 70% of the cases in which it averages a 3 micron displacement due to a slow drift of the cell. In addition, there seems to be multiple failures due to RMS or mean going over the requirement at some point in the following 15 second span.

Related documents
=================
`M1M3 Mirror Support Design Requirement Document LTS-88 <https://docushare.lsst.org/docushare/dsweb/Get/LTS-88/LTS-88.pdf>`__

.. Make in-text citations with: :cite:`bibkey`.
.. Uncomment to use citations
.. .. rubric:: References
.. 
.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa

