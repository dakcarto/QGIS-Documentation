.. _install_qgis_win:

*******************
Windows Compilation
*******************

.. contents::
   :local:
   :backlinks: top

Building with Microsoft Visual Studio
=====================================

This section describes how to build QGIS using Visual Studio on Windows. This is
currently also how the binary QGIS packages are made (earlier versions used
MinGW).

This section describes the setup required to allow Visual Studio to be used to
build QGIS.

Visual C++ Express Edition
--------------------------

The free (as in free beer) Express Edition installer is available under:

http://download.microsoft.com/download/d/c/3/dc3439e7-5533-4f4c-9ba0-8577685b6e7e/vcsetup.exe

The optional products are not necessary. In the process the Windows SDKs for
Visual Studio 2008 will also be downloaded and installed.

You also need the Microsoft Windows Server(R) 2003 R2 Platform SDK (for
setupapi):

http://download.microsoft.com/download/f/a/d/fad9efde-8627-4e7a-8812-c351ba099151/PSDK-x86.exe

You only need Microsoft Windows Core SDK / Build Environment (x86 32-Bit).

Other tools and dependencies
----------------------------

Download and install following packages:

+---------+--------------------------------------------------------------------------------------------+
| Tool    | Website                                                                                    |
+---------+--------------------------------------------------------------------------------------------+
| CMake   | http://www.cmake.org/files/v2.8/cmake-2.8.4-win32-x86.exe                                  |
+---------+--------------------------------------------------------------------------------------------+
| Flex    | http://gnuwin32.sourceforge.net/downlinks/flex.php                                         |
+---------+--------------------------------------------------------------------------------------------+
| Bison   | http://gnuwin32.sourceforge.net/downlinks/bison.php                                        |
+---------+--------------------------------------------------------------------------------------------+
| SVN     | http://sourceforge.net/projects/win32svn/files/1.6.13/Setup-Subversion-1.6.13.msi/download |
+---------+--------------------------------------------------------------------------------------------+
| or GIT  | http://msysgit.googlecode.com/files/Git-1.7.4-preview20110204.exe                          |
+---------+--------------------------------------------------------------------------------------------+
| OSGeo4W | http://download.osgeo.org/osgeo4w/osgeo4w-setup.exe                                        |
+---------+--------------------------------------------------------------------------------------------+

OSGeo4W does not only provide ready packages for the current QGIS release and
nightly builds of the trunk, but also offers most of the dependencies needs to
build it.

For the QGIS build you need to install following packages from OSGeo4W (select
Advanced Installation):

- expat
- fcgi
- gdal
- grass
- gsl-devel
- iconv
- pyqt4
- qt4-devel
- qwt5-devel-qt4
- sip
- spatialite
- libspatialindex-devel
- python-qscintilla

This will also select packages the above packages depend on.

Additionally QGIS also needs the include file unistd.h, which normally doesn't
exist on Windows. It's shipped with Flex/Bison in :file:`GnuWin32\\include` and
needs to be copied into the :file:`VC\\include` directory of your Visual C++
installation.

Earlier versions of this document also covered how to build all above
dependencies. If you're interested in that, check the history of this page in
the Wiki or the SVN repository.

Setting up the Visual Studio project with CMake
-----------------------------------------------

To start a command prompt with an environment that both has the VC++ and the
OSGeo4W variables create the following batch file (assuming the above packages
were installed in the default locations)::

  @echo off
  path %SYSTEMROOT%\system32;%SYSTEMROOT%;%SYSTEMROOT%\System32\Wbem;%PROGRAMFILES%\CMake 2.8\bin;%PROGRAMFILES%\subversion\bin;%PROGRAMFILES%\GnuWin32\bin
  set PYTHONPATH=

  set VS90COMNTOOLS=%PROGRAMFILES%\Microsoft Visual Studio 9.0\Common7\Tools\
  call "%PROGRAMFILES%\Microsoft Visual Studio 9.0\VC\vcvarsall.bat" x86

  set INCLUDE=%INCLUDE%;%PROGRAMFILES%\Microsoft Platform SDK for Windows Server 2003 R2\include
  set LIB=%LIB%;%PROGRAMFILES%\Microsoft Platform SDK for Windows Server 2003 R2\lib

  set OSGEO4W_ROOT=C:\OSGeo4W
  call "%OSGEO4W_ROOT%\bin\o4w_env.bat"

  @set GRASS_PREFIX=C:/OSGeo4W/apps/grass/grass-6.4.0
  @set INCLUDE=%INCLUDE%;%OSGEO4W_ROOT%\include
  @set LIB=%LIB%;%OSGEO4W_ROOT%\lib;%OSGEO4W_ROOT%\lib

  @cmd

Start the batch file and on the command prompt checkout the QGIS source from git
to the source directory :file:`QGIS`::

  git clone git://github.com/qgis/QGIS.git

Create a 'build' directory somewhere. This will be where all the build output
will be generated.

Now run cmake-gui (still from cmd) and in the *Where is the source code:* box,
browse to the top level QGIS directory.

In the *Where to build the binaries:* box, browse to the 'build' directory you
created.

If the path to bison and flex contains blanks, you need to use the short name
for the directory (i.e. ``C:\\Program`` Files should be rewritten to
``C:\\Progra~n``, where n is the number as shown in ``dir /x C:\\``).

Verify that the 'BINDINGS_GLOBAL_INSTALL' option is not checked, so that python
bindings are placed into the output directory when you run the INSTALL target.

Hit *Configure* to start the configuration and select Visual Studio 9 2008 and
keep native compilers and click Finish.

The configuration should complete without any further questions and allow you to
click *Generate*.

Now close cmake-gui and continue on the command prompt by starting vcexpress.
Use :menuselection:`File-->Open-->Project/Solutions` and open the
``qgis-x.y.z.sln`` file in your project directory.

Change Solution Configuration from ``Debug`` to ``RelWithDebInfo`` (Release with
Debug Info)  or ``Release`` before you build QGIS using the ``ALL_BUILD`` target
(otherwise you need debug libraries that are not included).

After the build completed you should install QGIS using the ``INSTALL`` target.

Install QGIS by building the ``INSTALL`` project. By default this will install
to :file:`C:\\Program Files\\qgis<version>` (this can be changed by changing the
``CMAKE_INSTALL_PREFIX`` variable in ``cmake-gui``).

You will also either need to add all the dependency DLLs to the QGIS install
directory or add their respective directories to your ``PATH``.

Packaging
---------

To create a standalone installer there is a perl script named
:file:`creatensis.pl` in :file:`qgis/ms-windows/osgeo4w`. It downloads all
required packages from OSGeo4W and repackages them into an installer using NSIS.

The script can be run on both Windows and Linux.

On Debian/Ubuntu you can just install the 'nsis' package.

NSIS for Windows can be downloaded at:

http://nsis.sourceforge.net

And Perl for Windows (including other requirements like 'wget', 'unzip', 'tar'
and 'bzip2') is available at:

http://cygwin.com

Packaging your own build of QGIS
--------------------------------

Assuming you have completed the above packaging step, if you want to include
your own hand built QGIS executables, you need to copy them in from your
windows installation into the ms-windows file tree created by the creatensis
script::

  cd ms-windows/
  rm -rf osgeo4w/unpacked/apps/qgis/*
  cp -r /tmp/qgis1.7.0/* osgeo4w/unpacked/apps/qgis/

Now create a package::

  ./quickpackage.sh

After this you should now have a nsis installer containing your own build
of QGIS and all dependencies needed to run it on a windows machine.

Osgeo4w packaging
-----------------

The actual packaging process is currently not documented, for now please take a
look at::

  ms-windows/osgeo4w/package.cmd

Building using MinGW
====================

Note: This section might be outdated as nowadays Visual C++ is use to build
the "official" packages.

Note: For a detailed account of building all the dependencies yourself you
can visit Marco Pasetti's website here:

http://www.webalice.it/marco.pasetti/qgis+grass/BuildFromSource.html

Read on to use the simplified approach with pre-built libraries...

MSYS
----

MSYS provides a unix style build environment under windows. We have created a
zip archive that contains just about all dependencies.

Get this:

http://download.osgeo.org/qgis/win32/msys.zip

and unpack to :file:`C:\\msys`.

If you wish to prepare your msys environment yourself rather than using
our pre-made one, detailed instructions are provided elsewhere in this
document.

Qt Toolkit
----------

Download Qt opensource precompiled edition exe and install (including the
download and install of mingw) from here:

http://qt.nokia.com/downloads/

When the installer will ask for MinGW, you don't need to download and install
it, just point the installer to :file:`C:\\msys\\mingw`.

When Qt installation is complete...

Edit :file:`C:\\Qt\\4.7.0\\bin\\qtvars.bat` and add the following lines::

  set PATH=%PATH%;C:\msys\local\bin;C:\msys\local\lib
  set PATH=%PATH%;"C:\Program Files\Subversion\bin"

I suggest you also add ``C:\Qt\4.7.0\bin`` to your Environment Variables
Path in the windows system preferences.

If you plan to do some debugging, you'll need to compile debug version of Qt::

  C:\Qt\4.7.0\bin\qtvars.bat compile_debug

Note: there is a problem when compiling debug version of Qt 4.7, the script ends
with this message  ``mingw32-make: No rule to make target 'debug'.  Stop.``.
To compile the debug version you have to go out of src directory and execute the
following command::

  C:\Qt\4.7.0 make

Flex and Bison
--------------

Get Flex
http://sourceforge.net/project/showfiles.php?group_id=23617&package_id=16424
(the zip bin) and extract it into :file:`C:\\msys\\mingw\\bin`.

Python stuff (optional)
-----------------------

Follow this section in case you would like to use Python bindings for QGIS.  To
be able to compile bindings, you need to compile SIP and PyQt4 from sources as
their installer doesn't include some development files which are necessary.

Download and Install Python - Use Windows Installer
...................................................

(It doesn't matter to what folder you'll install it)

http://python.org/download/

Download SIP and PyQt4 Sources
..............................

http://www.riverbankcomputing.com/software/sip/download
http://www.riverbankcomputing.com/software/pyqt/download

Extract each of the above zip files in a temporary directory. Make sure
to get versions that match your current Qt installed version.

Compile SIP
...........

::

  C:\Qt\4.7.0\bin\qtvars.bat
  python configure.py -p win32-g++
  make
  make install

Compile PyQt
............

::

  C:\Qt\4.7.0\bin\qtvars.bat
  python configure.py
  make
  make install

Final Python Notes
..................

.. warning::

    You can delete the directories with unpacked SIP and PyQt4 sources after a
    successfull install, they're not needed anymore.

git
---

In order to check out QGIS sources from the repository, you need a git client.
This installer should work fine:

http://msysgit.googlecode.com/files/Git-1.7.4-preview20110204.exe

CMake
-----

CMake is build system used by QGIS. Download it from here:

http://www.cmake.org/files/v2.8/cmake-2.8.2-win32-x86.exe

QGIS
----

Start a cmd.exe window ( :menuselection:`Start-->Run-->cmd.exe` ) Create
development directory and move into it::

  md C:\dev\cpp
  cd C:\dev\cpp

Check out sources from GIT::

  git clone git://github.com/qgis/QGIS.git

Compiling
---------

As a background read the generic building with CMake notes at the end of
this document.

Start a cmd.exe window ( :menuselection:`Start-->Run-->cmd.exe` ) if you don't
have one already. Add paths to compiler and our MSYS environment::

  C:\Qt\4.7.0\bin\qtvars.bat

For ease of use add ``C:\Qt\4.7.0\bin\\`` to your system path in system
properties so you can just type qtvars.bat when you open the cmd console.
Create build directory and set it as current directory::

  cd C:\dev\cpp\qgis
  md build
  cd build

Configuration
-------------

::

  cmakesetup ..

Note: You must include the '..' above.

Click *Configure* button.  When asked, you should choose 'MinGW Makefiles' as
generator.

There's a problem with MinGW Makefiles on Win2K. If you're compiling on this
platform, use 'MSYS Makefiles' generator instead.

All dependencies should be picked up automatically, if you have set up the
Paths correctly. The only thing you need to change is the installation
destination (``CMAKE_INSTALL_PREFIX``) and/or set 'Debug'.

For compatibility with NSIS packaging scripts I recommend to leave the install
prefix to its default :file:`C:\\program files\\`.

When configuration is done, click 'OK' to exit the setup utility.

Compilation and installation
----------------------------

::

   make
   make install

Run qgis.exe from the directory where it's installed (CMAKE_INSTALL_PREFIX)
---------------------------------------------------------------------------

Make sure to copy all :file:`dll` files needed to the same directory as the
qgis.exe binary is installed to, if not already done so, otherwise QGIS will
complain about missing libraries when started.

A possibility is to run qgis.exe when your path contains
:file:`C:\\msys\\local\\bin` and :file:`C:\\msys\\local\\lib` directories, so the DLLs
will be used from that place.

Create the installation package: (optional)
-------------------------------------------

Download and install NSIS from http://nsis.sourceforge.net/Main_Page.

Now using windows explorer, enter the win_build directory in your QGIS source
tree. Read the ``README`` file there and follow the instructions. Next right
click on qgis.nsi and choose the option 'Compile NSIS Script'.

Creation of MSYS environment for compilation of QGIS
====================================================

Initial setup
-------------

MSYS
....

This is the environment that supplies many utilities from UNIX world in Windows
and is needed by many dependencies to be able to compile.

Download from here:

http://puzzle.dl.sourceforge.net/sourceforge/mingw/MSYS-1.0.11-2004.04.30-1.exe

Install to :file:`C:\\msys`

All stuff we're going to compile is going to get to this directory (resp. its
subdirs).

MinGW
.....

Download from here:

http://puzzle.dl.sourceforge.net/sourceforge/mingw/MinGW-5.1.3.exe

Install to :file:`C:\\msys\\mingw`

It suffices to download and install only g++ and mingw-make components.

Flex and Bison
..............

Flex and Bison are tools for generation of parsers, they're needed for GRASS and
also QGIS compilation.

Download the following packages:

http://gnuwin32.sourceforge.net/downlinks/flex-bin-zip.php

http://gnuwin32.sourceforge.net/downlinks/bison-bin-zip.php

http://gnuwin32.sourceforge.net/downlinks/bison-dep-zip.php

Unpack them all to :file:`C:\\msys\\local`

Installing dependencies
-----------------------

Getting ready
.............

Paul Kelly did a great job and prepared a package of precompiled libraries for
GRASS. The package currently includes:

- zlib-1.2.3
- libpng-1.2.16-noconfig
- xdr-4.0-mingw2
- freetype-2.3.4
- fftw-2.1.5
- PDCurses-3.1
- proj-4.5.0
- gdal-1.4.1

It's available for download here:

http://www.stjohnspoint.co.uk/grass/wingrass-extralibs.tar.gz

Moreover he also left the notes how to compile it (for those interested):

http://www.stjohnspoint.co.uk/grass/README.extralibs

Unpack the whole package to :file:`C:\\msys\\local`

GRASS
.....

Grab sources from CVS or use a weekly snapshot, see:

http://grass.itc.it/devel/cvs.php

In MSYS console go to the directory where you've unpacked or checked out sources
(e.g. :file:`C:\\msys\\local\\src\\grass-6.3.cvs`)

Run these commands::

  export PATH="/usr/local/bin:/usr/local/lib:$PATH"
  ./configure --prefix=/usr/local --bindir=/usr/local \
  --with-includes=/usr/local/include --with-libs=/usr/local/lib \
  --with-cxx --without-jpeg --without-tiff --with-postgres=yes \
  --with-postgres-includes=/local/pgsql/include \
  --with-pgsql-libs=/local/pgsql/lib \
  --with-opengl=windows --with-fftw --with-freetype \
  --with-freetype-includes=/mingw/include/freetype2 --without-x \
  --without-tcltk --enable-x11=no --enable-shared=yes \
  --with-proj-share=/usr/local/share/proj

  make
  make install

It should get installed to :file:`C:\\msys\\local\\grass-6.3.cvs`

By the way, these pages might be useful:

- http://grass.gdf-hannover.de/wiki/WinGRASS_Current_Status
- http://geni.ath.cx/grass.html

GEOS
....

Download the sources:

http://geos.refractions.net/geos-2.2.3.tar.bz2

Unpack to e.g. :file:`C:\\msys\\local\\src`

To compile, I had to patch the sources: in file source/headers/timeval.h line
13. Change it from::

  #ifdef _WIN32

to::

  #if defined(_WIN32) && defined(_MSC_VER)

Now, in MSYS console, go to the source directory and run::

  ./configure --prefix=/usr/local
  make
  make install

SQLITE
......

You can use precompiled DLL, no need to compile from source:

Download this archive:

http://www.sqlite.org/sqlitedll-3_3_17.zip

and copy sqlite3.dll from it to :file:`C:\\msys\\local\\lib`

Then download this archive:

http://www.sqlite.org/sqlite-source-3_3_17.zip

and copy sqlite3.h to :file:`C:\\msys\\local\\include`

GSL
...

Download sources:

ftp://ftp.gnu.org/gnu/gsl/gsl-1.9.tar.gz

Unpack to :file:`C:\\msys\\local\\src`

Run from MSYS console in the source directory::

  ./configure
  make
  make install

EXPAT
.....

Download sources:

http://dfn.dl.sourceforge.net/sourceforge/expat/expat-2.0.0.tar.gz

Unpack to :file:`C:\\msys\\local\\src`

Run from MSYS console in the source directory::

  ./configure
  make
  make install

POSTGRES
........

We're going to use precompiled binaries. Use the link below for download:

http://wwwmaster.postgresql.org/download/mirrors-ftp?file=%2Fbinary%2Fv8.2.4%2Fwin32%2Fpostgresql-8.2.4-1-binaries-no-installer.zip

Copy contents of pgsql directory from the archive to :file:`C:\\msys\\local`.

Cleanup
-------

We're done with preparation of MSYS environment. Now you can delete all stuff in
:file:`C:\\msys\\local\\src` - it takes quite a lot of space and it's not necessary
at all.

