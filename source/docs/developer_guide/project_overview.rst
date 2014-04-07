.. _project_overview:

****************
Project Overview
****************

Core Technologies
=================

QGIS is built upon many open source libraries and GUI toolkits, and provides
an extensive scriptable interface via bindings to the Python language. The core
technologies include:

* `CMake <http://cmake.org>`_ - Flexible abstracted build system to ensure
  source code can be built on many different platforms, using various compilers.

* `C++ <http://www.c.org>`_ - Programming language used for core source code,
  core and add-on plugins, and for custom stand-alone applications.

* `Qt Toolkit <http://qt-project.org>`_ - Allows abstraction of much of the core
  source GUI code, by providing platform-agnostic widgets and routines, to
  ensure the graphical interface is similar across platforms. Qt also offers
  convenient platform-agnostic routines for common C++ tasks, greatly reducing
  the need for platform-specific code or verbose routines based upon the
  standard C libraries.

* `Python <http://www.python.org>`_ - Programming language used for core
  and add-on plugins, and for custom stand-alone applications.

* `Sip and PyQt <http://www.riverbankcomputing.co.uk>`_ - Sip provides the
  means of binding (exposing) core QGIS C++ objects and interfaces to Python.
  PyQt are sip bindings to Qt's objects and interfaces for Python, allowing
  almost every aspect of Qt's toolkit to be used from within Python.

* Data Source Providers - For reading and/or writing the myriad of GIS data
  source formats QGIS has internal providers which leverage either open source
  libraries, e.g. `GDAL/OGR <http://gdal.org>`_, `PostgresSQL <http://postgres.org>`_
  / `PostGIS <http://example.com>`_ and `Spatialite <http://example.com>`_, or
  proprietary libraries, e.g. `Oracle DB Client <http://oracle.com>`_.
  Providers for obscure or new data storage formats...

