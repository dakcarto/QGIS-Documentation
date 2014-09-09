.. comment out this Section (by putting '|updatedisclaimer|' on top) if file is not uptodate with release

.. _dev_shop_standalone_py:

****************************************
Workshop - Standalone Python Development
****************************************

.. contents::
   :local:
   :backlinks: top

C++ standalone apps
-------------------

See https://github.com/qgis/QGIS-Code-Examples

.. warning::

   Examples in the ``QGIS-Code-Examples`` repository are likely to need some
   work to get to function with latest master.


Python standalone apps
----------------------

In PyCharm, open the following directory as a new project::

  ﻿/media/user/qgis-dev/workshop/src/Standalone/shape_viewer

PyCharm doesn't have native support for 'compiling' or opening Qt :file:`.rsc`
or :file:`.ui` files. However, you can easily add external tool helpers.

Go to :menuselection:`Tools->Settings...` and under
:menuselection:`IDE Settings->External Tool`. Create a new tool with the
:guilabel:`Group` set to ``PyQt`` for compiling :file:`.ui` files into
:file:`.py` files:

.. figure:: /static/developer_workshop/70_py-standalone-external-tool.png
   :align: center
   :width: 40em

The full line of :guilabel:`Parameters` is::

  $FileDirRelativeToProjectRoot$/$FileNameWithoutAllExtensions$.py $FileDirRelativeToProjectRoot$/$FileName$

Create another new tool with under the :guilabel:`Group` of ``PyQt`` for opening
:file:`.ui` file in ``Qt Designer``:

.. figure:: /static/developer_workshop/73_py-standalone-external-tool-open.png
   :align: center
   :width: 40em

You may notice the following line causes PyCharm's code inspector to alert you
that there is a missing module in an ``import`` statement:

.. figure:: /static/developer_workshop/72_py-standalone-ui-mod-missing.png
   :align: center
   :width: 40em

Using the new external tool ``Compile .ui``, select the :file:`shapeviewer_gui.ui` and run the tool:

.. figure:: /static/developer_workshop/71_py-standalone-external-tool-select.png
   :align: center
   :width: 40em

The associated :file:`shapeviewer_gui.py` should be generated, and the
inspection in the editor should turn **green**.

The standalone module is ready to run, but requires the custom ``QGISHOME``
environment variable to be set.

Choose :menuselection:`Run->Run...` and elect to edit the default configuration
named after the module name:

.. figure:: /static/developer_workshop/74_py-standalone-run-edit-config.png
   :align: center
   :width: 40em

In the :guilabel:`Edit configuration settings` dialog, choose to add to
:guilabel:`Environment variables`:

.. figure:: /static/developer_workshop/75_py-standalone-qgishome.png
   :align: center
   :width: 40em

When done, click Apply, then Run the configuration. You should be presented by
a file open dialog. Browse to the following file and choose it::

  ﻿/media/user/qgis-dev/workshop/data/alaska.shp

.. figure:: /static/developer_workshop/76_py-standalone-shapefile.png
   :align: center
   :width: 40em

Which, should run the viewer:

.. figure:: /static/developer_workshop/77_py-standalone-shapefile-run.png
   :align: center
   :width: 40em

.. note::

   You should see a lot of textual output from the running of the app. This is
   debug output from |qg|, which was built as
   ``CMAKE_BUILD_TYPE=RelWithDebInfo``.
