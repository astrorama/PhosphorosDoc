.. _format-reference-section:

*************************
File format
*************************

This chapter describes the format of all files Phosphoros needs to
know about. It is divided into three sections, describing the format
of input files (provided by users), of intermediate products (generated
by Phosphoros itself for internal use) and of output files (containing
Phosphoros results).

Input files
===========

.. _format-catalogs:

Catalogs
--------

Input catalogs can be either ASCII or FITS tables (Phosphoros
will auto-detect the type). They must contain the following columns:

- **ID**: The ID of the source
  
- **Filter Fluxes**: One column for each filter, containing the Flux
  in :math:`\mu`\ Jy
  
- **Filter Flux Errors**: One column for each filter, containing the Flux
  error in :math:`\mu`\ Jy

and optionally:

- **SpecZ**: The spectroscopic redshift (mandatory for training
  catalogs)

- **RA**, **DEC**: Right ascension and declination of the source, in
  degrees

- **GAL_EBV**: Galactic color excess in the source sky position, in
  magnitude

- **Filter Shifts**: One column for each filter, containing the filter
  shift in |AAm|

Phosphoros does not require any specific order or naming of the
catalog columns. In ASCII tables, the first line, starting with #,
should specify the column names.

Phosphoros internally uses strings for the IDs and double
precision floats for all the other columns. If the input catalogs
contain columns of different type, Phosphoros will automatically
perform the convertion.

..
  which can be casted to the
  internally used type, Phosphoros will perform this cast. This means
  you do not have to manually make the convertions.

.. _auxiliary_format:
  
Auxiliary Data
------------------------

..
  Many of the following input files are specific cases of the more
  generic file format of a dataset. The dataset files are ASCII, space
  separated tables, with two columns. The meaning of the columns
  changes depending on the type of the file (as explained in the
  following sections). Both columns will be parsed by Phosphoros as
  double precission decimal numbers. Scientific notation (i.e.
  0.1234e-56) is allowed.

Auxiliary data are ASCII space-separated tables, with two columns. The
meaning of columns changes depending on the data type. Unless
particular cases, both columns will be parsed by Phosphoros as double
precision decimal numbers. Scientific notation (i.e.  0.1234e-56) is
allowed.
  
A dataset file can contain any number of comments, starting with the  
symbol **#**. 

The name of an auxiliary dataset (e.g., a SED or a filter) used by
Phosphoros is by default the ``<folder name>/<file name without
extension>``. However, if the first line of the file is a one-word
comment (e.g., ``# TestName``), Phosphoros will use it (e.g.,
``<folder name>/TestName``) as the name of the dataset, in place of
the filename.

Here below a list of typical auxiliary data and their format.

- **Filter Transmission Curves**: the first column contains the
  wavelength values expressed in |AAm| and the second column the
  transmission value in the range [0,1].

  .. note::

     By default, Phosphoros assumes that filter transmission curves
     refer to photon-counting systems. In order to specify that a
     filter is for a energy-measuring system, users must insert the
     following header in the filter transmission file::

       # <Filter Name>
       # FilterType Energy
       3600 0.000242676
       3610 0.000493176
       3620 0.000953069
       ...

     The presence of ``<Filter Name>`` is mandatory for
     energy-measuring systems (but not for photon-counting
     ones). Moreover, if no keyword ``FilterType`` is found or it has
     a different value from ``Energy``, photon-counting systems will
     be considered.

- **SED Templates**: the first column contains the wavelength values
  expressed in |AAm| and the second column the flux expressed
  in erg/s/cm\ :sup:`2`/|AAm|.

  .. note::

     In their header, SED templates can also contain physical
     parameter information. As example::

       # PARAMETER : mass=0.1*L+1.3[solar mass]
       901.57              0.0000164030  
       903.65              0.0000163025  
       905.73              0.0000162024  


- **Reddening Curves**: the first column contains the wavelength
  values expressed in |AAm| and the second column the values of the
  reddening curve :math:`k(\lambda)`.

- **Luminosity Function Curves**: the first column contains the
  luminosity values in erg/s/Hz or the AB magnitude values and the
  second column the values of the galaxies number density (typically,
  but not necessarily, in :math:`{\rm Mpc}^{-3}\,({\rm
  erg/s/Hz})^{-1}` or in :math:`{\rm Mpc}^{-3}`). Note that the format
  of the file is the same regardless of using magnitude or luminosity.

..  in [:math:`{\rm Mpc}^{-3}({\rm erg/s/Hz})^{-1}`] or Mpc\ :sup:`-3`, respectively
  
..  The separation of the files is done in Phosphoros, as explained in
    the :ref:`luminosity-prior` section.

.. _axes-priors:

Axes Priors
^^^^^^^^^^^^^^^^^^

Axis priors have different format according to whether the model
parameter is a numerical or categorical variable.

- **SED and Reddening Curve Axes Priors**: here the first column is a
  string, representing the name of the SED template or the Reddening
  Curve accordingly. The second column is a double precision decimal
  number, representing the prior weight to multiply the likelihood
  with.

  .. note::

    The first column must contain the qualified name of the file as
    seen by Phosphoros (including, i.e., group information). For
    example, for a SED template in the directory::

      > $PHOSPHOROS_ROOT/AuxiliaryData/SEDs/CosmosEll

    the first column should be ``CosmosEll/<sed name>``.
    Phosphoros actions ``display_seds`` (``DS``) and
    ``display_reddening_curves`` (``DRC``) can retrieve these names,
    as explained in :ref:`Explore Auxiliary Data<explore_aux_cli>`.

  
- **E(B-V) and Redshift Axes Priors**: the first column contains
  E\ :sub:`(B-V)` or redshift values accordingly, and the second
  column contains the prior weight to multiply the likelihood with.

.. _grid-prior-format:

Multi-dimensional Priors
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Multi-dimensional generic priors are FITS files with the following
Header Data Units (HDUs), in this specific order:

**1. Primary HDU**: The primary HDU is intentionally left empty. If it
contains any data, they are ignored by Phosphoros.

**2. Prior HDU**: The prior HDU is an image extension, containing a 4
dimensional array, which keeps the prior weights for each cell of the
parameter space. It must have the following characteristics:

* **extension name** : it can be any string, which is used for identifying the
  parameter space region in sparse grids (see bellow) 
* **array type** : double precision floating point (BITPIX=-64)
* **first axis** : represents redshift
* **second axis** : represents E\ :sub:`(B-V)`
* **third axis** : represents reddening curve
* **fourth axis** : represents SED

**3. Redshift HDU**: the redshift HDU is a binary table extension, which
keeps the values of the redshift axis knots. It must have the
following characteristics:

* **extension name** : ``Z_region``, where ``region`` is the name of
  the related prior HDU
* **length** : The same as the first axis in the related prior HDU
* **first column** :
    * Name : Index
    * Type : 32-bit integer (TFORM=J)
* **second column** :
    * Name : Value
    * Type : double precision floating point (TFORM=D)

**4. E(B-V) HDU**: the E\ :sub:`(B-V)` HDU is a binary table
extension, which keeps the values of the E\ :sub:`(B-V)` axis
knots. It must have the following characteristics:

* **extension name** : ``E(B-V)_region``, where ``region`` is the name
  of the related prior HDU
* **length** : The same as the second axis in the related prior HDU
* **first column** :
    * Name : Index
    * Type : 32-bit integer (TFORM=J)
* **second column** :
    * Name : Value
    * Type : double precision floating point (TFORM=D)

**5. Reddening Curve HDU**: the Reddening Curve HDU is a binary table
extension, which keeps the values of the reddening curve axis
knots. It must have the following characteristics:

* **extension name** : ``Reddening Curve_region``, where ``region`` is
  the name of the related prior HDU
* **length** : The same as the third axis in the related prior HDU
* **first column** :
    * Name : Index
    * Type : 32-bit integer (TFORM=J)
* **second column** :
    * Name : Value
    * Type : string (TFORM=*A, where * the max length)

**6. SED HDU**: the Sed HDU is a binary table extension, which keeps the
values of the SED axis knots. It must have the following
characteristics:

- **extension name** : ``SED_region``, where ``region`` is the name of
  the related prior HDU
- **length** : The same as the fourth axis in the related prior HDU
- **first column** :
    - Name : Index
    - Type : 32-bit integer (TFORM=J)
- **second column** :
    - Name : Value
    - Type : string (TFORM=*A, where * the max length)
    
**7. Sparse Grids HDUs**: to create priors for sparse grids, the set of
prior HDU and axes HDUs have to be repeated as many times
as the number of regions in the sparse grid.


.. tip::
    
    Do not try to create files of this complex format from
    scratch!  Phosphoros provides the tool ``create_flat_grid_prior``
    (``CFGP``) that will generate a flat prior FITS file based on
    the parameter space of a model grid file (for more info see
    :ref:`multi_dim_generic_prior`).
    

.. _output_files_format: 

Intermediate Products
=========================

In the standard directory organization of Phosphoros, all intermediate
products are stored in the directory (or in sub-directories of)::

  > $PHOSPHOROS_ROOT/IntermediateProducts/<Catalog Type>


Model Photometry Grid
-------------------------------------------

Due to the size, the file containing the grid of modeled photometry is
typically stored in an internal Phosphoros format. Access from the C++
language can be done by using the Phosphoros ``PhzDataModel``
module. Access outside C++ can be performed with the Phosphoros action
``display_model_grid`` (``DMG``). For more information see the
:ref:`investigate-model-grids` section.

Users can also store the model grid file in ASCII using the CLI, by
setting the following option of the ``compute_model_grid`` (``CMG``)
action as::

  --output-model-grid-format=TEXT

By default, the file is named as ``Grid_<Catalog Type>_<parameter
space name>_<IGM prescription>.dat`` (e.g.,
``Grid_Challenge2_Parameter_Space_MADAU.dat``) and stored in the
``IntermediateProducts/<Catalog Type>/ModelGrids`` directory. A
different name can however be chosen with the GUI (see
:ref:`generating-model-grid`) or with the CLI (using the
``--output-model-grid`` option)

.. _zeropoint-format:

Photometric Zero Point Corrections
----------------------------------------------

This file is an ASCII table with two columns. The first column is the
qualified name of filters (including the group information) and the
second one is the photometric correction value.

By default, the file is named as ``<Catalog Type>_<parameter space
name>_<average method>.txt`` (e.g.,
``Challenge2_Parameter_Space_WEIGHTED_MEDIAN.txt``) and stored in the
``IntermediateProducts/<Catalog Type>`` directory.

.. note::

   The corrections are on the source flux and not on the magnitude,
   meaning that the flux of each filter will be multiplied with the
   provided value.


.. _filter-mapping:   
   
Filter Mapping
-----------------------------------

The ``filter_mapping.txt`` file is an ASCII file used to map filter
trasmission curve files to catalog column names. It is located in the
following directory::

  > $PHOSPHOROS_ROOT/IntermediateProducts/<Catalog Type>/

This file looks like::

    # Filter, Flux Column, Error Column, Upper Limit/error ratio, Convert from MAG, Filter Shift Column
    DECAM/g FLUX_G FLUXERR_G 3 0 NONE
    DECAM/i FLUX_I FLUXERR_I 3 0 NONE
    DECAM/r FLUX_R FLUXERR_R 3 0 NONE
    DECAM/z FLUX_Z FLUXERR_Z 3 0 NONE
    EUCLID_DC1/vis FLUX_VIS FLUXERR_VIS 3 0 NONE
    vista/H FLUX_H FLUXERR_H 3 0 NONE
    vista/J FLUX_J FLUXERR_J 3 0 NONE
    vista/Y FLUX_Y FLUXERR_Y 3 0 NONE

and includes 6 columns:

- Column 1, ``Filter``: The qualified name of the file containing the
  filter transmission curve (i.e., the directory name below the
  ``AuxiliaryData/Filters`` directory plus the filter name)
- Column 2, ``Flux Column``: The catalog flux column name
  corresponding to the filter
- Column 3, ``Error Column``: The catalog flux error column name
  corresponding to the filter
- Column 4, ``Upper Limit/error ratio``: The number used to recompute
  flux errors if ``Upper Limit recompute error flag`` is equal to,
  e.g., ``-99`` (see :ref:`GUI: Mapping filters to column names<mapping>`)
- Column 5, ``Convert from MAG``: ``0`` if photometry are provided in
  fluxes, ``1`` in AB magnitude
- Column 6, ``Filter Shift Column``: The name of the catalog column
  containing the filter shift (if ``NONE``, filter variation
  correction is not applied to the filter)

The ``error_adjustment_param.txt`` file is found in the
same directory and looks like::

    DECAM/g 1  0  0
    DECAM/i 1  0  0
    DECAM/r 1  0  0
    DECAM/z 1  0  0
    EUCLID_DC1/vis 1  0  0
    vista/H 1  0  0
    vista/J 1  0  0
    vista/Y 1  0  0

where Column 1 is the qualified name of the file containing the filter
transmission curve, and Columns 2,3,4 are the values of the
coefficients :math:`\alpha_k`, :math:`\beta_k`, :math:`\gamma_k` used to
re-calibrate flux errors (see Eq. :eq:`eq_err_cal`).
  
The files are automatically generated by the GUI at the ``Catalog
Setup`` step. Otherwise, users have to create them at the right place.

Other Products
--------------------------------

Phosphoros generates other intermediate products when luminosity
priors, filter variation correction and Galactic absorption correction
are applied. They are the *luminosity model grid*, the *filter
variation correction grid* and the *correction coefficients grid* and are
located, respectively, at the directories::

  > IntermediateProducts/<Catalog Type>/LuminosityModelGrids/
  > IntermediateProducts/<Catalog Type>/FilterVariationCoefficientGrids/
  > IntermediateProducts/<Catalog Type>/GalacticCorrectionCoefficientGrids/
  
Both files are stored by default in binary format, accessible only by the
Phosphoros C++ executables. They can also be stored in ASCII format
using the CLI, as follows:

- in the ``compute_luminosity_model_grid`` (or ``CLMG``) action, by
  setting the option ``--output-model-grid-format=TEXT``

- in the ``Compute Filter Variation Coefficient Grid`` (or ``CFVCG``)
  action, by setting the option
  ``--output-filter-variation-coefficient-grid-format=TEXT``
  
- in the ``compute_galactic_correction_coeff_grid`` (or ``CGCCG``)
  action, by setting the option
  ``--output-galactic-correction-coefficient-grid-format=TEXT``.

.. _result_files_format: 

Results
==============

In the standard directory organization, all Phosphoros outputs
are stored in the directory::

  > $PHOSPHOROS_ROOT/Results/<Catalog Type>/<input catalog name>/

where the name of the input catalog is without the extention.


Output Catalogs
-----------------------

Output catalogs can be stored either in FITS or in ASCII format. The
default name is ``phz_cat``, with the extension according to the
format.

In the basic case (i.e., without saving the best model or
the 1D PDFs), output catalogs contain the following columns

- **ID**: the source ID

- **Z**: the best-estimate of redshift (in this case it coincides with the
  1DPDF-Peak-Z value)

- **Posterior-Log**: the amplitude of the posterior distribution at
  the maximum

- **Likelihood-Log**: the amplitude of the likelihood at the maximum

- **1DPDF-Peak-Z**: the redshift at the maximum of the 1D redshift PDF

If ``Best posterior model`` is enabled in the GUI (or
``--create-output-best-model=YES`` in the ``compute_redshift`` action
of the CLI), these columns are added:

- **SED**, **ReddeningCurve**, **E(B-V)** and **Z**: they are the
  values corresponding to the maximum of the posterior
  distribution.

- **SED-Index**: this is the index of the best-model SED template
  inside the group the SED belongs to.

- **Scale**: the normalized scale factor :math:`\alpha` associated with
  the best model.

If ``Best likelihood model`` is enabled (or
``--create-output-best-likelihood-model=YES``), the columns have the
same names as those above except that they start with ``LIKELIHOOD-``
(e.g., ``LIKELIHOOD-SED``).

.. note::

   If enabled, output catalogs also contain the physical parameter
   values as estimated from the best-fit model, one column for each
   physical parameter.


Marginalized 1D PDFs
-------------------------

The marginalized 1D PDFs can be either generated as part of output
catalogs or as an individual file.

If they are generated as a catalog column in ASCII format, they are a
list of comma separated values. If they are generated in FITS format,
they are vector columns. In both cases, the axis bins are given as
part of the comments of the file.

If the 1D PDFs are generated as an individual file, they are FITS files
containing binary table HDUs with two columns, the first of which
represents the axis parameter (e.g., redshift) and the second the
probability. The name of each HDU is the ID of the corresponding
source and it can be used for searching the 1D PDFs. Moreover,
the order of the HDUs matches the order of the sources in the input
catalog (starting from the first extension HDU).

Multi-dimensional Posterior Distribution
-----------------------------------------------

Multi-dimensional posterior outputs depend on the choice of users to
save the full grid or a sampling of the posterior distribution. All the
multi-dimensional outputs are stored in the directory::

  > $PHOSPHOROS_ROOT/Results/<Catalog Type>/<input catalog name>/posteriors/

**a)** If ``Full grid`` is selected in the GUI (or
``--full-PDF-sampling=NO`` in the ``compute_redshift`` action),
Phosphoros produces one FITS file for each source of the catalog,
containing the multi-dimensional posterior distribution. The name of
the file is the ID of the source, with the extension *fits*. It
contains the following HDUs:

- **Primary**: a 4-dimensional array containing the likelihood or
  posterior distribution (order of axes: Z, E\ :sub:`(B-V)`, RedCurve,
  SED)
- **Z**: a single column binary table with the values of the Z axis
- **E(B-V)**: a single column binary table with the values of the
  E(B-V) axis
- **Reddening Curve**: a single column binary table with the values of
  the Reddening Curve axis
- **SED**: a single column binary table with the values of the SED axis

.. note::

   Phosphoros provides a tool for visualising files of this type, as
   explained in the :ref:`posterior-investigation` section.


**b)** If ``Sampling`` is selected in the GUI (or
``--full-PDF-sampling=YES`` in the ``compute_redshift`` action),
Phosphoros saves only a sampling of the model parameters. Multiple
FITS files are produced, each with the results of at the most ten
thousand sources (this number can be modified using the CLI). The name
of output files is ``Sample_File_posterior_<n>.fits``, where ``<n>`` is
the file index. The output files have the following columns:
**OBJECT_ID**, **GRID_REGION_INDEX** [#f1]_, **SED_INDEX**,
**REDSHIFT**, **RED_CURVE_INDEX** and **EB_V**.  In addition,
Phosphoros creates a FITS file (``Index_File_posterior.fits``)
containing the object IDs and the file names where the corresponding
outputs are stored.

.. note::

   If enabled, the ``Sample_File_posterior_<n>.fits`` files contain
   additional columns with the value of physical parameters and of the
   model luminosity at 10pc (in the filter used for the SED
   normalization).
  

.. rubric :: Footnotes

.. [#f1] **GRID_REGION_INDEX** is the 0-based index of sub-spaces in
	  the parameter space (e.g., ``Elliptical``, ``Spiral``,
	  etc.). The index follows the order in which the sub-spaces
	  have been defined in the GUI or in the CLI. This information
	  is useful, for example, to display the photometry of
	  best-fit models through the ``display_model_grid`` action
	  (see :ref:`investigate-model-grids`).
