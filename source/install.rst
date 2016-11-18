.. _phosphoros-install:

*************************
Download and Installation
*************************

Instructions on how to download and install the the Phosphoros software suite are provided for

- :ref:`MacOSX_section`

- :ref:`Linux_section`

- :ref:`Source_Installation_section`

.. _MacOSX_section:

Mac OSX
=======

Phosphoros installation  with the MacPorts tool. MacPorts installation
instructions can be found on this
`link <https://www.macports.org/install.php>`_.  If you do not want to use
MacPorts, please try the source code installation instructions provided
below.

Phosphoros MacPorts installation:

#. Update the source location by adding the following line:  
      | "http://degauden.isdc.unige.ch/euclid/repo/macports/ports.tar.gz" 
      | into  the 
      | "/opt/local/etc/macports/sources.conf" 
      | configuration file
#. Synchronized the port DataBase with:
      | > sudo port sync 
#. Install the software: 
      | > sudo port install Phosphoros-0.6
#. Test your installation by launching the GUI with:  
      | > Phosphoros GUI

.. _Linux_section:

Linux
=====

We support CentOS7 and Fedora 23 installations with RPM packages.

CentOS7

   #. Download http://degauden.isdc.unige.ch/euclid/repo/centos/7/x86_64/EuclidRepo-0.1-1.noarch.rpm
   #. Execute: 
         | > sudo yum localinstall EuclidRepo-0.1-1.noarch.rpm
   #. Install Phosphoros:
         | > sudo yum install Phosphoros-devel

Fedora 23

   #. Download http://degauden.isdc.unige.ch/euclid/repo/fedora/23/x86_64/EuclidRepo-0.1-1.noarch.rpm
   #. Execute: 
         | > sudo dnf install EuclidRepo-0.1-1.noarch.rpm
         | > sudo dnf install gcc-c++
         | > sudo dnf install EuclidEnv

   #. Reboot your computer

   #. Install Phosphoros with:
        | > sudo dnf install Phosphoros-devel

   #. Create an alias by adding the following line in your .bashrc file:
        | alias Phosphoros="ERun Phosphoros 0.6 Phosphoros"

   #. Test your installation by launching the GUI with:
        | > Phosphoros GUI

Please note that you will have to update the alias every time you update to a new version of
Phosphoros.

For other Linux distributions, you have to install the software from the sources as described in the next section.

.. When installing via the RPMs, you will have to run Phosphoros using the `ERun`
   command. For your convenience, you can create an alias to the Phosphoros command by adding the following line in your .bashrc file::
    
    alias Phosphoros="ERun Phosphoros 0.5 Phosphoros"

    where you have to replace the version with the one you just installed. Note that
    you will have to update the alias every time you update to a new version of Phosphoros.

.. _Source_Installation_section:

Installation from sources
=========================

Phosphoros installation involve installing a number of tools/packages in a given order.

#. External dependencies
#. EuclidEnv (temporary)
#. Elements (building and packaging framework)
#. Alexandria (SDC-CH generic C++ library)
#. PhosphorosCore (main package)
#. Phosphoros (Qt4-based GUI)

Here we explain how to achieve a single-user installation of Phosphoros in a custom, local repository
(for example $HOME/<applications>/<phosphoros>). If you want to carry out another type of installation, please review
installation options on the following page
http://euclid.roe.ac.uk/projects/elements/wiki/InstallFromSource.


1. Installing External Dependencies
-----------------------------------

Please use the appropriate installer (mac ports, yum or apt-get for Mac OSX, Linux Red-Hat or Debian families, respectively)
to install the following dependencies:

+-----------------+------------+---------------------------------------------------------+
| Name            | Version    | Note                                                    |
+=================+============+=========================================================+
| CMake           |  latest    | The underlying building tool                            |
+-----------------+------------+---------------------------------------------------------+
| Python          | 2.7 or 3.x | Your own choice, Elements is compatible with both       |
+-----------------+------------+---------------------------------------------------------+
| C++ compiler    | latest     | clang++ from XCode for MacOSX or GNU C++ for Linux      |
+-----------------+------------+---------------------------------------------------------+
| Boost           | latest     | including the "devel" part                              |
+-----------------+------------+---------------------------------------------------------+
| log4cpp         | latest     | including the "devel" part                              |
+-----------------+------------+---------------------------------------------------------+
| cfitsio         | latest     |                                                         |
+-----------------+------------+---------------------------------------------------------+
| CCFITS          | 2.5 and up |                                                         |
+-----------------+------------+---------------------------------------------------------+
| Qt              | 4.x        |                                                         |
+-----------------+------------+---------------------------------------------------------+

The names provided above may not correspond to the distribution-specific package names. Please check your system for
the exact naming.

2. EuclidEnv
------------

This EnclidEnv package provides 2 main scripts ELogin and ERun which setup, respectively, the build and the runtime
environments of any Elements-based project such as Phosphoros. The installation of EuclidEnv is currently required, but
it will not be necessary in future versions anymore::

    cd $HOME/tmp/ (or any other temporary directory)
    wget http://degauden.isdc.unige.ch/euclid/repo/sources/EuclidEnv-....tar.gz (check version number)
    tar xzf EuclidEnv-....tar.gz
    cd EuclidEnv...
    mkdir -p $HOME/<applications>/<phosphoros>/env
    python setup.py install --prefix=$HOME/<applications>/<phosphoros>/env

3. User configuration
---------------------

Edit your configuration (.bashrc or equivalent for other shell) and defined::

    export PATH=$HOME/<applications>/<phosphoros>/env:${PATH}
    export EUCLID_BASE=$HOME/<applications>/<phosphoros>/base

EUCLID_BASE defines the location where all Phosphoros related software will be installed. Make sure this directory exist::

    mkdir -p $EUCLID_BASE

4. Elements 4.0
---------------

Sources are first built in a temporary location and then install in $EUCLID_BASE::

    cd $HOME/tmp/ (or any other temporary directory)
    wget http://degauden.isdc.unige.ch/euclid/repo/sources/Elements-4.0.tar.gz
    tar xzf Elements-4.0.tar.gz
    cd Elements-4.0
    . ELogin.sh (setup the build environment)
    mkdir build
    cd build
    cmake -DCMAKE_TOOLCHAIN_FILE=$CMAKE_PREFIX_PATH/ElementsToolChain.cmake  ../
    make
    make install

5. Alexandria 2.6, PhosphorosCore-0.6 and Phosphoros-0.6
--------------------------------------------------------

Please repeat the above Elements-related instruction for::

    Alexandria-2.6
    PhosphorosCore-0.6
    Phosphoros-0.6

(replacing "Elements-4.0" by the above names in the procedure)

6. Running Phosphoros
---------------------

For your convenience, the best is to create an alias to the Phosphoros command
by adding the following line in your .bashrc file::
    
    alias Phosphoros=". ELogin.sh; E-Run --no-user Phosphoros 0.6 Phosphoros"

You can then type::

    Phosphoros --help

to get command line option help and test that your installation is OK, and::

    Phosphoros GUI

to invoke Phosphoros GUI

Python packages
===============

To run some of the investigation tool, you will need the following Python packages

- NumPy
- Astropy
- Mathplotlib

