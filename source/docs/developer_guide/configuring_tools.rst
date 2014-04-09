.. _tools_config:

*********************
Configuring Dev Tools
*********************

.. _config_qtcreator:

Qt Creator
==========

Qt Creator is a newish IDE from the makers of the Qt library. With Qt Creator
you can build any C++ project, but it's really optimised for people working on
Qt-based applications (including mobile apps).

Installing Qt Creator
---------------------

For most platforms you can  Qt Creator . You can just download a pre-built
binary directly from the Qt Project:

http://qt-project.org/downloads

Also on Linux (stable repository version may be a little old)::

  sudo apt-get install qtcreator qtcreator-doc

After installing on Linux you should find it in your one of your main menus.

Setting up Project
------------------

This assumes you already have a local QGIS source git repository clone, and have
installed all needed build dependencies, etc. There are detailed in instructions
on doing in :ref:`managing_source` and :ref:`install_qgis`.

On my system I have checked out the code into $HOME/dev/cpp/QGIS and the
rest of the article is written assuming that, you should update these paths as
appropriate for your local system.

On launching QtCreator do:

:menuselection:`File->Open File or Project`

Then use the resulting file selection dialog to browse to and open this file::

  $HOME/dev/cpp/QGIS/CMakeLists.txt

.. figure:: /static/developer_guide/configuring_tools/qtcreator_01.jpeg
    :align: center
    :width: 50em

Next you will be prompted for a build location. I create a specific build dir
for QtCreator to work in under::

  $HOME/dev/cpp/QGIS/build-master-qtcreator

Its probably a good idea to create separate build directories for different
branches if you can afford the disk space.

.. figure:: /static/developer_guide/configuring_tools/qtcreator_02.jpeg
    :align: center
    :width: 50em

Next you will be asked if you have any CMake build options to pass to CMake. We
will tell CMake that we want a debug build by adding this option::

  -DCMAKE_BUILD_TYPE=Debug

.. figure:: /static/developer_guide/configuring_tools/qtcreator_03.jpeg
    :align: center
    :width: 50em

Thats the basics of it. When you complete the Wizard, QtCreator will start
scanning the source tree for autocompletion support and do some other
housekeeping stuff in the background. We want to tweak a few things before we
start to build though.

Setting up Build Environment
----------------------------

Click on the 'Projects' icon on the left of the QtCreator window.

.. figure:: /static/developer_guide/configuring_tools/qtcreator_04.jpeg
    :align: center

Select the build settings tab (normally active by default).

.. figure:: /static/developer_guide/configuring_tools/qtcreator_05.jpeg
    :align: center
    :width: 50em

We now want to add a custom process step. Why? Because QGIS can currently only
run from an install directory, not its build directory, so we need to ensure
that it is installed whenever we build it.  Under 'Build Steps', click on the
'Add Build  Step' combo button and choose 'Custom Process Step'.

.. figure:: /static/developer_guide/configuring_tools/qtcreator_06.jpeg
    :align: center

Now we set the following details::

  Enable custom process step [yes]
  Command: make
  Working directory: $HOME/dev/cpp/QGIS/build-master-qtcreator
  Command arguments: install

.. figure:: /static/developer_guide/configuring_tools/qtcreator_07.jpeg
    :align: center
    :width: 50em

You are almost ready to build. Just one note: QtCreator will need write
permissions on the install prefix.  By default (which I am using here) QGIS is
going to get installed to /usr/local. For my purposes on my development
machine, I just gave myself write permissions to the /usr/local directory.

To start the build, click that big hammer icon on the bottom left of the
window.

.. figure:: /static/developer_guide/configuring_tools/qtcreator_08.jpeg
    :align: center

Setting up Run Environment
--------------------------

As mentioned above, we cannot run QGIS from directly in the build directly, so
we need to create a custom run target to tell QtCreator to run QGIS from the
install dir (in my case /usr/local/). To do that, return to the projects
configuration screen.

.. figure:: /static/developer_guide/configuring_tools/qtcreator_04.jpeg
    :align: center

Now select the 'Run Settings' tab

.. figure:: /static/developer_guide/configuring_tools/qtcreator_09.jpeg
    :align: center
    :width: 50em

We need to update the default run settings from using the 'qgis' run
configuration to using a custom one.

.. figure:: /static/developer_guide/configuring_tools/qtcreator_10.jpeg
    :align: center
    :width: 50em

Do do that, click the 'Add v' combo button next to the Run configuration
combo and choose 'Custom Executable' from the top of the list.

.. figure:: /static/developer_guide/configuring_tools/qtcreator_11.jpeg
    :align: center

Now in the properties area set the following details::

  Executable: /usr/local/bin/qgis
  Arguments :
  Working directory: $HOME
  Run in terminal: [no]
  Debugger: C++ [yes]
            Qml [no]

Then click the 'Rename' button and give your custom executable a meaning full
name e.g. 'Installed QGIS'

.. figure:: /static/developer_guide/configuring_tools/qtcreator_12.jpeg
    :align: center
    :width: 50em

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

.. _config_pycharm:

PyCharm
=======

Why use PyCharm? Well it happens to be a really nice platform for QGIS python /
python plugin development, allows for good auto-completion and will help keep
your code PEP8-compliant. Also, it now offers open source code and a pre-built
'Community' version that is free for open source development:

http://www.jetbrains.com/pycharm/

Debugging in PyCharm
--------------------

.. note::

    Originally published on `linfiniti.com blog
    <http://linfiniti.com/2012/09/remote-debugging-qgis-plugins-using-pycharm/>`_.

We are going to focus on the steps needed to remote debug Python scripts /
plugins running inside QGIS. We are going to start with the assumption that you
have your PyCharm set up and your plugin basics in place and now you are at the
point where you wish to debug your software while it is running in QGIS.

.. note::

    You should very seldom need to use this technique if your code is heavily
    tested (by means of a Python test suite); i.e., in most cases you can just
    debug a particular test directly without needing to remotely attach to a
    Python process in QGIS.

The first thing you need to do is set up a Python debug server (provided as part
of PyCharm). To do this,  choose edit configurations from the task list:

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

And then in the place where you wish execution to halt, add this line::

    pydevd.settrace('192.168.1.62',
                    port=53100,
                    stdoutToServer=True,
                    stderrToServer=True)

You can also try using 'localhost' instead of your IP address.

The last thing you need to have in place before you can test is ``pydevd`` needs
to be in your ``PYTHONPATH`` in the context of the running plugin. In my case I
simply extracted the ``pydevd`` egg supplied in the root of the PyCharm
installation into my plugin directory::

    $ cp ~/apps/pycharm-2.5.2/pycharm-debug.egg .
    $ unzip pycharm-debug.egg

There are a number of other ways you could do this, for example by changing your
code to add the pydev directory into ``sys.path``.

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

Next fire up your copy of QGIS and open the plugin that will trigger your
settrace. When the trace point is hit, PyCharm will enter debug mode and
highlight the trace line in blue like this:

.. figure:: /static/developer_guide/configuring_tools/pycharm_06.jpg
    :align: center
    :width: 50em

Now you can step through your code, inspect variables and generally have a
productive time understanding your code.
