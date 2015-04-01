.. _cli:

***************
|CLI| Reference
***************

There are two different ways for calling Phosphoros from the command line. The first is to call
the ``Phosphoros`` command providing an *action* and the second is to call the individual commands
directly.

Phosphoros command
==================

.. program:: Phosphoros

The ``Phosphoros`` command id a single entry point for using all the functionality provided by the
Phosphoros software.

Usage::
	
   Phosphoros <action> <action_options>
	
Actions List
------------

.. option:: help

   Lists the available actions
   
.. option:: GUI

   Starts the Graphical User Interface |br|
   Alternative executable: ``PhosphorosUI``
   
.. option:: build_templates, BT

   Builds the template photometry library |br|
   Alternative executable: ``PhosphorosBuildTemplates``
   
.. option:: derive_zero_points, DZP

   Calculates the Photometric Zero Point Corrections |br|
   Alternative executable: ``PhosphorosDeriveZeroPoints``
   
.. option:: fit_templates, FT

   Calculates the PHZ for a given catalog by using template fitting |br|
   Alternative executable: ``PhosphorosFitTemplates``
   
.. option:: ls_aux, LA

   Browses the auxiliary data in a given directory |br|
   Alternative executable: ``PhosphorosLsAux``
   
.. option:: display_templates, DT

   Browses through a template photometry library |br|
   Alternative executable: ``PhosphorosDisplayTemplates``
   
.. option:: display_dataset, DD

   Displays the SED data a template in the library |br|
   Alternative executable: ``PhosphorosDisplayDataset``
   
.. option:: vs_specz, VS

   Shows plots comparing the PHZ result with SPECZ |br|
   Alternative executable: ``PhosphorosVsSpecZ``
   
.. option:: display_likelihood, DL

   Plots views of a multi-dimensional likelihood |br|
   Alternative executable: ``PhosphorosDisplayLikelihood``
   
Generic options
===============

The following options are common to all the Phosphoros actions:

.. option:: --help

   Shows the help message for the given action

.. option:: --version

   Shows the version of the software
   
.. option:: --log-level <level>

   Sets the log level: NONE=0, FATAL=100, ERROR=200, WARN=300, INFO=400(default), DEBUG=500

.. option:: --log-file <filename>

   Sets the file to store the log. If this option is missing, the log messages are only shown on
   the screen.
   
.. option:: --config-file <filename>

   Use the given configuration file. If this option is missing, the automatically detected
   configuration file is used. Running the command ``Phosphoros <action> --help`` will show where
   is this default file.

Build Templates options
=======================

.. program:: PhosphorosBuildTemplates


