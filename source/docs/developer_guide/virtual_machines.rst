.. _virtual_machines:

*********************
Virtual Test Machines
*********************

OSGeo-Live QGIS-dev Virtual Machine
===================================

The purpose of this virtual machine (VM) conifiguration is to become familiar
with the base techniques of setting up a dev build enviroment for QGIS. It
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

you may wish to add a line to ``/etc/rc.local`` to have the Shared Folder automount on startup::

    $ sudo nano /etc/rc.local
      [sudo] password for user:  <-- input: user
    # add to end of /etc/rc.local, before 'exit 0' add the following

    mount -t vboxsf -o uid=user,rw GIS /home/user/GIS

    # WrtieOut changes and Exit nano

Also, there is an issue with the Thunar filesystem browser hanging on launches;
you may wish to fix with::

    # fix Thunar delay in opening filesystem browsers
    # see: https://bugs.launchpad.net/ubuntu/+source/thunar/+bug/775117
    #      https://bugs.launchpad.net/ubuntu/+source/thunar/+bug/775117/comments/54
    # open Applications->Accessories->Terminal Emulator
    $ cd /etc/samba
    $ sudo cp smb.conf smb.conf.backup
      [sudo] password for user:  <-- input: user
    $ sudo nano smb.conf
    # change:

    ;   name resolve order = lmhosts host wins bcast

    # to

       name resolve order = wins bcast

    # WrtieOut changes and Exit nano

Install QGIS-dev ISO contents
-----------------------------

Instead of having yet another VM, the ``qgis-dev`` setup is installed onto the
basic OSGeo-Live VM. This allows adding it to any exisitng VM a user may already
be running.

Download the `qgis-dev_osgeolive-79.iso <>`_ and attach the .iso file to the
VM's CD/DVD drive, while the VM is *powered down*.

.. figure:: /static/developer_guide/virtual_machines/qgis-dev_attach-iso.png
    :align: center
    :width: 50em


Upon startup and login, the .iso should automount on the Desktop as a
``qgis-dev``-named drive. Open the drive with the filesystem browser to make
sure it is mounted.

Once mounted, the drive is connected to the ``/media/qgis-dev`` mount point.

Go to :menuselection:`Applications --> Accessories --> Terminal Emulator` and
issue the following::

    $ cd /media/qgis-dev
    $ sudo ./1_install-qgis-dev.sh

    # interactive install
    $ ./2_install-qtcreator.sh

    # interactive install
    $ ./3_install-pycharm.sh

This will install all of the dependencies for building QGIS, Qt development
tools (including Qt Creator) and `PyCharm`_. The ``/home/user/qgis-dev`` directory
will contain a local copy of the QGIS Developer Guide, a QGIS source code
repository and an empty ``qgis-build`` directory.

.. _PyCharm: https://www.jetbrains.com/pycharm/
