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

.. _cli-actions-list:

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

   Displays the SED data for a template in the library |br|
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

.. _build_template_options:

Build Templates options
=======================

.. program:: PhosphorosBuildTemplates


**The configuration options are separated in the following sections:**

- :ref:`config-section-BT-output`: Contains the parameters related with the output of the program

- :ref:`config-section-BT-model`: Contains the parameters of the templates to calculate the photometry for
 
- :ref:`config-section-BT-photometry`: Contains the parameters related with the photometry to be calculated

- :ref:`config-section-BT-algo`: Contains options for controlling the algorithm used for the photometry calculation

- :ref:`config-section-BT-data`: Contains options related with data retrieval


.. _config-section-BT-output:

Output Parameters section
-------------------------


Contains options related with the output of the program.

.. option:: --output-photometry-grid 
   
   The file to export in binary format the grid containing the calculated templates photometries.
   
.. _config-section-BT-model:

Model Parameter Space section
-----------------------------

Contains options for selecting the parameter space to construct the templates
when calculating the photometry.

**SED selection options :**


.. option:: --sed-group

   Use all the SEDs in the given group and its subgroups. Can appear more than once cumulatively.
   
.. option:: --sed-name

   Use the given SED. Can appear more than once cumulatively.

.. option:: --sed-exclude  

   Exclude the given SED. Can appear more than once cumulatively. 

**Reddening Curve selection options:**

.. option:: --reddening-curve-group

   Use all the Reddening Curves in the given group and subgroups. Can appear more than once cumulatively.
   
.. option:: --reddening-curve-name

   Use the given Reddening Curve. Can appear more than once cumulatively.
   
.. option:: --reddening-curve-exclude

   Exclude the given Reddening Curve. Can appear more than once cumulatively.

**E(B-V) selection options:**

.. option:: --ebv-range

   Use the E(B-V) values in a given range, sampled with the given step. It consists 
   of three space separated values, representing the min, the max and the (linear) 
   step. Can appear more than once cumulatively, but overlapping ranges are not allowed.
   
.. option:: --ebv-value

   User the given E(B-V) values. Can appear more than once cumulatively.

**Z selection options:**

.. option:: --z-range

   Use the redshift values in a given range, sampled with the given step. It
   consists of three space separated values, representing the min, the max
   and the (linear) step. Can appear more than once cumulatively, but
   overlapping ranges are not allowed.
   
.. option:: --z-value

   User the given redshift value. Can appear more than once cumulatively.

.. _config-section-BT-photometry:

Photometry section
------------------

Contains options for configuring the photometry calculated for each model.

**Filter selection options (Filters for which photometry will be calculated):**

.. option:: --filter-group

   Calculate photometry for all the Filters in the given group and subgroups.
   Can appear more than once cumulatively.
   
.. option:: --filter-name

   Calculate photometry for the given Filter. Can appear more than once
   cumulatively.
   
.. option:: --filter-exclude

   Exclude the given Filter. Can appear more than once cumulatively.

.. _config-section-BT-algo:

Algorithm Configuration section
-------------------------------

Contains options for configuring the algorithm for calculating the photometry
of each model.

.. option:: --igm-absorption-type

   The algorithm used for calculating the IGM absorption. One of OFF, MADAU
  
.. _config-section-BT-data: 
   
Data Management section
-----------------------

.. option:: --sed-root-path (required)

   The directory containing the SED datasets, organized in folders

.. option:: --reddening-curve-root-path (required)

   The directory containing the Reddening Curve datasets, organized in folders

.. option:: --filter-root-path (required)

   The directory containing the Filter datasets, organized in folders 
   

.. _derive_zero_points_options:
   
Derive Zero Points Options
==========================

.. program:: PhosphorosDeriveZeroPoints
   
**The configuration options are separated in the following sections:**

- :ref:`config-section-DZP-output`: Contains the parameters related with the output of the program

- :ref:`config-section-DZP-training`: Contains the parameters related with the training catalog

- :ref:`config-section-DZP-photometry`: Contains options related with the templates photometries (simulated models)

- :ref:`config-section-DZP-algo`: Contains options for controlling the algorithm used for the photometric correction calculation
 
.. _config-section-DZP-output:

Output Parameters section
-------------------------

Contains options related with the output of the program.

.. option:: --output-phot-corr-file

   The file to export the calculated photometric correction in ASCII format


.. _config-section-DZP-training:

Training catalog section
-------------------------

Contains the parameters related with the training catalog. This catalog contains photometry and spectroscopic redshift information.

**Generic catalog options:**

.. option:: --input-catalog-file

   The file containing the input training catalog
   
.. option:: --input-catalog-format

   The format of the input catalog. One of FITS or ASCII. It is optional and defaults to automatic detection.
   
.. option:: --source-id-column-name

   The name of the column representing the source ID
   
.. option:: --source-id-column-index

   The index (1-based) of the column representing the source ID
   
Note: The source-id-column-name and source-id-column-index are mutually exclusive and if are both missing they default to the column with name "ID".

**Photometry options:**

.. option:: --filter-name-mapping

   Defines the columns containing the photometric information for a specific
   filter. It follows the format: "filter-name-mapping  =  FILTER_NAME  FLUX_COLUMN_NAME  ERROR_COLUMN_NAME"
   This option must be repeated once for each filter.

**Spectroscopic redshift options:**

.. option:: --spec-z-column-name

   The name of the column containing the spectroscopic redshift
   
.. option:: --spec-z-column-index

   The index (1-based) of the column containing the spectroscopic redshift
   
Note: The spec-z-column-name and spec-z-column-index are mutually exclusive

.. option:: --spec-z-err-column-name

   The name of the column containing the spectroscopic redshift error
   
.. option:: --spec-z-err-column-index

   The index (1-based) of the column containing the spectroscopic redshift error
   
Note: the spec-z-err-column-name and spec-z-err-column-index are mutually exclusive. 
If both are missing, the error is set to zero.


.. _config-section-DZP-photometry:

Model Photometries section
--------------------------

Contains options related with the photometries of the teplates (simulated models).

.. option:: --photometry-grid-file

   The file containing the templates photometries. This file should be created with
   the :option:`Phosphoros -build_templates` action. Note that the given grid must contain
   photometries for the same filters with the input catalog.

.. _config-section-DZP-algo:

Algorithm Configuration section
-------------------------------

Contains options for configuring the algorithm for calculating the photometric correction.

.. option:: --phot-corr-tolerance

   The tolerance which if achieved (for all filters) between two iteration
   steps, the iteration stops. It defaults to 1E-3 and it must be a non
   negative number.

.. option:: --phot-corr-iter-no

   The number of iterations to perform, if the required tolerance is not
   achieved. A negative value will enable the iteration to stop only when the
   tolerance has achieved. It defaults to 5.

.. option:: --phot-corr-selection-method

   The method used for selecting the photometric correction from the optimal
   corrections of each source. It can be one of the following MEDIAN (default),
   WEIGHTED_MEDIAN, MEAN, WEIGHTED_MEAN. The weighted versions use as weight
   the observed flux devided with its error.
   
.. _fit-templates-options:

Fit Templates Options
=====================

.. program:: PhosphorosFitTemplates

The configuration options are separated in the following sections:

- :ref:`config-section-FT-output`: Contains the parameters related with the output of the program

- :ref:`config-section-FT-input`: Contains the parameters related with the input catalog

- :ref:`config-section-FT-model`: Contains options related with the photometries of the simulated models

- :ref:`config-section-FT-correction`: Contains options related with the photometric zero-point correction

- :ref:`config-section-FT-algo`: Contains options for controlling the algorithm used for the PHZ calculation

.. _config-section-FT-output:

Output Parameters section
-------------------------

Contains options related with the output of the program.

.. option:: --output-catalog-file (optional)

   The file to export the best fitted models, in ASCII format
   
.. option:: --output-pdf-file (optional)

   The file to export the 1D PDF of each source, in FITS format

.. option:: --output-likelihood-dir (optional)

   The directory in which the multi-dimensional likelihood grids for all
   sources are stored. They are stored as one file per source, following the
   naming convention "<SOURCE_ID>.fits". WARNING: The multi-dimensional
   likelihood grids can be relatively big files. Creating them for a detailed
   parameter space or for many sources might require a lot of disk space.

.. _config-section-FT-input:

Input catalog section
---------------------

Contains the parameters related with the input catalog. This catalog contains photometry information.

**Generic catalog options:**

.. option:: --input-catalog-file

   The file containing the input training catalog
   
.. option:: --input-catalog-format

   The format of the input catalog. One of FITS or ASCII. It is optional and
   defaults to automatic detection.
   
.. option:: --source-id-column-name

   The name of the column representing the source ID
   
.. option:: --source-id-column-index

   The index (1-based) of the column representing the source ID
   
Note: The source-id-column-name and source-id-column-index are mutually
exclusive and if are both missing they default to the column with name "ID".

**Photometry options:**

.. option:: --filter-name-mapping

   Defines the columns containing the photometric information for a specific
   filter. It follows the format:
   "filter-name-mapping  =  FILTER_NAME  FLUX_COLUMN_NAME  ERROR_COLUMN_NAME"
   This option must be repeated once for each filter.
 
 .. _config-section-FT-model:  
 
Templates Photometries section
------------------------------

Contains options related with the photometries of the simulated models.

.. option:: --photometry-grid-file

   The file containing the templates photometries. This file should be created with
   the :option:`Phosphoros -build_templates` action. Note that the given grid must contain
   photometries for the same filters with the input catalog.   
 
.. _config-section-FT-correction:
    
Photometric Correction section
------------------------------

Contains options related with the photometric zero-point correction.

.. option:: --photometric-correction-file

   The file containing the photometric corrections for each filter, as created
   with the CalculatePhotometricCorrection executable. If missing, all corrections
   are assumed to be 1.

.. _config-section-FT-algo:

Algorithm Configuration section
-------------------------------

Contains options for controlling the algorithm used for the PHZ calculation.

**Marginalization options:**

.. option:: --marginalization-type

   The method used for marginalizing the multidimentional PDF to the 1D PDF.
   The available options are: |br|
   - SUM: Simple sum of the probabilities of all models of the same redshift.  
   NOTE: The SUM option assumes that all axes of the multidimentional PDF 
   contain equally distributed values |br|
   - MAX: Maximum probability of all models of the same redhsift. 
   NOTE: The MAX option does not follow beyesian statistics |br|
   - BAYESIAN: Performs integration over the numerical axes. Currently non
   numerical axes are assumed to contain equally distributed values.
   
Browse Auxiliary Data Options
=============================

.. program:: PhosphorosLsAux   
   
.. option:: --data-root-path

   The directory containing the data files, organized in folders
   
.. option:: --group

   List the contents of the given group
   
.. option:: --data

   Print the data of the given dataset
   

.. _display_template_options:   
   
Display Template Options
========================

.. program:: DisplayTemplate 

Input section
-------------

.. option:: --photometry-grid-file

   The file containing the templates photometries. This file should be created with
   the :option:`Phosphoros -build_templates` action. Note that the given grid must contain
   photometries for the same filters with the input catalog.  

Axis informations section
-------------------------

.. option:: --sed

   Show the SED axis values
   
.. option:: --redcurve

   Show the reddening curve axis values
   
.. option:: --ebv

   Show the E(B-V) axis values
   
.. option:: --z

   Show the redshift axis values
   
Single Template photometry section
----------------------------------
   
.. option:: --phot

   Show the photometry of the cell (SED,REDCURVE,EBV,Z) (zero based indices)
   
Display Dataset Options
=======================

.. program:: DisplayDataset 

Data Management section
-----------------------

.. option:: --sed-root-path (required)

   The directory containing the SED datasets, organized in folders

.. option:: --reddening-curve-root-path (required)

   The directory containing the Reddening Curve datasets, organized in folders
   
Axis informations section
-------------------------

.. option:: --sed-name

   The name of the SED.

.. option:: --reddening-curve-name

   The name of the reddening curve.
   
.. option:: --ebv-value

   The E(B-V) value.
   
.. option:: --z-value

   The redshift value

Algorithm Configuration section
-------------------------------

.. option:: --igm-absorption-type

   The algorithm used for calculating the IGM absorption. One of OFF, MADAU



Photo-z Vs Spetro-z Options
===========================

.. program:: PhosphorosVsSpecZ 

.. option:: --files, -f

   Input file(s) to read (catalog)

.. option:: --id, -i

   The name of the ID column (default: ID)
   
.. option:: --specz , -s

   The name of the spectroscopic redshift column (default: ZSPEC)

.. option:: --phz, -p

   The name of the photometric redshift column (default: Z)

.. option:: --display, -d

   Disables the plot window
 
.. _display-ikelihood-options:
 
Display Likelihood Options
==========================
 
.. program:: PhosphorosDisplayLikelihood

.. option:: --file, -f

   FITS file containing the likelihood grid
   
.. option:: --plot-type, -p

   The type of plot to show (one of Ebv, Sed, Z, Sed-Z, Ebv-Z, Sed-Ebv, Sed-Ebv-Z)

