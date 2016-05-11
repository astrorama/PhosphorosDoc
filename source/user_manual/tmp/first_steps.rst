
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


.. _catalog-type:

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
    catalog files would be located into "$HOME/Phosphoros/Catalogs/Cosmos/")

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

.. Explain the logic behind the organization of the Phosphoros directories. This
    should include the catalog-type concept. Here we should not explain every single
    one of the directories, but focus more on the concept and mention the most used
    ones. We should also mention the PHOSPHOROS_ROOT environment variable.*

Phosphoros "internal" data
--------------------------

Much of the data manipulated by Phosphoros can be reused in different analyses. The directory structure described in previous section
is designed to hold the input, intermediate and output data files of an arbitrary number of analyses. It can be seen as an kind of underlying "internal data base". It is
however not making use of any real databases (such as mysql) as it just relies on the file system organisation.

When catalog files are put at the appropriate location (according to their catalog types), intermediate data products and final results are sorted in such a way as to co-exist
with equivalent files obtained from other analyses. The idea is to ``save`` the results of as many analyses as wanted side by side in as logical an organisation as possible.
There are also however options to overwrite or delete any Phosphoros outputs.

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
    
Setting up input data
=====================

..  First explain what the input data are. At this level we should limit it to the
    catalogs, filters, SEDs and reddening curves. We should not describe the formats
    of the files, but have links to the format reference section.

Catalogs
--------

The main Phosphoros input data is a **catalog** file. A catalog is a table including
as columns photometric measurements obtained through a number of different filters,
with their corresponding errors, either in flux or magnitude. Rows refer to different sources
and one column named `ÌD`` must be present. The catalog format is either ASCII or FITS
as described in this section (TBD). Fluxes must be provided in |mu|\ Jy unit.

Input catalog files can be examined using TOPCAT (http://www.star.bris.ac.uk/~mbt/topcat/) for example.
They must be placed in the $PHOSPHOROS_ROOT directory ($HOME/Phosphoros by default) before starting any
analyses, using shell commands (mv or cp). Files must be organized into sub-directories according to
their different type (see above catalog type concept :ref:`section. <catalog-type>`).

For example, considering a set of catalogs from the ``COSMOS`` and the ``Euclid Challenge 2`` types, the
corresponding files must be located into

    > $HOME/Phosphoros/Catalogs/COSMOS/...

and

    > $HOME/Phosphoros/Catalogs/Challenge2/...

respectively.

Auxiliary Data
--------------

All input files which are not catalogs are referred to as ``Auxiliary`` data. They comprise the following types.

**Filter transmission curves**: characterise the full transmission in the range [0, 1], including the
    telescope optic, the filter itself and the detector efficiency.

**Spectral Energy Distribution (SED)**: in erg/s/cm\ :sup:`2`/|AA| which are to be integrated through the
    filters to compute the modelled photometric values.

**Reddening Curve**: providing the :math:`k_{(\lambda)}` values required to compute the ``internal``
    absorption caused by the interstellar matter in the galaxy.

The input files corresponding to these three types of data must be formatted as ASCII tables, with |AA| wavelengths
in the first column and specific values in the second one.

CLI: Managing Auxiliary Data
----------------------------

In order to make auxiliary data available to data analysis, they first have to be imported inside the Phosphoros directory structure.

The most convenient way to do this is to download a tar file containing already sets of Filters, SEDs and Reddening curves and to expand it at the correct location. i.e.::

    > cd ~
    > cd Phosphoros (or cd $PHOSPHOROS_ROOT)
    > wget http://www.isdc.unige.ch/phosphoros/data/auxiliary/Challenge2AuxiliaryData.tar.gz
    > tar -xzf Challenge2AuxiliaryData.tar.gz

Files are arranged inside three directories ``Filters``, ``SEDs`` and ``ReddeningCurves`` below ``$HOME/Phosphoros/AuxiliaryData``.
Optional ``group`` sub-directories can be added to organize auxiliary files in the most logical way. Users can complete or re-arranged
these sub-directories to match their preferred organization scheme.

Any auxiliary files following the supported formats (REFERENCE NEEDED) can also be added and arranged using shell commands
such as ``mkdir``, ``mv``, ``cp`` or ``rm`.

GUI: ``Configuration`` : ``Aux. Data Management``
-------------------------------------------------

The Phosphoros GUI can also be used to display and managed auxiliary data files. Start the GUI with ``Phosphoros GUI`` `
and click on ``Configuration`` and ``Aux. Data` tabs. This is not yet the place for selecting filters or SEDs for a
particular analysis (for this see :ref:``Parameter Space Definition <parameter-space-definition> below).

First, the GUI provides a view of all available auxiliary data (if any) as shown on the below screen shot for the case
of the ``Filter Transmission``. Similar views can be displayed for the ``SEDs``, ``Reddening Curves``and ``Luminosity
Function Curves`` auxiliary files.

.. figure:: /_static/first_step/AuxDataManagement.png
    :align: center

Second, there are functionalities to ``Import`` new files or to ``Create (Sub)-Group``. Selecting ``Import`` opens a
new window showing how to import single file or an entire directory contents.

.. _catalog-column-mapping:
    
Mapping the catalog columns
===========================

In order to compute modelled photometry values, Phosphoros needs filter transmission curves which correspond to the filters
used to obtained the observed photometric values. In general, the names of the observed photometric bands, typically
reflected in the catalog column names may not match the names of the filter transmission files. Consequently, the mapping of
these names must be specified. The easiest is to use teh GUI although an ASCII file can also be used providing it is located
at the right place in the Phosphoros directory structure.

GUI
---

CLI how-to
----------

filter_mapping.txt explanation

.. _parameter-space-definition:
    
Defining the model parameter space
==================================

In template fitting algorithm, photometric redshifts are derived by finding the best match between the observations and a number of
precomputed model photometric values. One of the main configuration is therefore the specification of the to-be-considered
parameter space, which comprises a number of axes, such as redshift, SED and reddening law. For each of these axes, a
number of values or a range and a step have to be provided. The first step of Phosphoros is then to compute a vector of model
photometric values (one for each filter) for each cell of this space. This is then called the model photometric grid.
This calculation do not depend on the observations. It can therefore be achieved before hand, once for
all sources of the catalog.

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

This section is for computing all our photometry models based on the
``Catalog Type``, the ``Parameter Space`` and the ``Filter Section`` you
supposed to have already defined in the previous sections. |br|

.. figure:: /_static/quickstart/ModelGridSubPanel1.png
    :align: center

On the image above we have four sub-panels. For generating the models we only
focus on the first one (red frame)::

 1- Model Grid Generation / Selection (in orange)

To produce your models you should apply the following steps:

1. **IGM absorption type** |br|

 Select one of the IGM absorption type (Madau, Meiksin, Inoue or Off ) to be applied. 

2. **Model Grid File** |br|

 Give a filename for storing your models. By default, a filename is automatically
 generated. Your ``Catalog Type``, ``Paramter Space`` and the ``IGM absorption Type`` names
 are used for building the filename as in our example ``Grid_Quickstart Parameter Space_MADAU``.
 The file will be stored into the following directory::
 
 $PHOSPHOROS_ROOT/IntermediateProducts/"Catalog Type"/ModelGrids

 In our example ``Catalog Type`` will be replaced by ``Quickstart``

3. **(Re) Generate the Grid**

 Click on this button to generate your photometry models.
 
4. Get Config File (optional)

 If you click on this button, a file (with "Untitled.conf" as default filename) 
 containing your setup for computing your grid will be stored into the directory::
 
 $PHOSPHOROS_ROOT/config/Untitled.conf

Note:
 The orange color (on the image) means that we have not produced any model grid
 for our selection yet. The color will turn in black immediately after your models 
 have been created.
 Each time you see this orange color at any sub-panel it means either you forget
 to set something or the setting is not applied yet.
 
CLI
---

To produce the photometry models using the commmand line interface do the following::

Phosphoros compute_model_grid --config-file=$USER/Phosphoros/config/Untitled.conf

Show the command. Mention the default configuration file name. Explain where the
files are created (and the reasoning behind the default naming).

Computing the Redshift
======================

GUI
---

.. figure:: /_static/quickstart/InputOutputSubpanel4.png
    :align: center

We focus here on the sub-panel four (red frame).
This is the last step for computing the photometric redshift. It provides you the 
best fit catalog with the best fitted models for the catalog containing your sources.|br|

You need to define the followings:

1. **Input Catalog File**
 
 Provide a photometry catalog (FITS or ASCII format) containing your sources 
 (See format details **here**).
 
2. **Generate Output Catalog with Format**

 Select the output format of the Phosphoros best fitted catalog you wish. It is
 either ASCII or FITS format (See format details **here**).
 
3. **Generate Output PDF Files**
 
 Click the checkboxe for generating the 1D PDF(z) files, one per source 
 (See format details **here**).

4. **Generate Likelihood Files**
 
 Click the checkboxe for generating the likelihood FITS files, one per source 
 (See format details **here**).
 
5. **Generate Posterior Files**

 Click the checkboxe for generating the posterios FITS files, one per source 
 (See format details **here**).
 
6. **Output Folder**
 
 The output folder is provided by default and it is where the phosphoros result
 files will be stored. So you have the possibility to locate the outputs files
 somewhere else.

CLI
---

Show the command. Mention the default configuration file name. Show the same steps
as for the GUI.

Examining the results
=====================

Explain that there are some tools for that and that are available only at CLI.

Explain how to use the plot_specz_comparison for seeing the specz-phz plot and
the PDFs. Also mention the SAMP functionality.

