.. _source-installation:

************************
Installation from source
************************

Phosphoros installation involve installing a number of tools/packages in a
given order.

#. External dependencies
#. Elements (building and packaging framework)
#. Alexandria (SDC-CH generic C++ library)
#. PhosphorosCore (main package)
#. Phosphoros (Qt5-based GUI)

Here we explain how to achieve a single-user installation of Phosphoros in a
custom, local repository (for example $HOME/<applications>/<phosphoros>).

If you want to carry out another type of installation, please review
installation options on the following page
http://euclid.roe.ac.uk/projects/elements/wiki/InstallFromSource.


Installing External Dependencies
-----------------------------------

Please use the appropriate installer (mac ports, yum or apt-get for Mac OSX,
Linux Red-Hat or Debian families, respectively) to install the following
dependencies:

+-----------------+------------+---------------------------------------------------------+
| Name            | Version    | Note                                                    |
+=================+============+=========================================================+
| make            | latest     |                                                         |
+-----------------+------------+---------------------------------------------------------+
| CMake           | latest     | The underlying building tool                            |
+-----------------+------------+---------------------------------------------------------+
| Python          | 2.7 or 3.x | Preferably 3.x, but Elements is compatible with both    |
+-----------------+------------+---------------------------------------------------------+
| C++ compiler    | latest     | clang++ from XCode for MacOSX or GNU C++ for Linux      |
+-----------------+------------+---------------------------------------------------------+
| Boost           | latest     | including the "devel" part                              |
+-----------------+------------+---------------------------------------------------------+
| log4cpp         | latest     | including the "devel" part                              |
+-----------------+------------+---------------------------------------------------------+
| sphinx          | latest     | python3-sphinx in Fedora (1)                            |
+-----------------+------------+---------------------------------------------------------+
| sphinx-apidoc   | latest     | python3-sphinxcontrib-apidoc in Fedora (1,2)            |
+-----------------+------------+---------------------------------------------------------+
| cfitsio         | latest     |                                                         |
+-----------------+------------+---------------------------------------------------------+
| CCFITS          | 2.5 and up |                                                         |
+-----------------+------------+---------------------------------------------------------+
| Qt              | 5.x        |                                                         |
+-----------------+------------+---------------------------------------------------------+

The names provided above may not correspond to the distribution-specific package names. Please check your system for
the exact naming.

(1) Can be skipped adding ``-DUSE_SPHINX=OFF`` to ``CMAKEFLAGS``
(2) Can be skipped adding ``-DUSE_SPHINX_APIDOC=OFF`` to ``CMAKEFLAGS``

User configuration
---------------------

Edit your configuration (.bashrc or equivalent for other shell) and define:

.. code:: bash

    export CMAKE_PROJECT_PATH=$HOME/<applications>
    export CMAKE_PREFIX_PATH=$CMAKE_PROJECT_PATH/Elements/5.10/cmake
    export CMAKEFLAGS=""

Note that ``<applications>`` is just a placeholder that can be replaced with
the most convenient location. It must exist before running the next steps.

.. _customize some parts of the build process: https://euclid.roe.ac.uk/projects/elements/wiki/NewUserReference

``CMAKEFLAGS`` can be used to `customize some parts of the build process`_.
It can be used, for instance, to disable building the documentation of the
API (see previous section).

Elements 5.10
-------------

Run the following commands:

.. code:: bash

    cd $CMAKE_PROJECT_PATH
    mkdir -p Elements/5.10
    wget https://github.com/astrorama/Elements/archive/5.10/Elements-5.10.tar.gz
    tar xzf Elements-5.10.tar.gz --strip-components 1 -C Elements/5.10
    cd Elements/5.10
    make -j
    make install

.. note::
  By default, ``make -j`` will do a parallel build using all available CPUs.
  If this makes your system unresponsive, you can manually specify the number
  of  CPUs: for instance ``make -j4`` to use only 4.


Alexandria 2.17, PhosphorosCore 0.17 and Phosphoros 0.17
--------------------------------------------------------

Please repeat the above Elements-related instruction for:

.. code:: bash

    # Alexandria-2.17
    wget https://github.com/astrorama/Alexandria/archive/2.17/Alexandria-2.17.tar.gz
    # PhosphorosCore-0.17
    wget --header "PRIVATE-TOKEN: <your-access-token>" "https://gitlab.euclid-sgs.uk/api/v4/projects/PF-PHZ%2FPhosphorosCore/repository/archive?sha=0.17" -O PhosphorosCore-0.17.tar.gz
    # Phosphoros-0.17
    wget --header "PRIVATE-TOKEN: <your-access-token>" "https://gitlab.euclid-sgs.uk/api/v4/projects/SDC-CH%2FPhosphoros/repository/archive?sha=0.17" -O Phosphoros-0.17.tar.gz

(replacing "Elements-5.10" by the above names in the procedure)

.. _Gitlab Access Token: https://gitlab.euclid-sgs.uk/-/profile/personal_access_tokens

.. warning::
  For PhosphorosCore and Phosphoros you will need to generate a
  `Gitlab Access Token`_ with the ``read_repository`` permission.

Running Phosphoros
------------------

For your convenience, the best is to create an alias to the Phosphoros command
by adding the following line in your .bashrc file:

.. code:: bash

    alias Phosphoros="python $CMAKE_PROJECT_PATH/Elements/5.10/InstallArea/<binary-tag>/cmake/scripts/env.py --xml /innerhome/fake/Applications/Phosphoros/0.17/InstallArea/<binary-tag>/PhosphorosEnvironment.xml Phosphoros"

You can then type:

.. code:: bash

    Phosphoros --help

to get command line option help and test that your installation is OK, and:

.. code:: bash

    Phosphoros GUI

to invoke Phosphoros GUI.

.. note::
  ``<binary-tag>`` is a system-dependent location. It will be easy to see which
  one corresponds to yours just doing an ``ls`` of the ``InstallArea`` directory.
  It normally looks something like ``x86_64-fc33-gcc102-o2g``
  (``<architecture>-<os>-<compiler>-<build-type>``).

.. _EuclidEnv: https://gitlab.euclid-sgs.uk/ST-TOOLS/ST_EuclidEnv

.. note::
  If you have EuclidEnv_ installed, you can use instead:

  .. code:: bash

    alias Phosphoros=". ELogin.sh; E-Run --no-user Phosphoros 0.17 Phosphoros"
