Executing Phosphoros
====================

The Phosphoros software provides two different execution modes:

 * the |CLI| allows execution from the command line, and
 * the |GUI| provides a graphical interface for interactive execution.

.. _cli-execution-mode:

Using |CLI| Execution Mode
--------------------------

Any Phosphoros processing can be started with the ``Phosphoros`` main command. The syntax is as follow::

   > Phosphoros <action> <action_parameters>  

where

**action**: is a keyword which refers to a specific Phosphoros executable |br|
**action_parameters**: refers to the options related to the action (executable) |br|

The Phosphoros command (without parameters or with ``--help``) shows the available actions::

   > Phosphoros

A help message for each available ``action`` can be displayed with::

  > Phosphoros <action> --help

Actions have a long and a short names (e..g, compute_model_grid == CMG)
  
Using |GUI| Execution Mode
--------------------------

The Phosphoros |GUI| provides a user friendly execution mode. Launching the |GUI| can be done from the command line,
with the ``GUI`` action::

   > Phosphoros GUI

Processing root directory
-------------------------

Phosphoros uses the ``$HOME/Phosphoros`` directory as the default root directory. A different root directory can
be specified by setting the environment variable ``PHOSPHOROS_ROOT`` to an alternative directory.


.. _config-file-usage:

Configuration files
-------------------

The list of command line options can be quite long and it is often more convenient to use a configuration file.
Phosphoros first looked into the::

 > $HOME/Phosphoros/conf

directory and it will use configuration files from that location **if** they have the exact expected names.

When Phosphoros cannot find the appropriate file there, it will pick up the system configuration files located at::

 <EUCLID_BASE>/<PROJECT_NAME>/<VERSION>/InstallArea/<BINARY_TAG>/conf/PhzExecutables/*.conf

These files are read only though, and this is why the default read/write ``$HOME/Phosphoros/conf/`` location is proposed for users
configuration.

An alternative configuration file can also be provided with the generic ''config-file`` option::

﻿ --config-file=<PATH>/<FILENAME>

Please note that the ``.conf`` extension is a naming convention which is not mandatory (i.e., the software will accept any other specified extension).

Users can either copy the system configuration file before modifying them, but the most convenient approach is to use
the GUI to output configuration files. The GUI can be conveniently use to enter all options in an interactive mode. When
this is completed, there is GUI ``Get Config. File`` option which allows to save the corresponding configuration files
at the ``$HOME/Phosphoros/conf``location. Beware to give the correct file name in this case. Configuration files produced
with the GUI can also be more conveniently copy, paste and edited for small changes.

Any command line options takes precedence over the equivalent option provided through a configuration file.