.. _virtual_machines:

*********************
Virtual Test Machines
*********************

Since QGIS runs on multiple platforms, it is important, as a developer, to be
able to test your contributions on as many as possible. Virtual machines allow
you to do this, though not with all of the major platforms. Mac dev environment
is the most robust, but only because it is restricted. Mac OS X (10.6+ Server
and 10.7+ Client) can only be  legally virtualized on Mac hardware; whereas,
Linux and Windows can be  virtualized on any CPU that supports them.

.. _osgeolive_vm:

OSGeo-Live QGIS-dev Virtual Machine
===================================

The purpose of this virtual machine (VM) configuration is to become familiar
with the base techniques of setting up a dev build environment for QGIS. It
leverages the `OSGeo-Live VM`_, which has many OSGeo programs and tools already
installed.

Since QGIS is built upon many of these tools, the OSGeo-Live VM is an ideal
starting point to learn or just try out QGIS development.

.. _OSGeo-Live VM: http://live.osgeo.org

.. note::

    This test machine is based off of OSGeo-Live 7.9,
    running under `VirtualBox 4.3.10 <https://www.virtualbox.org>`_.

Install VirtualBox and OSGeo-Live
---------------------------------

Follow the instructions on the `OSGeo-Live wiki`_ about installing VirtualBox
and loading the OSGeo-Live VM.

.. _OSGeo-Live wiki: http://live.osgeo.org/en/quickstart/virtualization_quickstart.html

In addition to the last command about Shared Folder mounting::

    user@osgeolive:~$ sudo mount -t vboxsf -o uid=user,rw GIS /home/user/GIS

you may wish to add a line to :file:`/etc/rc.local` to have the Shared Folder
automount on startup::

    $ sudo nano /etc/rc.local
      [sudo] password for user:  <-- input: user
    # add to end of /etc/rc.local, before 'exit 0' add the following

    mount -t vboxsf -o uid=user,rw GIS /home/user/GIS

    # WrtieOut changes and Exit nano

Also, there is an issue with the **Thunar** filesystem browser in OSGeo-Live 7.9
hanging on launches do to network parsing timeouts; you may wish to fix with.
Open Applications->Accessories->Terminal Emulator and issue::

    $ cd /etc/samba
    $ sudo cp smb.conf smb.conf.backup
      [sudo] password for user:  <-- input: user
    $ sudo nano smb.conf
    # change:

    ;   name resolve order = lmhosts host wins bcast

    # to

       name resolve order = wins bcast

    # WrtieOut changes and Exit nano

.. note::

   See: https://bugs.launchpad.net/ubuntu/+source/thunar/+bug/775117/comments/54

Install QGIS-dev ISO contents
-----------------------------

Instead of having yet another VM, the ``qgis-dev`` setup is installed onto the
basic OSGeo-Live VM. This allows adding it to any existing VM a user may already
be running.

Download the `qgis-dev_osgeolive-79.iso <>`_ and attach the .iso file to the
VM's CD/DVD drive, while the VM is *powered down*.

.. figure:: /static/developer_guide/virtual_machines/qgis-dev_attach-iso.png
    :align: center
    :width: 50em


Upon startup and login, the .iso should automount on the Desktop as a
``qgis-dev``-named drive. Open the drive with the filesystem browser to make
sure it is mounted.

Once mounted, the drive is connected to the :file:`/media/qgis-dev` mount point.

Go to :menuselection:`Applications --> Accessories --> Terminal Emulator` and
issue the following::

    $ cd /media/qgis-dev
    $ sudo ./1_install-qgis-dev.sh

    # interactive install
    $ ./2_install-qtcreator.sh

    # interactive install
    $ ./3_install-pycharm.sh

This will install all of the dependencies for building QGIS, Qt development
tools (including Qt Creator) and `PyCharm`_. The :file:`/home/user/qgis-dev`
directory will contain a local copy of the QGIS Developer Guide, a QGIS source
code repository and an empty :file:`qgis-build` directory.

.. _PyCharm: https://www.jetbrains.com/pycharm/

Create a Virtual Build Disk
---------------------------

See virtual_build_disk_.

Configuring Qt Creator
----------------------

.. note::

    See also: :ref:`config_qtcreator` for a 'regular' Ubuntu configuration.

#. Make an ``install`` location directory. Generally, this would not be
   necessary since the default of :file:`/usr/local` is appropriate. For the
   sake of this setup, we are not actually going to install, but run the built
   binary from the build directory.

   However, if we want to install, e.g. to test the install, this ensures we are
   not installing an ever-changing development version to our system::

       $ mkdir /home/user/qgis-install

#. Open the main CMakeLists.txt with Qt Creator::

       /user/user/qgis-dev/qgis-src/CMakeLists.txt

#. Paste a CMake option string. Note, this is sans the command's ``cmake``
   binary path prefix::

      # as multi-line command (incompatible with Qt Creator)
       -D CMAKE_INSTALL_PREFIX=/home/user/qgis-install \
       -D CMAKE_BUILD_TYPE=RelWithDebInfo \
       -D ENABLE_TESTS=TRUE \
       -D WITH_ASTYLE=TRUE \
       -D WITH_MAPSERVER=TRUE \
       -D WITH_STAGED_PLUGINS=FALSE \
       -D WITH_APIDOC=FALSE \
       -D WITH_QSCIAPI=FALSE \
       -D WITH_INTERNAL_QWTPOLAR=TRUE \
       -D WITH_GLOBE=FALSE \
       /home/user/qgis-dev/qgis-src

        # as single line
       ﻿-D CMAKE_INSTALL_PREFIX=/home/user/qgis-install -D CMAKE_BUILD_TYPE=RelWithDebInfo -D ENABLE_TESTS=TRUE -D WITH_ASTYLE=TRUE -D WITH_MAPSERVER=TRUE -D WITH_STAGED_PLUGINS=FALSE -D WITH_APIDOC=FALSE -D WITH_QSCIAPI=FALSE -D WITH_INTERNAL_QWTPOLAR=TRUE -D WITH_GLOBE=FALSE /home/user/qgis-dev/qgis-src

   .. warning::

      This single-line string is separated into multiple lines for clarity here.
      Do not paste it as is or Qt Creator will choke on it.

   .. note::

      See: :ref:`install_qgis_common` for info on working with CMake options.

#. Set project Build Steps:

   * Add a ``make`` step with a number of jobs argument relative to the number
     of available CPU cores you have to compile with::

         make -j4

   * Add a ``make`` step to stage the core Python plugins for use when running
     from the build directory (also re-compiles the Python modules)::

        make -j4 staged-plugins-pyc

   * Add a ``make`` step to install the build::

         make -j4 install

#. Set the Build Environment

Building QGIS
-------------

Open the Compile tab on the bottom of the main window, and expand it. Then,
click the compile icon:

.. figure:: /static/developer_guide/configuring_tools/qtcreator_08.jpeg
   :align: center

If QGIS compiles and installs OK, then for this setup you can tun off (don't
delete) the ``make -j4 install`` in Project Build Steps, since we only need to
compile and run/debug from build directory to test our source edits.

Once you have a functioning build setup, consider following the basic C++ Editing
tutorial.

.. _virtual_build_disk:

Virtual Build Disk
==================

When repeatedly compiling QGIS, quite a bit of drive space is used and many
files are written. Normally, on a physical hard drive, this eventually leads to
some fragmentation; however, on a virtual disk, this gradually also increases
the overall size of the file on disk.

One solution is to configure a Shared Folder to the physical disk and write the
build files out to it. Another is to create a separate virtual disk specifically
for the build files. Using the latter method, fragmentation on the physical
drive can be kept to a minimum, reclamation of the virtual disk space can
readily be done and the build files can easily be transferred to another
machine.

#. Power down the virtual machine.

#. Use VirtualBox to create another virtual hard drive, using a format efficient
   to VirtualBox, e.g. :file:`.vdi` or :file:`.vmdk`, and attached to your main
   Controller, which is usually ``SATA``.

   .. figure:: /static/developer_guide/virtual_machines/qgis-dev_add-build-hdd.png
       :align: center
       :width: 50em

   Allocate at least 8 GB, and be sure to define the type as dynamic; otherwise
   all hard drive space will be pre-allocated, regardless of whether it contains
   any data.

#. Start the virtual machine. You will not see the new HDD you created because
   it does not contain a filesystem yet, nor has it been mounted.

#. Open up ``Terminal Emulator`` and issue the following to find the device
   path for the unformatted hard drive, e.g. /dev/sdb::

       $ sudo fdisk -l
          [sudo] password for user:  <-- input: user

   Look for a disk that does not contain a filesystem and matches the space that
   you allocated.

#. Partition the drive and install a filesystem (assuming /dev/sdb is the
   location on your virtual build disk)::

       $ sudo fdisk /dev/sdb

   You should see some output that looks like the following. In the interactive::

        Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
        Building a new DOS disklabel with disk identifier 0x48551efe.
        Changes will remain in memory only, until you decide to write them.
        After that, of course, the previous content won't be recoverable.

        Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

        Command (m for help): p  <-- enter p for properties

        Disk /dev/sdb: 10.7 GB, 10737418240 bytes
        255 heads, 63 sectors/track, 1305 cylinders, total 20971520 sectors
        Units = sectors of 1 * 512 = 512 bytes
        Sector size (logical/physical): 512 bytes / 512 bytes
        I/O size (minimum/optimal): 512 bytes / 512 bytes
        Disk identifier: 0x48551efe

           Device Boot      Start         End      Blocks   Id  System

        Command (m for help): n   <-- enter n for partition
        Partition type:
           p   primary (0 primary, 0 extended, 4 free)
           e   extended
        Select (default p): p   <-- enter p for primary
        Partition number (1-4, default 1): 1
        First sector (2048-20971519, default 2048):  <-- use default
        Using default value 2048
        Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519):  <-- use default
        Using default value 20971519

        Command (m for help): p  <-- enter p for properties agian to verify

        Disk /dev/sdb: 10.7 GB, 10737418240 bytes
        255 heads, 63 sectors/track, 1305 cylinders, total 20971520 sectors
        Units = sectors of 1 * 512 = 512 bytes
        Sector size (logical/physical): 512 bytes / 512 bytes
        I/O size (minimum/optimal): 512 bytes / 512 bytes
        Disk identifier: 0x48551efe

           Device Boot      Start         End      Blocks   Id  System
        /dev/sdb1            2048    20971519    10484736   83  Linux

        Command (m for help): w  <-- enter w to write the partition table
        The partition table has been altered!

        Calling ioctl() to re-read partition table.
        Syncing disks.

#. Format the new partition with a ``ext4`` filesystem (assuming
   :file:`/dev/sdb1` is your partition path)::

       $ sudo mkfs -t ext4 /dev/sdb1

#. Make a mount point for mounting the new filesystem::

       $ sudo mkdir /media/qgis-build

#. Ensure the OSGeo-Live user has read-write access to the disk and mount point,
   e.g. set user:user as owner::

       $ sudo chown -R user:user /dev/sdb1
       $ sudo chown -R user:user /media/qgis-build

#. Add a label for the disk (make sure its not mounted)::

       sudo e2label /dev/sdb1 qgis-build

#. Find the UUID for volume. This is a unique identifier that will ensure correct
   mounting every time::

       $ sudo blkid

#. Edit /etc/fstab to automount drive on startup::

       $ sudo nano /etc/fstab

   And add the following lines to end of file (use your volume's UUID)::

       # qgis-build
       UUID=a5206def-d6dc-4b8c-bae3-6cb2797ea8bd /media/qgis-build ext4 defaults 0 0

#. Reboot the virtual machine.

Your virtual build drive should now be available at :file:`/media/qgis-build`.
You may wish to add it to the filesystem browser's sidebar for quick access.

