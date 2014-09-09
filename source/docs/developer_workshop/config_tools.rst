.. comment out this Section (by putting '|updatedisclaimer|' on top) if file is not uptodate with release

.. _dev_shop_tools:

*****************************
Workshop - Tool Configuration
*****************************

.. contents::
   :local:
   :backlinks: top

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

On launching QtCreator go to the :menuselection:`File->Sessions Manager...`
menu and ensure :guilabel:`Restore last session on startup` is *checked*.

.. figure:: /static/developer_workshop/52_config-creator-session-manager.png
   :align: center
   :width: 40em

.. note::

   If Creator ever launches and forgets your open project, or what files you
   have open, go back to :guilabel:`Sessions Manager` and select your session
   and click :guilabel:`Switch to`

Go to the :menuselection:`Tools->Options...` menu.

Go to the :guilabel:`Editor->Display` tab and ensure
:guilabel:`Highlight current line` is checked.

.. figure:: /static/developer_workshop/43_config-creator-opts-editor-display.png
   :align: center
   :width: 40em

Under the :guilabel:`Editor->Generic Highlighter` tab click
:guilabel:`Download Definitions...`:

.. figure:: /static/developer_workshop/53_config-creator-opts-editor-highlight.png
   :align: center
   :width: 40em

Select the following languages that are used in the |qg| project source tree, or
in the workshop tutorials, then **click** :guilabel:`Download Selected Definitions`::

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

Go to the :guilabel:`Build and Run->General` tab and ensure
:guilabel:`Always build ...` and :guilabel:`Always deploy ...` are *unchecked*.

.. figure:: /static/developer_workshop/51_config-creator-opts-build-run-gen.png
   :align: center
   :width: 40em

.. note::

   Since the |qg| project is based on CMake, and both it and Qt Creator sense
   when your project needs to be rebuilt, it is unnecessary to always do so,
   especially when launching to debug.

Configuring QGIS Project
------------------------

Go to the :menuselection:`File->Open File or Project` menu. Then, use the
resulting file selection dialog to browse to and open::

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

Compiling |qg| source tree
--------------------------

Usually compiling |qg| source can take a *very long time*, especially if you
have less thatn 4 CPU cores to devote to the process; but, the
:file:`qgis-dev-workshop.vmdk` disk already has a pre-compiled build.

Open the :guilabel:`Compile` tab on the bottom of the main window, and expand
it. Then, click the compile icon:

.. figure:: /static/developer_workshop/49_config-creator-proj-compile.png
   :align: center

If successful, you should see something like this output:

.. figure:: /static/developer_workshop/59_config-creator-compile-output.png
   :align: center
   :width: 40em

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
source file and click in the left column. To test debugging works, open the following file::

  ﻿/media/user/qgis-dev/qgis-src/src/app/main.cpp

Then, set a breakpoint at line ``#747``, or whatever line is associated with the
following code line:

.. figure:: /static/developer_workshop/54_config-creator-debug-line.png
   :align: center
   :width: 50em

Now launch QGIS under the debugger by clicking the icon with a bug on it in the
bottom left of the window.

.. figure:: /static/developer_workshop/55_config-creator-debug-button.png
   :align: center

.. note::

   Alternatively, you can go to the Debug view section of the IDE and start the
   debugger from the toolbar:

   .. figure:: /static/developer_workshop/56_config-creator-debug-section.png
      :align: center

If successful, the debug view should look similar to this:

.. figure:: /static/developer_workshop/57_config-creator-debug-view.png
   :align: center
   :width: 50em

To see the debug output from the |qg| application itself, open the
:guilabel:`Application Output` from the bottom toolbar of the app:

.. figure:: /static/developer_workshop/58_config-creator-debug-view-app-output.png
   :align: center
   :width: 50em


Configuring PyCharm
===================

Define new project
------------------

On launching PyCharm, if it is your first time, you will need to create a new
project:

.. figure:: /static/developer_workshop/60_config-pycharm-new-proj.png
   :align: center
   :width: 50em

Choose to make a new project with the following specifications:

.. figure:: /static/developer_workshop/61_config-pycharm-new-proj-specs.png
   :align: center
   :width: 50em

.. warning::

   |qg| (as of version 2.6) only works with Python 2.5.x -> 2.7.x. Choosing
   Python 3.x at this stage will greatly confuse things during development.

.. note::

   You will probably have to edit the :guilabel:`Location` path, since PyCharm
   will try to add the Project name to the path::

     ﻿/media/user/qgis-dev/qgis-src/python/plugins

You will be prompted to add existing files, click :guilabel:`Yes`:

.. figure:: /static/developer_workshop/62_config-pycharm-new-proj-add-files.png
   :align: center
   :width: 50em

Adjust project's Python interpreter
-----------------------------------

.. note::

   This is for when you are working with **qgis** modules of development builds.

Go to :menuselection:`File->Settings...`, then under  **Project Settings**, go to
**Project Interpreter**. Choose the small gear to the right of the listed
interpreter and **click** :guilabel:`More...`:

.. figure:: /static/developer_workshop/64_config-pycharm-settings-proj-py-int-more.png
   :align: center
   :width: 50em

Normally, the installed :file:`qgis` modules, e.g. :file:`qgis.core`, in the
system Python :file:`site-packages` (or :file:`dist-packages`) is fine for the
auto-completion and code inspection routines of PyCharm to work, but we need to
use the newly built :file:`qgis` modules from our source build directory.

Add the following path to the interpreter. Also, if you have modules you would
like to load from the PyCharm project's root path, add that as well::

  ﻿/media/user/qgis-dev/qgis-src-build/output/python

.. figure:: /static/developer_workshop/65_config-pycharm-settings-py-int-paths.png
   :align: center
   :width: 50em

If you have the stable version of |qg| installed, which is the case when using
OSGeo-Live, PyCharm will probably find those :file:`qgis` modules first, before
our development modules. This will keep us from seeing new changes in the
modules since the last stable build.

Do the following to ensure the stable :file:`qgis` modules stay on a system
Python path, *but* do not interfere with the development versions::

   ﻿mkdir -p ~/.local/lib/python2.7/site-packages
   ﻿sudo mv /usr/lib/python2.7/dist-packages/qgis \
   ~/.local/lib/python2.7/site-packages/

.. note::

   Last line is a multi-line single command.
   The user/password for the default OSGeo-Live user is user/user.

To test new the setup use the following Python code snippet and run it from
various Python interactive consoles::

  from qgis.core import *
  QGis.QGIS_VERSION

Results from :menuselection:`Tools->Run Python Console...` in PyCharm:

.. figure:: /static/developer_workshop/67_config-pycharm-console-check.png
   :align: center
   :width: 50em

Results from PyQGIS Console inside of |qg| stable on OSGeo-Live:

.. figure:: /static/developer_workshop/68_config-pycharm-console-check-qgis.png
   :align: center
   :width: 50em

Results from Python Console inside of Terminal session on OSGeo-Live::

  ﻿user@osgeolive:~$ python
  Python 2.7.6 (default, Mar 22 2014, 22:59:38)
  [GCC 4.8.2] on linux2
  Type "help", "copyright", "credits" or "license" for more information.
  >>> from qgis.core import *
  >>> QGis.QGIS_VERSION
  '2.4.0-Chugiak'

.. note::

   This does not affect the :file:`QGIS-dev-build` and :file:`QGIS-dev-install`
   custom-built apps on the :file:`qgis-dev-workshop.vmdk` disk, which already
   have their Python paths set up during the build process.

.. highlight:: python

The :file:`qgis` modules are compiled Python modules; and, as such, PyCharm must
build temporary 'skeletons' of the exposed function calls in order to make
auto-completion function in the code editor windows. To test auto-completion,
open :file:`﻿plugins/fTools/fTools.py` from the Project browser panel and go to
line #52 or whatever line is associated with the following code::

  ﻿self.QgisVersion = unicode(QGis.QGIS_VERSION_INT)

Then, :kbd:`Ctrl`-click on the ``﻿QGIS_VERSION_INT`` text. This should open the
generated 'skeleton' for the :file:`qgis.core` module and scroll to the
``QGis.QGIS_VERSION_INT`` property:

.. figure:: /static/developer_workshop/69_config-pycharm-test-skeleton.png
   :align: center
   :width: 50em

Debugging in PyCharm
--------------------

.. warning::

   Apparently, remote debugging is now only available with the Professional
   version of PyCharm, not the Community Edition.

   TODO: Switch to *what* for cross-platform graphical debugging?

.. note::

   Originally published on `linfiniti.com blog
   <http://linfiniti.com/2012/09/remote-debugging-qgis-plugins-using-pycharm/>`_.

   TODO: Finish reformatting and integration with downloaded ``Hello World``
   plugin.

We are going to focus on the steps needed to remote debug Python scripts /
plugins running inside |qg|. We are going to start with the assumption that you
have your PyCharm set up and your plugin basics in place and now you are at the
point where you wish to debug your software while it is running in |qg|.

.. note::

   You should very seldom need to use this technique if your code is heavily
   tested (by means of a Python test suite); i.e., in most cases you can just
   debug a particular test directly without needing to remotely attach to a
   Python process in |qg|.

Download the ``Hello World`` plugin. This will end up in::

  ~/﻿.qgis2/python/plugins/HelloWorld

You need to have ``pydevd`` module to be in your ``PYTHONPATH`` in the context
of the running plugin. There are a number of other ways you could do this, for
example by changing your code to add the pydev directory into ``sys.path``.

To keep things simple we are going to copy PyCharm's ``pydev`` module to the
``HelloWorld`` directory::

  ﻿cp -R ﻿/home/user/pycharm-community-3.4.1/helpers/pydev \
  /home/user/.qgis2/python/plugins/HelloWorld/

Next, configure a Python debug server (provided as part of PyCharm). To do this,
choose edit configurations from the task list:

.. figure:: /static/developer_guide/configuring_tools/pycharm_01.jpg
   :align: center
   :width: 50em

Next click on the little '+' icon and choose Python remote debug:

.. figure:: /static/developer_guide/configuring_tools/pycharm_02.png
   :align: center
   :width: 50em

.. note::

   See also: http://www.jetbrains.com/pycharm/webhelp/remote-debugging.html

Set the following options::

    Local host name: localhost
    Port: 53100

Note that you can use any high port that you like (assuming it is unused).

You will notice that on the dialog it gives you some handy hints as to what
needs to be inserted into your code in order to enable the trace point:

.. figure:: /static/developer_guide/configuring_tools/pycharm_03.png
   :align: center
   :width: 50em

The next thing you need to do is add a couple of lines to the module that you
wish to debug (this is also described in the above dialog). First, in your
imports add this::

    from pydev import pydevd

And then **in the place where you wish execution to halt**, add this line::

    pydevd.settrace('localhost',
                    port=53100,
                    stdoutToServer=True,
                    stderrToServer=True)

You can also try using your IP address instead 'localhost'.

Ok now you are all set. One thing to remember is that the settrace line is just
the initial breakpoint - you can set additional breakpoints in your code using
normal PyCharm debugging techniques. Now launch your PyCharm debug server
configuration by clicking the little run icon next to it (highlighted in red
below):

.. figure:: /static/developer_guide/configuring_tools/pycharm_04.png
   :align: center
   :width: 40em

After this you will see some output like this in the PyCharm run panel::

  Starting debug server at port 53100
  Waiting for connection...

Next fire up your copy of |qg| and open the plugin that will trigger your
settrace. When the trace point is hit, PyCharm will enter debug mode and
highlight the trace line in blue like this:

.. figure:: /static/developer_guide/configuring_tools/pycharm_06.jpg
   :align: center
   :width: 50em

Now you can step through your code, inspect variables and generally have a
productive time understanding your code.
