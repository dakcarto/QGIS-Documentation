.. comment out this Section (by putting '|updatedisclaimer|' on top) if file is not uptodate with release

.. _dev_shop_tools:

*****************************
Workshop - Tool Configuration
*****************************

Where are the tools located?
============================

Launch development tools from the :menuselection:`Programming`
sub-menu of the **Lubuntu OS** launcher.

.. figure:: /static/developer_workshop/31_config-programming-menu.png
   :align: center
   :width: 40em

Configuring Qt Creator
======================

Qt Creator is an `open source`_, newish IDE from the makers of the
`Qt library`_. With Qt Creator you can build any C++ project, but it's really
optimised for people working on Qt-based applications (including mobile apps).

.. _open source: https://qt.gitorious.org/qt-creator
.. _Qt library: http://qt-project.org

It has the following features:

- An advanced C++ code editor

- Project and build management tools

- Integrated, context-sensitive help system

- Visual debugger

- Code management and navigation tools

Overview of Qt Creator's Options
--------------------------------

TODO

Highlighter definitions to install
..................................

QMake
INI Files
JSON
Makefile
Python
Doxygen
CMake
SQL (PostgreSQL)
Bash
Diff
reStructureText
HTML
C++/Qt4

Configuring QGIS Project
------------------------

On launching QtCreator do:

:menuselection:`File->Open File or Project`

Then use the resulting file selection dialog to browse to and open this file::

  /media/usr/qgis-dev/qgis-src/CMakeLists.txt

.. figure:: /static/developer_workshop/32_config-creator-open-cmakelists.png
   :align: center
   :width: 40em

.. note::

   You may be presented with the following dialog:

   .. figure:: /static/developer_workshop/33_config-creator-no-user-file.png
      :align: center
      :width: 40em

      Click ``Yes``.

The :guilabel:`CMake Wizard` start and you will be prompted for a build
location. This *should* default to the correct path at::

  /media/usr/qgis-dev/qgis-src-build

.. figure:: /static/developer_workshop/34_config-creator-build-location.png
   :align: center
   :width: 40em

   NOTE: This is a separate build directory, outside of the |qg| source tree
   directory.

The next dialog will require a CMake build parameter string, which will be used
to set the :file:`qgis-src-build/CMakeCache.txt` contents and generate the build
files specific for the platform. Paste a CMake option string. Note, this is just
like the command line build options, without the command's ``cmake`` binary path
prefix::

   # as single line
   -D CMAKE_INSTALL_PREFIX=/media/user/qgis-dev/qgis-install -D CMAKE_BUILD_TYPE=RelWithDebInfo -D ENABLE_TESTS=TRUE -D WITH_ASTYLE=TRUE -D WITH_MAPSERVER=TRUE -D WITH_STAGED_PLUGINS=FALSE -D WITH_APIDOC=FALSE -D WITH_QSCIAPI=FALSE -D WITH_INTERNAL_QWTPOLAR=TRUE -D WITH_GLOBE=FALSE /media/user/qgis-dev/qgis-src

    # as multi-line command (incompatible with Qt Creator)
    -D CMAKE_INSTALL_PREFIX=/media/user/qgis-dev/qgis-install \
    -D CMAKE_BUILD_TYPE=RelWithDebInfo \
    -D ENABLE_TESTS=TRUE \
    -D WITH_ASTYLE=TRUE \
    -D WITH_MAPSERVER=TRUE \
    -D WITH_STAGED_PLUGINS=FALSE \
    -D WITH_APIDOC=FALSE \
    -D WITH_QSCIAPI=FALSE \
    -D WITH_INTERNAL_QWTPOLAR=TRUE \
    -D WITH_GLOBE=FALSE \
    /media/user/qgis-dev/qgis-src

.. warning::

   This single-line string is separated into multiple lines for clarity here.
   Do not paste it as is or Qt Creator *will* choke on it.

.. note::

   See: :ref:`install_qgis_common` for info on working with CMake options.
   For this workshop, and for OSGeo-Live QGIS install, the Globe plugin is
   **not enabled**.

The :guilabel:`Generator` should be set to **Unix**.

Click :guilabel:`Run CMake`, which will start the project's configuration and
build file generation:

.. figure:: /static/developer_workshop/36_config-creator-cmake-opts-run.png
   :align: center
   :width: 40em

.. warning::

   If this step fails, you must fix the issue or the project will not open until
   you do so.

If previous steps were successful, QtCreator will load your |qg| project and
start scanning the source tree for auto-completion support and do some other
housekeeping stuff in the background.

Click the :guilabel:`Project` section of the IDE:

.. figure:: /static/developer_workshop/45_config-creator-build-run.png
   :align: center
   :width: 40em

   Ensure you are under the :guilabel:`Build and Run` tab.

.. note::

   If you need to rerun the CMake options generation again at a later date,
   click the :guilabel:`Run CMake..` button next to
   :guilabel:`Reconfigure project:`.

Set project **Build Steps**:

- Add a ``make`` step with a number of jobs argument relative to the number
  of available CPU cores you have to compile with:

  .. figure:: /static/developer_workshop/38_config-creator-build-step-make.png
     :align: center
     :width: 40em

     Note: this should probably be the full number of CPU cores you defined for
     your OSGeo-Live VM.

- Add a ``make`` step to stage the core Python plugins for use when running
  from the build directory (also byte-compiles the Python modules):

  .. figure:: /static/developer_workshop/40_config-creator-build-step-staged.png
     :align: center
     :width: 40em

     Note: you can also choose ``staged-plugins`` (no ``-pyc`` suffix) if you do
     not want the byte-compiling to take place.

- Add a ``make`` step to install the build:

  .. figure:: /static/developer_workshop/41_config-creator-build-step-install.png
     :align: center
     :width: 40em

Switch to the :guilabel:`Run` tab:

.. figure:: /static/developer_workshop/45_config-creator-build-run.png
   :align: center
   :width: 40em

Set :guilabel:`Run` configuration to ``qgis``:

.. figure:: /static/developer_workshop/44_config-creator-run-step.png
   :align: center
   :width: 40em

Set :guilabel:`Run Environment` so that the non-standard build prefix libraries
can be found by the OS linker::

  ﻿/media/user/qgis-dev/qgis-src-build/output/lib

.. figure:: /static/developer_workshop/46_config-creator-build-run-env.png
   :align: center
   :width: 40em

   Also ensure that :guilabel:`Enable C++` debugger is checked.

Switch to the :guilabel:`Editor` tab:

.. figure:: /static/developer_workshop/47_config-creator-proj-editor.png
   :align: center
   :width: 40em

   Ensure the :guilabel:`Tab` and :guilabel:`Indent size` is **2** and that
   :guilabel:`Tabs policy` is **Spaces Only**.

Switch to the :guilabel:`Style` tab and :guilabel:`Import..` a pre-configured
style from::

  ﻿/media/user/qgis-dev/workshop/QtCreator/QGIS_code-style.xml

.. figure:: /static/developer_workshop/48_config-creator-proj-code-style.png
   :align: center
   :width: 40em

Building QGIS
-------------

Open the :guilabel:`Compile` tab on the bottom of the main window, and expand
it. Then, click the compile icon:

.. figure:: /static/developer_workshop/49_config-creator-proj-compile.png
   :align: center

If QGIS compiles and installs OK, then for this setup you can turn off (don't
delete) the ``make install`` step under the project's **Build Steps**, since we
only need to compile and run/debug from build directory to test our source
edits. You can also disable the ``make staged-plugins[-pyc]`` step, unless you
are pulling in changes from |qg| upstream repository and those changes include
the core plugins.

.. figure:: /static/developer_workshop/50_config-creator-proj-disable-build-steps.png
   :align: center
   :width: 40em

   This little step will save you from wasting time installing unnecessary stuff during build testing.

Once you have a functioning build setup, consider following the basic C++ Editing
tutorial.

Running and Debugging
---------------------

Now you are ready to run and debug QGIS. To set a break point, simply open a
source file and click in the left column.

.. figure:: /static/developer_guide/configuring_tools/qtcreator_14.jpeg
   :align: center
   :width: 50em

Now launch QGIS under the debugger by clicking the icon with a bug on it in the
bottom left of the window.

.. figure:: /static/developer_guide/configuring_tools/qtcreator_13.jpeg
   :align: center


Configuring PyCharm
===================


