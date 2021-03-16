.. _conda:

****************************************
Installation on Linux and Mac OS systems
****************************************

For Linux distributions and Mac OS, Phosphoros can be installed via ``conda``.

Stable version
==============

With the following instruction you create a conda environment named
``phosphoros`` and you provide the channel to search the Phosphoros
software. ::

  conda create -n phosphoros -c conda-forge -c astrorama phosphoros

We strongly advise to install Phosphoros on a separate conda environment to
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

.. warning::

   If you get the following error in Fedora::

     PhosphorosUI: error while loading shared libraries: libnsl.so.1:
     cannot open shared object file: No such file or directory

   you need to install ``libnsl``::

     sudo dnf install libnsl


Development version
===================

In order to employ the development version of Phosphoros, use the
following instruction::

  conda create -n phosphoros -c astrorama/label/develop -c astrorama -c conda-forge phosphoros

and follow the same instructions as above.
