.. much of the initial prose here was culled from Mapserver's INSTALL.CMAKE doc

.. _install_qgis_common:

**************************
General Build Instructions
**************************

Dependencies and Requirements
=============================

Required build tools:

- CMake >= 2.8.0
- Flex
- Bison >= 2.4

Required build deps:

- Qt >= 4.7.0
- Proj >= 4.4.x
- GEOS >= 3.0
- Sqlite3 >= 3.0.0
- GDAL/OGR >= 1.4.x
- Qwt >= 5.0 & (< 6.1 with internal QwtPolar)
- expat >= 1.95

Optional dependencies:

- for GRASS plugin - GRASS >= 6.0.0 (libraries compiled with exceptions support
  on Linux 32bit)
- for georeferencer - GSL >= 1.8
- for postgis support and SPIT plugin - PostgreSQL >= 8.0.x
- for gps plugin - gpsbabel
- for mapserver export and PyQGIS - Python >= 2.3 (2.5+ preferred)
- for python support - SIP >= 4.12, PyQt >= 4.8.3 must match Qt version,
  Qscintilla2
- for qgis mapserver - FastCGI

CMake Build System
==================

QGIS, like a number of major projects (eg. KDE 4.0), uses `CMake
<http://www.cmake.org>`_ for building from source. CMake is open source and free
of charge and is usually included in distribution packages, or can be downloaded
and compiled with no third party dependencies.

CMake itself does not do the actual compiling of the source code, it mainly
creates platform specific build files that can then be used by standard build
utilities (make on unixes, visual studio on windows, xcode on osx, etc...)

Install CMake
-------------

TODO

CMake Options
-------------

General CMake options useful for building QGIS on some platforms

.. note::

    See CMake's extensive docs for help with other general options:
    http://www.cmake.org/cmake/help/documentation.html

* CMAKE_INSTALL_PREFIX:PATH - Install path prefix, prepended onto install
  directories.

* CMAKE_PREFIX_PATH:UNINITIALIZED - Path(s) to non-standard install locations.
  *Important:* when used on command line, the delimiter is always a ';'
  (semicolon); whereas, when set as an environment variable, it is the PATH
  delimiter, e.g. ':' (colon) for Unix-based OSes.

* CMAKE_FIND_FRAMEWORK:UNINITIALIZED - How/when to handle looking for frameworks
  (useful for Mac).

* CMAKE_FRAMEWORK_PATH:UNINITIALIZED - Location of non-standard framework
  installs (useful for Mac).

QGIS-specific Options
.....................

These are some base CMake options that are specific to QGIS, beyond those for
finding dependencies, that control the build and install process.

* CXX_EXTRA_FLAGS:STRING - Appended CXXFLAGS, *after* QGIS's CMake setup has
  manipulated the flags. Useful for nixing some compiler warnings.

* ENABLE_TESTS:BOOL - Build unit tests?

* GRASS_PREFIX:PATH - Path to GRASS base directory.

* MAPSERVER_SKIP_ECW:BOOL - Determines whether QGIS mapserver should
  disable ECW (ECW in server apps requires a special license).

* PYTHON_CUSTOM_FRAMEWORK:PATH - Path to custom Python.framework on Mac
  (should not have to specify other Python options).

* QGIS_MACAPP_BUNDLE:STRING - What to bundle into app package.

* QGIS_MACAPP_BUNDLE_USER:STRING - Path to user bundling script.

* QGIS_MACAPP_DEV_PREFIX:STRING - Path to install developer frameworks.

* QGIS_MACAPP_INSTALL_DEV:BOOL - Install developer frameworks.

* QT_PLUGINS_DIR:PATH - The location of the Qt plugins.

* SITE:STRING - Name of the computer/site where compile is being run.

* WITH_APIDOC:BOOL - Determines whether the QGIS API doxygen documentation
  should be built.

* WITH_ASTYLE:BOOL - If you plan to contribute you should reindent with
  ``scripts/prepare-commit.sh`` (using 'our' astyle).

* WITH_BINDINGS:BOOL - Determines whether python bindings should be built.

* WITH_DESKTOP:BOOL - Determines whether QGIS desktop should be built.

* WITH_GLOBE:BOOL - Determines whether Globe plugin should be built.

* WITH_GRASS:BOOL - Determines whether GRASS plugin should be built.

* WITH_MAPSERVER:BOOL - Determines whether QGIS mapserver should be built.

* WITH_ORACLE:BOOL - Determines whether Oracle support should be built.

* WITH_POSTGRESQL:BOOL - Determines whether POSTGRESQL support should be built.

* WITH_PYSPATIALITE:BOOL - Determines whether PYSPATIALITE should be built.

* WITH_PY_COMPILE:BOOL - Determines whether Python modules in staged or
  installed locations are byte-compiled.

* WITH_QSCIAPI:BOOL - Whether to generate PyQGIS QScintilla2 API file. (For
  devs) run 'make qsci-pap-src' in between QGIS build and install to regenerate
  .pap file in source tree for console auto-completion.

* WITH_QSPATIALITE:BOOL - Determines whether QSPATIALITE sql driver should be
  built.

* WITH_QTMOBILITY:BOOL - Determines if QtMobility related code should be build
  (for example internal GPS).

* WITH_STAGED_PLUGINS:BOOL - Stage-install core Python plugins to run from build
  directory? (utilities and console are always staged).

* WITH_TOUCH:BOOL - Determines if touch interface related code should be build.

Configure and Generate Build Files
----------------------------------

Although you can configure and build in QGIS's source directory as created by
downloading a tarball or using a git clone, it is **highly** recommended to run
*out-of-source* builds, i.e. having all build files be generated and compiled in
a *different directory* than the actual sources. This allows to have different
configurations running alongside each other (e.g. release and debug builds,
cross-compiling, enabled features, etc...).

To configure on the command line using an out-of-source directory::

    $ mkdir build && cd build
    $ cmake [-D option=value, -D ...] path-to-source

The simplest form of this is to create the ``build`` directory inside the source
directory.

Example (one command, separated on multiple lines)::

    $ cmake -D CMAKE_INSTALL_PREFIX=/home/user/qgis-install \
    -D CMAKE_BUILD_TYPE=RelWithDebInfo \
    -D ENABLE_TESTS=TRUE \
    -D WITH_ASTYLE=TRUE \
    -D WITH_MAPSERVER=TRUE \
    -D WITH_STAGED_PLUGINS=FALSE \
    -D WITH_APIDOC=FALSE \
    -D WITH_QSCIAPI=FALSE \
    -D WITH_INTERNAL_QWTPOLAR=TRUE \
    -D WITH_GLOBE=FALSE \
    /home/user/qgis-src

The command is run from the build directory. The last part of that command is
the path to the QGIS source.

The ``cmake -D option ... path-to-source`` command will both configure the
project and, if successful, generate the default build files for your platform.
However, unless you know everything is ready to build, there are more than
likely dependency issues that need addressed. To view the configuration, open
the following file::

    $ cd build
    $ nano CMakeCache.txt

If you edit ``CMakeCache.txt``, you will need to re-generate the build files
again. You are better off changing your ``-D option ...`` string of options in a
script to preserve your choices, and to allow complete removal of the build
directory without losing any custom configuration.

Using ccmake and cmake-gui
..........................

Alternatively, there are graphical interfaces for working with CMake options and
generating build files. These may be included with your install of CMake.

* ccmake - console curses interface: http://www.cmake.org/Wiki/CCMake_2.8.12_Docs

* cmake-gui - Qt interface when CMake is compiled with the ``--qt-gui`` option.

