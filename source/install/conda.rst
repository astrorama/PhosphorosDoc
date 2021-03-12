.. installation with Fedora & conda

This section contains instructions of how to download and install the
Phosphoros software under Linux and Mac OS operating systems. It also
includes instructions to install Phosphoros from source.

.. _fedora-installation:

************************************
Installation on Fedora
************************************

Stable version
=================

The Phosphoros software is available from Bintray. Create the file
``astrorama.repo`` under the ``/etc/yum.repos.d/`` directory with the
following content::

  [bintray--astrorama-fedora]
  name=bintray--astrorama-fedora
  baseurl=https://dl.bintray.com/astrorama/travis/master/fedora/$releasever/$basearch
  gpgcheck=0
  repo_gpgcheck=0
  enabled=1

To install the software, simply do::

  sudo dnf install Phosphoros

Phosphoros is now ready to run, and you can use any of the Phosphoros
commands, for example::

  Phosphoros GUI

.. warning::

   In principle, we only provide builds for the Fedora Supported
   Releases

.. warning::

   Bintray will be deprecated in May 2021. We will still provide RPMs,
   but it is still to be decided where to host them.

.. warning::

   If you get the following error in Fedora::

     PhosphorosUI: error while loading shared libraries: libnsl.so.1:
     cannot open shared object file: No such file or dir

   you need to install ``libnsl``::

     sudo dnf install libnsl
   
Development version
=====================

The *development* version of Phosphoros can be also obtained by changing
the content of the file ``/etc/yum.repos.d/astrorama.repo`` with::

  [bintray--astrorama-fedora]
  name=bintray--astrorama-fedora
  baseurl=https://dl.bintray.com/astrorama/travis/develop/fedora/$releasever/$basearch
  gpgcheck=0
  repo_gpgcheck=0
  enabled=1

Follow then the same instructions as above.


.. _conda-installation:

*********************************************
Installation on Linux and Mac OS systems
*********************************************

For other Linux distributions (or Fedora without requiring system
changes), and Mac OS, Phosphoros can be installed via ``conda``.

Stable version
=================

With the following instruction you create a conda environment named
``phosphoros`` and you provide the channel to search the Phosphoros
software. ::

  conda create -n phosphoros -c conda-forge -c astrorama phosphoros

We strongly advise to install Phosphoros on the conda environment to
avoid conflicts with other installed packages. To use the
``phosphoros`` environment, activate it::

  conda activate phosphoros

After running this command your terminal will be moved in the
``phosphoros`` environment. Now you can install Phosphoros by::
  
  conda install phosphoros

Phosphoros is now ready to run, and you can use any of the Phosphoros
commands, for example::

  Phosphoros GUI

When you want to leave the phosphoros environment, type::

  conda deactivate

Development version
=================

In order to employ the development version of Phosphoros, use the
following instruction::

  conda create -n phosphoros -c astrorama/label/develop -c astrorama -c conda-forge phosphoros

and follow the same instructions as above.
