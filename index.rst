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


Statistical analysis of settling time of M1M3
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

The data comes from the EFD: MTM1M3.imsData. The IMS is the
Independent Measurement System, a set of electronic
micrometers that measure the displacement of the M1M3 mirror
with respect to the cell. According to LTS-88 it has a 4 um
accuracy in XYZ and 3e-5 degree accuracy in RXRYRZ. 

We also use azimuth and elevation data from MTMount.azimuth and MTMount.elevation. 

Test Data
---------

- dayObs = 2023-12-20
- block = 146, soak tests (operations like)
- selected all events of type = SLEW ending in TRACKING, a total of 226 events

Results
-------

Out of 223 events (some events were identified as not properly being of the type we are interested in), around 130 had at least one failure, defined as one of the position or rotation columns from the IMS being above the repeatability requirement for the IMS at any point between the 5 second mark after the slew start, and 20 seconds after it (checked for RMS and bias).

.. figure:: /_static/nb_failures.png
   :name: fig-nb_failures

The time in which the system settles is defined here as the last time the M1M3 IMS statistics (mean, RMS) misses the requirement in the 20 second window after the slew start. This is shown below for position and rotation. 

.. figure:: /_static/settletime_position_xyz.png
   :name: fig-settle_position_xyz

.. figure:: /_static/settletime_rotation_xyz.png
   :name: fig-settle_rotation_xyz

It looks like at 5 seconds after the slew start, in several events the IMS yPosition value is still drifting towards its settle position. There are also some specific cases for yRotation failures. This is the distribution of the events in azimuth and elevation, highlighting specifically where the yRotation and yPosition failures happen. There is a general tendency associated with slews happening at elevationsgreater than 60 degrees.

.. figure:: /_static/azel.png
   :name: fig-azel

Some examples of the yPosition bias drift, that usually just fail above the 5 s mark.

.. figure:: /_static/yposition_94.png
   :name: fig-yposition_94

.. figure:: /_static/yposition_127.png
   :name: fig-yposition_127

The yRotation bias failures also exhibit similar drifts, with adjustments to settling peaking at 6.5 seconds after slew start.

.. figure:: /_static/yrotation_94.png
   :name: fig-yrotation_94

If we remove the condition of the *bias* going above the requirement, and just focus on the jitter (RMS), the number of failures is smaller.

.. figure:: /_static/nb_failures_nobias.png
   :name: fig-nb_failures_nobias


Conclusions
===========

The requirement is failed using a threshold of 5 seconds after slew start due to a failure in the yPosition and yRotation columns predominantly, due to a slow drift of the cell. However, in a large majority of  cases settling happens in < 2 s later and just barely misses the requirement for the system. NB that we have included RMS *and* bias of the IMS value, despite not being strictly the specification, as we considered it relevant to highlight these slow drifts that may not incur in any jittering at all.

Related documents
=================
`M1M3 Mirror Support Design Requirement Document LTS-88 <https://docushare.lsst.org/docushare/dsweb/Get/LTS-88/LTS-88.pdf>`__

.. Make in-text citations with: :cite:`bibkey`.
.. Uncomment to use citations
.. .. rubric:: References
.. 
.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa

