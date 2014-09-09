.. comment out this Section (by putting '|updatedisclaimer|' on top) if file is not uptodate with release

.. _dev_shop_core:

*******************************
Workshop - Core C++ Development
*******************************

Bit of trivia: *Why are QGIS C++ classes prefixed with Qgs?* `Gary Sherman explains`_.

.. _Gary Sherman explains: http://spatialgalaxy.net/2014/03/29/why-all-qgis-classes-start-with-qgs/

Tour of the API documentation
=============================

http://qgis.org/api/

Qt Creator's integrated help system
===================================

.. figure:: /static/developer_workshop/83_core-dev-qt-help.png
   :align: center
   :width: 50em

Committing to core codebase
===========================

See the following docs:

- :ref:`managing_source`

- :ref:`coding_standards`

- :ref:`contribute_code`

- :ref:`debugging_testing`

- :ref:`interface_guidelines`

Example of editing a GUI class
==============================

Locate and edit ``QgsMessageBar`` class
---------------------------------------

Add new bar status type:

.. figure:: /static/developer_workshop/84_core-dev-messbar-enum.png
   :align: center
   :width: 50em

Update applied stylesheet:

.. figure:: /static/developer_workshop/85_core-dev-messbar-item.png
   :align: center
   :width: 50em

Update associated ``sip`` file
------------------------------

.. figure:: /static/developer_workshop/87_core-dev-update-sip.png
   :align: center
   :width: 50em

Create external tool to clean syntax on local repository
--------------------------------------------------------

.. figure:: /static/developer_workshop/86_core-dev-prepare-commit.png
   :align: center
   :width: 50em

Path to executable::

    %{CurrentProject:Path}/scripts/prepare-commit.sh

Run the external tool on the repository:

.. figure:: /static/developer_workshop/88_core-dev-prepare-commit-run.png
   :align: center
   :width: 50em

Sample output:

.. figure:: /static/developer_workshop/89_core-dev-prepare-commit-run-out.png
   :align: center
   :width: 50em

Re-compile and test use of qgis.gui module
------------------------------------------

Recompile source tree using just ``make`` step; then, launch app from build
directory.

In PyQGIS console::

  from qgis.gui import *
  mb = iface.messageBar()
  mb.pushMessage('Hello', 'World', QgsMessageBar.OMG)

It stacked a new message, but why is it not pink?

How would you go about fixing that?

(Optional) Add message bar to Python standalone app
---------------------------------------------------

See: `Nathan Woodrow's post`_

.. _Nathan Woodrow's post:
   http://nathanw.net/2013/08/02/death-to-the-messagebox-using-the-qgis-messagebar/

Review changes in repository using a git client
-----------------------------------------------

.. figure:: /static/developer_workshop/90_core-dev-smartgit.png
   :align: center
   :width: 50em

   Using SmartGit client

To **revert** the changes you have made, choose to ``reset`` the repository to
the commit before your changes, using the ``hard`` option which removes the
changes and returns your working tree to a clean state.

You will then have to re-compile again to revert your |qg| build.
