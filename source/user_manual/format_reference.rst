.. _format-reference-section:

*********************
File format reference
*********************

This section describes the format of all files Phosphoros needs to know about.
It is devided to two sub-sections, one describing the format of the input files
(files provided by the user) and the output files (files produced by Phosphoros
itself).

Input files
===========

Catalogs
--------

The Phosphoros input catalogs can be either ASCII or FITS tables (Phosphoros
will try to auto-detect the type). They must contain the following columns:

- **ID** : The ID of the source
- **Filter Fluxes** : One column for each filter, containing the Flux in |mu|\ Jy
- **Filter Errors** : One column for each filter, containing the Flux error
- **SpecZ** : The spectroscopic redshift (only for training catalogs)

Phosphoros provides flexible description of the catalog columns, so it does not
require specific order or naming of the catalog columns. For a detailed
description of how to define the catalog columns see :ref:`catalog-column-mapping`.

Phosphoros internally uses 64 bit integers for the IDs and double precision
floats for all the other columns. If the input catalogs contain columns of
different type, which can be casted to the internally used type, Phosphoros will
perform this cast. This means you do not have to maually make the convertions.

Dataset files
-------------

Many of the following input files are specific cases of the more generic file
format of a dataset. The dataset files are ASCII, space separated tables, with
two columns. The meaning of the columns changes depending on the type of the
file (as explained in the following sections). Both columns will be parsed by
Phosphoros as double precission decimal numbers. Scientific notation (i.e.
0.1234e-56) is allowed.

A dataset file can contain any number of comments, starting with the symbol
**#**. If the first line of the file is a one-word comment, it is going to be
used as the name of the dataset, in place of the filename.

Filter Transmissions
--------------------

The filter transmissions are dataset files, where the first column contains the
wavelength values expressed in |AA| and the second column the transmissoin value
in the range [0,1].

SED Templates
-------------

The SED templates are dataset files, where the first column contains the
wavelength values expressed in |AA| and the second column the Flux density,
expressed in erg/s/cm\ :sup:`2`/|AA|.

Reddening Curves
----------------

The Reddening Curves are dataset files, where the first column contains the
wavelength values expressed in |AA| and the second column the values of the
:math:`k_{(\lambda)}`, which will used for applying the extinction.

Luminosity Function Curves
--------------------------

The Luminosity Function Curves are dataset files where the first column contains
the Luminosity values, either expressed in AB magnitude or in flux (erg/s/Hz),
and the second column the values of the density (1/Mpc\ :sup:`3`). Note that the
format of the file for the two different types (magnitude and flux) is the same.
The separation of the files is done in Phosphoros, as explained in the
:ref:`luminosity-prior` section.

Axes Priors
-----------

SED and Reddening Curve Axes Priors
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The SED and Reddening Curve Axes Prior files are ASCII, space separated tables,
with two columns (note that these files are not datasets). The first column is a
string, representing the name of the SED or Reddening Curve accordingly. The
second column is a double precission decimal number, representing the prior
weight to multiply the likelihood with.

.. warning::
    
    The names of the first column have to be the same names as Phosphoros sees
    the datasets, which might be different than the file names. You can use the
    Phosphoros actions `display_seds` (`DS`) and `display_reddening_curves`
    (`DRC`) to retrieve these names, as explained in the :ref:`explore_aux_cli`
    section.

E\ :sub:`(B-V)` and Redshift Axes Priors
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The E\ :sub:`(B-V)` and Redshift Axes Priors are dataset files, where the first
column contains the E\ :sub:`(B-V)` or Redshift value accordingly, and the
second column contains the prior weight to multiply the likelihood with.

Multi-dimensional Generic Priors
--------------------------------

The multi-dimensional generic priors are FITS files with the following HDUs, in
this specific order:

.. tip::
    
    Do not try to create files of this complex format from the scratch!
    Phosphoros provides the tool `create_flat_grid_prior` (`CFGP`) which will
    generate a flat prior FITS file, based on the parameter space of a model
    photometry grid file (for more info see :ref:`multi_dim_generic_prior`)
    
Primary HDU
^^^^^^^^^^^

The primary HDU is intentionally left empty. If it contains any data, they
are ignored by Phosphoros.

Prior HDU
^^^^^^^^^

The prior HDU is an image extension, containing a 4 dimensional array, which
keeps the prior weights for each cell of the parameter space. It must have the
following characteristics:

* **extension name** : it can be any string, which is used for identifying the
  region when using sparse grids (more about this bellow) 
* **array type** : double precision floating point (BITPIX=-64)
* **first axis** : represents the redshift
* **second axis** : represents the E\ :sub:`(B-V)`
* **third axis** : represents the reddening curve
* **fourth axis** : represents the SED

Redshift HDU
^^^^^^^^^^^^

The redshift HDU is a binary table extension, which keeps the values of the
redshift axis knots. It must have the following characteristics:

* **extension name** : *Z_region*, where region is the name of the related prior
  HDU
* **length** : The same as the first axis for the related prior HDU
* **first column** :
    * Name : Index
    * Type : 32-bit integer (TFORM=J)
* **second column** :
    * Name : Value
    * Type : double precision floating point (TFORM=D)

E\ :sub:`(B-V)` HDU
^^^^^^^^^^^^^^^^^^^

The E\ :sub:`(B-V)` HDU is a binary table extension, which keeps the values of
the E\ :sub:`(B-V)` axis knots. It must have the following characteristics:

* **extension name** : *E(B-V)_region*, where region is the name of the related
  prior HDU
* **length** : The same as the second axis for the related prior HDU
* **first column** :
    * Name : Index
    * Type : 32-bit integer (TFORM=J)
* **second column** :
    * Name : Value
    * Type : double precision floating point (TFORM=D)

Reddening Curves HDU
^^^^^^^^^^^^^^^^^^^^

The Reddening Curves HDU is a binary table extension, which keeps the values of
the Reddening Curves axis knots. It must have the following characteristics:

* **extension name** : *Reddening Curve_region*, where region is the name of the
  related prior HDU
* **length** : The same as the third axis for the related prior HDU
* **first column** :
    * Name : Index
    * Type : 32-bit integer (TFORM=J)
* **second column** :
    * Name : Value
    * Type : string (TFORM=*A, where * the max length)

SED HDU
^^^^^^^

The Sed HDU is a binary table extension, which keeps the values of the SED axis
knots. It must have the following characteristics:

- **extension name** : *SED_region*, where region is the name of the related
  prior HDU
- **length** : The same as the fourth axis for the related prior HDU
- **first column** :
    - Name : Index
    - Type : 32-bit integer (TFORM=J)
- **second column** :
    - Name : Value
    - Type : string (TFORM=*A, where * the max length)
    
Sparse Grids HDUs
^^^^^^^^^^^^^^^^^

To create priors for sparse grids, the set of prior HDU together with the axes
HDUS can be repeated as many times, as regions in the sparse grid.

.. _emission-line-tables:

Emission Line tables
--------------------

The emission lines table is an ASCII table with the following columns:

- **Line name** : The name of the emission line
- **Wavelength** : The central wavelength of the line
- **Relative Flux 1** : The flux of the line, relative to the H\ |beta| flux
- **Relative Flux 2** : The flux of the line, relative to the H\ |beta| flux
- ...

The table can have any number of relative flux columns, each one containing the
relative fluxes for different metalicity values.

.. tip::
    
    If you also want to add the H\ |beta| line, you need to add a row, with all
    relative fluxes set to 1.

All values of the table (except of the line names) are parsed as double
precission decimal numbers. Scientific notation (i.e. 0.1234e-56) is allowed.
Note that Phosphoros accesses this table only by index, so the names of the
columns in the file are ignored.
    
.. _metal-to-phot-table:

Metalicity to Ionized Photons table
-----------------------------------

The Metalicity to Ionized Photons table is an ASCII table with the following
columns:
    
- **Z/Z0** : The metalicity Z (in solar units)
- **log(Qh/L1500)** : The logarithm of the number of ionized photons normalized
  by the luminosity at 1500 |AA|

All values of the table are parsed as double precission decimal numbers.
Scientific notation (i.e. 0.1234e-56) is allowed. Note that Phosphoros accesses
this table only by index, so the names of the columns in the file are ignored.

Output files
============

Model Photometry Grid
---------------------

Due to file size, the model photometry grid file is stored in an internal
Phosphoros format. Access from the C++ language can be done by using the
Phosphoros *PhzDataModel* module. Access outside C++ can be performed with the
Phosphoros action `display_model_grid` (`DMG`). For more info see the section
:ref:`investigate-model-grids`.

Photometric Zero Point Corrections
----------------------------------

This file is an ASCII table with two columns. The first column is the filter
fully qualified name (including the group information) and the second one is
the photometric correction value. Note that the corrections are Flux corrections
and not magnitude, meaning that the Flux of each filter will be multiplied with
the provided value.

PDF\ :sub:`(Z)`
---------------

This is a FITS file which contains the PDF\ :sub:`(Z)` for all sources. Each
PDF\ :sub:`(Z)` is a binary table HDU with two columns, the first of which
represents the redshift and the second the probability. The name of each HDU is
the ID of the corresponding source and it can be used for searching the PDFs.
Alternativelly, the order of the HDUs matches the order of the sources in the
input catalog (starting from the first extension HDU).

Multi-dimensional Likelihood and Posterior
------------------------------------------

Phosphoros (when any of these outputs is enabled) produces one FITS file for
each source of the catalog. The name of the file is the ID of the source, with
the extension *fits*. It contains the following HDUs:

- Primary : A 4-dimensional array containing the likelihood (order of axes:
  Z, E\ :sub:`(B-V)`, RedCurve, SED)
- Z : A single column binary table with the values of the Z axis
- E(B-V) : A single column binary table with the values of the E(B-V) axis
- Reddening Curve : A single column binary table with the values of the Reddening Curve axis
- SED : A single column binary table with the values of the SED axis

Note that Phosphoros provides a tool for visualising files of this type, as
explained in the :ref:`posterior-investigation` section.