Computing Redshifts
===================

GUI: ``Compute Redshifts``
--------------------------

.. figure:: /_static/first_step/InputOutputSubpanel4.png
    :align: center

We focus here on the sub-panel four. This is the last step for computing the photometric redshifts. It provides you the
best fit catalog with the best fitted models for the catalog containing your sources. |br|

To do so, you need to define the following:

1. **Input Catalog File**
 
 Provide a photometry catalog (FITS or ASCII format) containing your sources 
 (See format details :ref:`here <format-reference-section>`).
 
2. **Output catalog**

 Select the output format of the Phosphoros catalog you wish. It is
 either ASCII or FITS format (See format details :ref:`here <format-reference-section>`).
 Also select if the best fitted model columns will be included or not in the
 catalog.
 
3. **1D PDF**
 
 Select the parameter space axes for which the marginalized 1D PDF will be
 produced. They can be generated as columns of the output catalog (containing
 vector data) or as individual FITS files, one per axis.
 (See format details :ref:`here <format-reference-section>`).

4. **Multi-Dimensional Output**
 
 Enable the generation of the likelihood or posterior FITS files, one per source 
 (See format details :ref:`here <format-reference-section>`).
 
5. **Output Folder**
 
 The Phosphoros final results are stored there. The output folder is provided
 by default and is located to::
 
 $PHOSPHOROS_ROOT/Results/Catalog/"Catalog Type" /"Catalog File Name"
 
 But you can choose another location by clicking on the ``Browse`` button. The
 ``Catalog Type`` is the one defined at the top left of the image. The ``Catalog
 File Name`` is the name of catalof file without the extension.

Note you do not need to go through all the points above. Select just the ones you
need. As shown in the above image the ``Run`` button is inactive for our example. It
means that something is not setup yet and the computing can not be done. In such
case, just put your mouse pointer on the ``Run`` button and some help will appear
explaining you what is missing

The bottom right buttons (the blue rectangle on the image):

- **Run**

 You click on this button to start the computation of the redshifts.
 All your results are written into the ``Output Folder`` you have defined.
 (see details about output files produced :ref:`here <format-reference-section>`)
 
- **Get Config File**

 Use this button to export your settings into a configuration file. The file is stored into::
 
$PHOSPHOROS_ROOT/config/PhosphorosComputeRedshift.conf

This configuration file then be used for command line execution of the PhosphorosComputeRedshift C++ executable.

CLI: ``PhosphorosComputeRedshift.conf``
---------------------------------------

The redshifts are computed by calling the PhosphorosComputeRedshift C++ executable. This executable can be directly
called from a terminal and the list of command line options can be display through the help message::

    ﻿Phosphoros compute_redshift (or CR) --help

Options can be passed on the command line or with a configuration file (see :ref:`here <config-file-usage>` for
configuration file best usage).

A complete and fully documented PhosphorosComputeRedshift.conf is available as part of the installed software in the following location::

    /path_to_PhosphorosCore_installation_directory/conf/PhzExecutables/PhosphorosComputeRedshift.conf

With a system installation, ``/path_to_PhosphorosCore_installation_directory`` should be something like
``/opt/euclid/PhosphorosCore/0.5/InstallArea/x86_64-fc23-gcc53-o2g/" where ``0.5`` is PhosphorosCore version and
``x86_64-fc23-gcc53-o2g`` reflect some of the compilation options. These two fields takes different values in different installations.
