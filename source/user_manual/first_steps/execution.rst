Executing Phosphoros
====================

The Phosphoros software provides two different execution modes:

 * the |CLI| which allows execution from the command line, and 
 * the |GUI| which provides a graphical interface for interactive execution.

.. _cli-execution-mode:

Using |CLI| Execution Mode
--------------------------

The |CLI| consists of the main entry command ``Phosphoros``. |br|

Phosphoros uses the ``PHOSPHOROS_ROOT`` environment variable to locate all necessary information for
the computation (configuration files, catalogs, output files etc..). If this variable is not defined, the ``$HOME/Phosphoros`` directory 
is considered as the default location.

``Phosphoros`` command can be used to execute all Phosphoros actions, using the following syntax::

   > Phosphoros <action> <action_parameters>  

where

**action**: is a keyword which referrers to a specific Phosphoros executable |br|
**action_parameters**: referrers to the parameters of the action (executable) |br|

Let's take this following example::

 > Phosphoros compute_model_grid --output-model-grid="filename.fits"

* ``compute_model_grid`` (or using the alias CMG) is the action which calls the ``PhosphorosComputeModelSed`` executable for computing the model grid. |br|
* ``--output-model-grid`` is one of the parameters of this ``PhosphorosComputeModelSed`` executable

Use the following command to display the available actions::

   > Phosphoros 

And use the following command to display the available action parameters::

  > Phosphoros <action> --help (e.g. Phosphoros compute_model_grid --help)
  
Using |GUI| Execution Mode
--------------------------

The |GUI| of Phosphoros provides a more user friendly alternative of the |CLI|.
Launching the |GUI| can be done from the command line, by executing the ``GUI``
action::

   > Phosphoros GUI