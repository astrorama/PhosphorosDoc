
***************************
First Steps with Phosphoros
***************************

What is template fitting
========================

Small very basic description so the reader understands what the software tries
to do, but it should be a bit longer that what goes to the introduction chapter.

Phosphoros Workflow
===================

It starts with a paragraph explaining the three kind of steps: model grid generation,
optional steps and redshift computation.

Introduces the concept of the parameter space. Explains that the models are the
computed photometries.

This is at theoretical level. Diagrams should be used, files or directories not.

Some important concepts
=======================

.. Explain the logic behind the organization of the Phosphoros directories. This
should include the catalog-type concept. Here we should not explain every single
one of the directories, but focus more on the concept and mention the most used
ones. We should also mention the PHOSPHOROS_ROOT environment variable.

Catalog type
------------

Modern surveys typically make their catalog data available trough a number of 
files, either because of the big size and/or because there are different samples (with 
or without spectroscopic redshift for example). Assuming that these files are
standard in a given survey, with the same column names in particular, the concept 
of ``catalog type`` is introduced. We consider that all catalog data files having
identical column names, with photometric band names referring to the same filter
belong to the same ``catalog type``.

One of the required configuration task is the mapping of the catalog photometry column names to the
files providing the filter transmission curves. This mapping can
be defined only once for all files of the same type ``catalog type``.

The ``catalog type`` concept is also exploited to define the directory structure naming convention
used to organize Phosphoros files and described in the next section.

.. _directory-organization:

Directory Organization
----------------------

All data files are organized into a standardized directory structure. This allows to greatly
reduce the amount of specifications as the code automatically knows where to look for
input data and where to write output files without necessarily relying on additional user configuration.

Below the root directory ($HOME/Phosphoros by default), there are five main directories.

**Catalogs**:
    input catalogs files located into sub-directories according to their ``catalog types`` (e.g. Cosmos
    catalog files would be located into "$HOME/Phosphoros/Catalogs/Cosmos/"

**AuxiliaryData**:
    auxiliary data includes (1) filter transmission curves, (2) Spectral Energy Distribution (SED) templates, (3) reddening curve files, etc.

**IntermediateProducts**
    intermediate products organized per ``catalog type`` which can be reused for different runs

**Results**
    resulting data products also organized per ``catalog type``

**config**
    configuration files

In most cases, there is no need to alter the standard directory structure, nor even to understand all details. The simplest is to used the default configuration.
The location of the top-level directory can simply be specified by setting the PHOSPHOROS_ROOT environment variable.

In case of particular needs, the location of the above five directories can be changed using options which are described in the following advanced :ref:`section. <directory_howto_section>`

*Explain the logic behind the organization of the Phosphoros directories. This
should include the catalog-type concept. Here we should not explain every single
one of the directories, but focus more on the concept and mention the most used
ones. We should also mention the PHOSPHOROS_ROOT environment variable.*

Phosphoros "internal" data
--------------------------

Much of the data manipulated by Phosphoros can be reused in different analyses. The directory structure described in previous section
is designed to hold the input, intermediate and output data files of an arbitrary number of analyses. It can be seen as an kind of underlying "internal data base". It is
however not making use of any real databases (such as mysql) as it just relies on the file system organisation.

When catalog files are put at the appropriate location (according to their catalog types), intermediate data products final results are sorted in such a way as to co-exist
with equivalent files obtained from other catalogs analyses. The idea is to ``save`` the results of as many analyses as wanted side by side in as logical an organisation as possible.
There are also however option to overwrite or delete any Phosphoros outputs.

The standard procedure is to ``import`` auxiliary data files, such as filter transmission or SEDs, into this kind of underlying
data base. As it relies on the file system, any operation performs with the GUI such as ``importing``, ``moving`` of ``deleting`` files as a simple shell command line
equivalent (cp, mv or rm).

Executing Phosphoros
====================

The Phosphoros software provides two different execution modes:

 * the |CLI| which allows execution from the command line, and 
 * the |GUI| which provides a graphical interface for interactive execution.

Using |CLI| Execution Mode
--------------------------

The |CLI| consists of the main entry command ``Phosphoros``. |br|

Phosphoros uses the ``PHOSPHOROS_ROOT`` environment variable to locate all necessary information for
the computation (configuration files, catalogs, output files etc..). If this variable is not defined, the ``$HOME/Phosphoros`` directory 
is considered as the default location.

`Phosphoros`` command can be used to execute all Phosphoros actions, using the following syntax::

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


.. _setup-input-data:
    
Setting up the input data
=========================

..  First explain what the input data are. At this level we should limit it to the
    catalogs, filters, SEDs and reddening curves. We should not describe the formats
    of the files, but have links to the format reference section.

The main Phosphoros input data is a **catalog** file. A catalog include photometric
measurements obtained through a set of different filters as columns, with their
corresponding errors and either in flux or magnitude.

Import using the GUI
--------------------

Import manually
---------------

.. _catalog-column-mapping:
    
Mapping the catalog columns
===========================

Explain again the concept of the catalog type.

GUI how-to
----------

CLI how-to
----------

filter_mapping.txt explanation

.. _parameter-space-definition:
    
Defining the model parameter space
==================================

Explain the concept of the parameter space. Specify the axes. Not mention yet
the sparse parameter space.

GUI how-to
----------

Show an example with multiple ranges and values

CLI how-to
----------

Explain the related configuration options, which map to the same example shown
at the GUI

Generating the models
=====================

GUI
---

Show the steps and explain where the file is created. Here we should explain the
color codes of the Compute Redshifts panel.

CLI
---

Show the command. Mention the default configuration file name. Explain where the
files are created (and the reasoning behind the default naming).

Computing the Redshift
======================

GUI
---

Show the steps and explain the different outputs (and link to the format descriptions).
Explain where the data are created. Have a link back to the directory organization
strucutre.

CLI
---

Show the command. Mention the default configuration file name. Show the same steps
as for the GUI.

Examining the results
=====================

Explain that there are some tools for that and that are available only at CLI.

Explain how to use the plot_specz_comparison for seeing the specz-phz plot and
the PDFs. Also mention the SAMP functionality.

