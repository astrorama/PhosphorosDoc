.. _source-installation:

************************
Installation from source
************************

Phosphoros installation involve installing a number of tools/packages in a given order.

#. External dependencies
#. Elements (building and packaging framework)
#. Alexandria (SDC-CH generic C++ library)
#. PhosphorosCore (main package)
#. Phosphoros (Qt5-based GUI)

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
| Qt              | 5.x        |                                                         |
+-----------------+------------+---------------------------------------------------------+

The names provided above may not correspond to the distribution-specific package names. Please check your system for
the exact naming.

2. User configuration
---------------------

Edit your configuration (.bashrc or equivalent for other shell) and defined::

    export CMAKE_PROJECT_PATH=$HOME/<applications>
    export CMAKE_PREFIX_PATH=$CMAKE_PROJECT_PATH/Elements/5.8/cmake

3. Elements 5.8
---------------

    cd $CMAKE_PROJECT_PATH
    mkdir -p Elements/5.8
    wget https://github.com/astrorama/Elements/archive/5.8.tar.gz
    tar xzf 5.8.tar.gz --strip-components 1 -C Elements/5.8
    cd Elements/5.8
    make -j
    make install

4. Alexandria 2.15, PhosphorosCore-0.6 and Phosphoros-0.6
---------------------------------------------------------

Please repeat the above Elements-related instruction for::

    Alexandria-2.6
    PhosphorosCore-0.6
    Phosphoros-0.6

(replacing "Elements-4.0" by the above names in the procedure)

5. Running Phosphoros
---------------------

For your convenience, the best is to create an alias to the Phosphoros command
by adding the following line in your .bashrc file::

    alias Phosphoros=". ELogin.sh; E-Run --no-user Phosphoros 0.6 Phosphoros"

You can then type::

    Phosphoros --help

to get command line option help and test that your installation is OK, and::

    Phosphoros GUI

to invoke Phosphoros GUI
