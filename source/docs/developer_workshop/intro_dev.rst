.. comment out this Section (by putting '|updatedisclaimer|' on top) if file is not uptodate with release

.. _dev_shop_intro:

********************************
Introduction to |qg| Development
********************************

.. QGIS Dev splash, Swiss Army knife

.. As people are installing, have everyone introduce themselves and short blurb
   about plans for QGIS Dev and what languages they work with

.. figure:: /static/developer_workshop/22_qgis_dev-splash.png
   :align: center
   :width: 40em

See also: :ref:`dev_guide`

Short history of |qg| development
=================================

.. Gary slide

Gary Sherman began development of Quantum GIS in early 2002,

.. figure:: /static/developer_workshop/22_qgis_0.0.5.png
   :align: center
   :width: 40em

   Quantum GIS, circa 2002

Two videos of Tim Sutton interviewing Gary on 29 September 2011:
`Part 1 <http://youtu.be/-CuSMDjhmow>`_ and `Part 2 <http://youtu.be/OeeF7bXQRsc>`_.

It became an incubator project of the `Open Source Geospatial Foundation <http://www.osgeo.org/>`_
in 2007. Version 1.0 was released in January 2009.

In 2002 there were two people (just Gary until October). There were significant
jumps in developer interest in 2004 and 2008.

In 2004 there were a number of releases that added significant functionality.
And another following an announcement at FOSS4G 2007 in Victoria about the
release of 0.9, which included Python support. This opened up plugin development
to a whole new group of people and contributed to growth in 2008.

Using the git log leading up to the 1.7 release (June 2011), `Gary plotted the
following <http://spatialgalaxy.net/2011/09/23/history-of-qgis-committers/>`_
that shows the growth of committers working on the project:

.. figure:: /static/developer_workshop/21_qgis-devs-over-time.png
   :align: center
   :width: 40em

The graphic includes all committers including documentation writers,
translators, and developers. In addition, it doesn't cover the whole spectrum of
people who have contributed code and patches that were applied by project
developers. There are a lot more people involved in the care and feeding of QGIS
than the 46 represented on the graph.

In recent years, the project has used `OpenHub to track project information <https://www.openhub.net/p/qgis>`_,
using the `QGIS project github.com account <https://github.com/qgis/QGIS>`_.

In 2013, the project name of **Quantum GIS** was shortened to just **QGIS**.

`Tim Sutton keynote presentation <http://youtu.be/sQ8ytFJE_Wk>`_ at FOSS4G 2013 that
includes great anecdotes on early QGIS development (starting ~ 8:50).

`Cool video that graphically represents development from 2003 through 2011 <http://woostuff.wordpress.com/2011/01/03/generating-a-gource-source-commit-history-visualization-for-qgis-quantum-gis/>`_.

Older releases and their dates are listed on `Wikipedia <http://en.wikipedia.org/wiki/QGIS>`_.

Who contributes to the |qg| Project and community?
==================================================

.. figure:: /static/developer_workshop/20_qgis-devs-world.png
   :align: center
   :width: 40em

   Developers as of early 2014 (yes, map made in |qg|).

.. figure:: /static/developer_workshop/19_qgis-devs-europe.png
   :align: center
   :width: 40em

   Developers located in Europe as of early 2014.

`Listing of current contributors <https://github.com/qgis/QGIS/graphs/contributors>`_
on |qg| github.com account.

`Listing of sponsors and donors <http://qgis.org/en/site/about/sponsorship.html>`_
on the |qg| web site.

How is the |qg| Project structured?
===================================

See `QGIS Governance section of the documentation <https://www.qgis.org/en/site/getinvolved/governance/organisation/governance.html>`_.

Where is |qg| in the OSGeo/FOSS4G software stack?
=================================================

Desktop is at the top. Or, QGIS libs and/or PyQGIS is in the middle or at the
bottom!

How is development sponsored?
=============================

.. figure:: /static/developer_workshop/23_get-involved-donate.png
   :align: center
   :width: 40em

One way to support development is to contribute funds directly to the |qg|
Project. Often general funds from the Project are used to organize code sprints
(or hackfests in |qg| parlance). Such code sprints involve many contributors in
different areas of the project working together, e.g. documentation, releasing
and coding. In other words, when funds are contributed to the project, the
project decides how to use the contribution.

If your company or institution would prefer to focus your funding towards a
specific feature or coding goal, you can contact a developer directly and offer
to sponsor their work. If the work has the potential to be included in the core
|qg| codebase, it is advisable to discuss your intentions with the developer
community prior to doing the work, or submitting a pull request.

It is not uncommon to find out the feature you are requesting has been attempted
before. It is the nature of open source to not reinvent the wheel, i.e. build
upon others previous work. If the existing code is GPL-licensed, everyone wins,
and the feature is implemented quicker and probably with less cost.

|qg| Enhancement Proposal
-------------------------

As of September 2014, the project has a renewed interest in a more formal
process of garnering community feedback: the standard Request For Comment (RFC)
process used in many software project, e.g. MapServer and GDAL/OGR.

What can you as a dev do with |qg|?
===================================

.. InaSafe, ROAM, Plugins, core Dev

