.. comment out this Section (by putting '|updatedisclaimer|' on top) if file is not uptodate with release

.. _dev_shop_intro_shop:

*****************************
Workshop - Overview and Setup
*****************************

Workshop concept is on how to become a |qg| developer. The goal is to get
potential developers excited about and involved with the QGIS project, and to
explore the benefits of becoming a developer or, as a business, sponsoring
one.  

The focus will be on the many avenues one can gain working knowledge and apply
their ideas to help the project and themselves or their business, whether it be
with writing plugins, standalone apps, documentation or tutorials, to submitting
code pull requests or becoming a core project developer.  

The workshop covers taking the first steps from user or existing developer to
becoming a |qg| developer, with insights on the project’s organization and
maintainers. It includes an overview of the technologies used and recommends
tools and workflows for crafting and managing |qg|’s C++ and Python source
code.  

What attendees should leave having and knowing
==============================================

- Setting up a build environment for |qg| and its main dependencies, e.g. GDAL,
  on developer’s main platform and others.

- Accessing and managing the different |qg| source code repositories using git
  and github.com.

- Configuring Qt Creator to build and debug |qg| source.

- How |qg| source code is organized and overview of the build process and its
  many options.

- Using the Qt toolkit for GUI and multilingual development.

- |qg| Python bindings and how to use them for core and external development,
  and setting up PyCharm to work with built components.

- Debugging and writing unit tests.

- Developer guidelines, submitting maintainable code, and deploying plugins.

When finished, you should be ready to embark on a code-writing project and know
how/where to get help. It is not specifically about how to write code, but how
to gain the knowledge, set up the cross-platform development environment, then
deploy and maintain the code, plugin or app.

Why a |vb| and virtual machine (VM) setup?
==========================================

This workshop is based off of the `OSGeo-Live Virtual Machine hard disk <http://live.osgeo.org/>`_.
The OSGeo-Live project is a mature sub-project of the OSGeo Foundation and
consists of numerous hard-working volunteers who have made an excellent demo
tool for the OSGeo and FOSS4G tool stack.

See also: `Slide presentation of what's on OSGeo-Live <http://live.osgeo.org/en/presentation/index.html>`_.
Make sure to type 's' to see presentation notes.

OSGeoLive 8.0+ is based off of Lubuntu_.

.. _Lubuntu: http://lubuntu.net/

**OK that's great, but why use a VM setup?**

- Everyone will have exactly the same development environment and OS for use in
  the workshop.

- While the ultimate intent of the workshop is to set up an environment to get
  you rolling on |qg| development, managing all of the potential problems of
  installing dependencies and getting |qg| to compile could easily eat up a
  whole day of a workshop.

- The |vb| setup is a bit redundant for some attendees; but, when the workshop
  is done, they will still have a working setup (for reference) and can later
  delete it and uninstall |vb| to bring your system back to a clean state.

- It is better to inconvenience workshop attendees with a |vb| setup than be
  responsible for messing up their system (if things go wrong).

- A good practice of being a |qg| developer is gaining the knowledge to set up
  and manage a source build environment, often for multiple platforms.

- Working in virtual machines has many advantages: portability, snapshots, and
  automated build tools, like `Vagrant <https://www.vagrantup.com/>`_.

- |qg| development *should be cross-platform* (even for Python plugin
  developers). Having VM setups for the major platforms supported by |qg| will
  help with testing your work and helping users with issues.

Install of |qg| development environment
=======================================

Install |vb| on host computer
-----------------------------

.. figure:: /static/developer_workshop/14_virtualbox.png
   :align: center
   :width: 40em

- `VirtualBox main site <https://www.virtualbox.org/>`_

- `VirtualBox 4.3.14 issues on Windows <https://forums.virtualbox.org/viewtopic.php?f=6&t=62615>`_

- `VirtualBox for Linux install instructions <https://www.virtualbox.org/wiki/Linux_Downloads>`_

Create virtual machine
----------------------

See also: `OSGeo-Live Quickstart for Running in a Virtual Machine <http://live.osgeo.org/en/quickstart/virtualization_quickstart.html>`_

#. Create virtual machine with the following parameters

   .. figure:: /static/developer_workshop/07_initial-vm.png
      :align: center
      :width: 40em

- (Recommended) Leave the virtual machine **empty**, without a virtual hard
  drive:

  .. figure:: /static/developer_workshop/09_initial-vm-nodrive.png
     :align: center
     :width: 40em

  .. note::

     An empty machine will allow you to place the existing virtual hard drives
     into the new virtual machine directory, allowing for a more portable
     virtual machine.

- Define the rest of the machine parameters as you see fit, relative to your
  hardware specs.

- Copy :file:`osgeo-live-8.0.vmdk` and :file:`qgis-dev_workshop.vmdk` into
  virtual machine directory.

  .. figure:: /static/developer_workshop/15_initial-vm-dirdrives.png
     :align: center
     :width: 40em

- Attach :file:`osgeo-live-8.0.vmdk` to virtual machine

  .. figure:: /static/developer_workshop/10_initial-vm-adddrive.png
     :align: center
     :width: 40em

     Add drive to main Controller.

  .. figure:: /static/developer_workshop/11_initial-vm-adddrive-existing.png
     :align: center
     :width: 40em

     Browse to :file:`osgeo-live-8.0.vmdk`.

  .. figure:: /static/developer_workshop/12_initial-vm-maindrive-ssd.png
     :align: center
     :width: 40em

     (Optional) If you are running the virtual machine from a Solid-state
     Drive, chose to pass that info on to the guest.

- Launch virtual machine to test OSGeo-Live boots properly

  .. figure:: /static/developer_workshop/16_initial-vm-test-vm.png
     :align: center
     :width: 40em

- Install virtual machine Guest Additions to OSGeo-Live

  .. note::

     If you have not already done so, download the Extensions Pack for the same
     version of |vb| that you are using. Then, open |vb| Preferences add it
     under Extensions.

  .. figure:: /static/developer_workshop/17_initial-vm-extensions.png
     :align: center
     :width: 30em

  While the OSGeo-Live VM is running, choose the menu item
  :menuselection:`Device-->Insert Guest Additions CD image...`. This will mount
  the image in the OSGeo-Live guest.

  From within the guest, open a Terminal and issue the following to install the
  Additions (actual version may differ)::

    ﻿cd /media/user/VBOXADDITIONS_4.3.14_95030/
    ﻿sudo ./VBoxLinuxAdditions.run

  .. note:: The user/password for the default OSGeo-Live user is user/user.

  Once installed, **reboot** the guest. Test the additions were installed
  properly by turning on clipboard sharing and attempting to share text between
  host and guest, or vice versa.

  .. warning::

     Ensure your OSGeo-Live guest OS can connect to the internet. Otherwise,
     further package installs to the guest will likely fail.

- Create shared folder between your host and guest

  .. figure:: /static/developer_workshop/24_initial-vm-shared-folder.png
     :align: center
     :width: 40em

     Ensure Auto-mount is checked.

  Assuming your share (Folder Name in GUI) is ``my-share`` then |vb| will
  auto-mount the host folder in the guest OS here (note the ``sf_`` prefix)::

    /media/sf_my-share

  To ensure write access by the guest's user, add the OSGeo-Live default user to
  the appropriate group. Then, create a symbolic link to the share in a more
  useable place::

    sudo adduser user vboxsf
    ln -s /media/sf_my-share ~/my-share

  .. note:: The user/password for the default OSGeo-Live user is user/user.

- Attach :file:`qgis-dev_workshop.vmdk` secondary virtual hard drive

  .. figure:: /static/developer_workshop/01_virtualbox_add-disk.png
     :align: center
     :width: 40em

     Attach virtual drive to main Controller, same as :file:`osgeo-live-8.0.vmdk`.

  .. figure:: /static/developer_workshop/02_virtualbox_disk-writethrough.png
     :align: center
     :width: 40em

     Ensure attached drive is of *Writethough* type.

  .. note::

     The *Writethough* type allows the :file:`qgis-dev_workshop.vmdk` drive
     image to remain independent of the main virtual drive, i.e. it is not
     included in the virtual machine's snapshots, and keeps its state when the
     virtual machine is rolled back to a previous snapshot.

.. highlight:: sh

- (Optional) If you do not do this step, you will always have to mount the
  :file:`qgis-dev` volume at the beginning of every login. Just clicking on it
  in the sidebar should be enough to mount it.

  Set :file:`qgis-dev_workshop.vmdk` to auto-mount on guest launch. In a
  Terminal session inside of the guest, issue the follow commands::

    # find the drive's UUID, which will be the one with: Label="qgis-dev"
    sudo blkid

    sudo nano /etc/fstab

    # add the following line to auto-mount the drive to /media/user/qgis-dev
    # NOTE: single line command may be wrapped below
    UUID=618d6d15-066c-43fb-bf66-bc9bf81103f0 /media/user/qgis-dev ext4 defaults 0 0

  .. note:: The user/password for the default OSGeo-Live user is user/user.

  **Reboot** the guest virtual machine.

Review of :file:`qgis-dev` and test QGIS-dev-build and -install
---------------------------------------------------------------

From within the OSGeo-Live guest, open a filesystem browser window and click on
the :file:`qgis-dev` drive in the sidebar:

.. figure:: /static/developer_workshop/18_test-qgis-dev-build-install.png
   :align: center
   :width: 40em

**Contents of :file:`qgis-dev`:**

- :file:`﻿installers` contains scripts and archives to install packages to
  OSGeo-Live for building QGIS source tree.

- :file:`lost+found` is a Linux-specific volume directory.

- :file:`qgis-install` is where QGIS will be installed. For development
  purposes, we install to a custom prefix, until the application is ready to
  install to the system.

- :file:`qgis-src` is a ``git`` clone of the QGIS source tree available from:

  https://github.com/qgis/QGIS

- :file:`qgis-src-build` is the out-of-source tree build directory that contains
  temporary files for a regular system install of QGIS.

- :file:`workshop` contains workshop tutorial files and example source code, and
  possibly a copy of the latest QGIS documentation.

The drive contains two desktop shortcuts to pre-compiled applications of a
recent version of the QGIS source tree:

- ``﻿QGIS-dev-build`` (:file:`qgis-dev-build.desktop`) is a shortcut to the
  application in the build directory, after compiling, but before installing,
  i.e. :file:`qgis-src-build`.

- ``﻿QGIS-dev-install`` (:file:`qgis-dev-install.desktop`) is a shortcut to
  the application in the directory in which QGIS is locally installed, i.e.
  :file:`qgis-install`.

Install development dependencies and tools
------------------------------------------

From a Terminal session within OSGeo-Live guest OS, issue the following
commands::

  cd /media/user/qgis-dev/installers

  sudo ./1_install-qgis-dev.sh
  ./2_install-qtcreator.sh
  ./3_install-pycharm.sh

  # optional install
  sudo ./4_opt-install-smartgit.sh

**Description of installer scripts:**

- :file:`1_install-qgis-dev.sh` (requires ``sudo``) installs the dependencies
  for building QGIS (but not QGIS itself). Also installs some extra utilities:
  ``Qt development tools``, ``qgit`` lightweight ``git`` GUI, ``geany`` basic code
  editor, ``gdb`` debugger and ``ccache`` compiler cacher to speed up subsequent
  builds of QGIS after a clean build of the source tree.

  .. note::

     When ``ccache`` is installed, two symbolic links are generated::

       /usr/local/bin/g++ -> /usr/bin/ccache
       /usr/local/bin/gcc -> /usr/bin/ccache

     If you wish to remove or disable ccache in the future, remove the above
     symbolic links.

- :file:`2_install-qtcreator.sh` installs the free and open source C++ code and
  GUI editor created by the `Qt Project <http://qt-project.org/>`_. It is built
  upon Qt and includes many integrations that make it very useful for building
  Qt-based applications, e.g. |qg|.

- :file:`3_install-pycharm.sh` installs the free and open source Community
  version of the powerful `PyCharm <http://www.jetbrains.com/pycharm/>`_ Python
  editor built upon Java. It has features that make it an excellent choice for
  PyQGIS (Python scripting of QGIS via Python bindings).

- (Optional) :file:`4_opt-install-smartgit.sh` (requires ``sudo``) installs a
  free ``git`` GUI built upon Java. It is free for open source projects, but
  is *not open source* itself. It runs equally well in Mac, Linux and Windows,
  which makes it an good choice if you intend to do multi-platform QGIS
  development (recommended) and wish to use a graphical ``git`` tool, instead of
  the command line, for working with QGIS's source tree and editing commits.

  If you are uncomfortable using proprietary software to work on open source
  projects, the ``qgit`` GUI client for ``git`` is also installed.

You should now be ready to configure your editing tools and delve into the
tutorials and code examples included in the workshop.
