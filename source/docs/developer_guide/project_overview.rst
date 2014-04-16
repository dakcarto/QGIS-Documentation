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

Directory Structure
===================

::

    CMakeLists.txt         <- main CMake setup file
    CMakeLists.txt.user    <- Qt Creator project settings (if using Qt Creator)
    CTestConfig.cmake
    INSTALL, CODING, etc.  <- older dev docs
    COPYING                <- GPL license
    cmake/                 <- CMake find modules and scripts
    cmake_templates/       <- CMake files to be generated
    debian/                <- package scripts
    doc/                   <- older docs based on txt2tags
    i18n/                  <- Qt Linguist translation files
    images/                <- core image resources, including SVGs
      images.qrc           <- where to add GUI images to become binary resources
    mac/                   <- CMake build setup for Mac packaging
    ms-windows/            <- Packaging scripts, including installer from Linux
    postinstall/           <- CMake scripts to run after `make install`
    python/                <- .sip binding files, matching sections of 'src'
      analysis/
      console/
      core/
      gui/
      plugins/             <- core Python plugins
      pyplugin_installer/  <- Plugin Manager and Installer
      pyspatialite/        <- older internal module source
      qsci_apis/           <- built QScintilla2 API files for PyQGIS Console
      utils.py             <- PyQGIS utility module
    resources/             <- resources and utilities to be installed
    scripts/               <- various scripts
    src/                   <- base source code
      analysis/            <- analysis classes
      app/                 <- main Desktop GUI app classes
      astyle/              <- utility for C++ code style checking
      browser/             <- QGIS Browser source
      core/                <- core, base classes
      crssync/             <- tool to sync srs.db with system projection tools
      designer/            <- Qt Designer plugins
      gui/                 <- reusable Qt-subclassed GUI classes
      helpviewer/          <- obsolete help viewer?
      mapserver/           <- QGIS Server
      plugins/             <- core C++ plugins
      providers/           <- data source providers source
      python/              <- embedded Python interpreter classes
      ui/                  <- Qt Designer .ui files
    tests/                 <- CTest suite
      algorithms/          <- custom unit tests
      bench/               <- benchmarking utilities
      qt_modeltest/        <- tool from Qt for checking custom item view models
      src/                 <- C++ and Python unit tests, that match C++ 'src'
        analysis/
        app/
        core/
        gui/
        providers/
        python/            <- PyQGIS-based unit tests
      testdata/            <- data for tests, including control images
    tools/                 <- build config utility

