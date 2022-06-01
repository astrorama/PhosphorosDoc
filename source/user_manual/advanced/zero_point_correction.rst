.. _zero-point-correction:

Photometric Zero-Point Corrections
=====================================

Photometric zero-point correction aims to correct flux measurements by
band-specific photometric offsets, which can lead to systematic shifts
in the observed colors, and consequently in the redshift
estimates. This correction takes into account remaining zero-point
calibration issues in one or more bands, and also potential mismatches
between galaxy colours and the templates used to model them. 

In order to derive zero-point corrections, Phosphoros follows the
method implemented in the *Le Phare* code. It needs a training
sample of the input catalog with (spectroscopic) redshift
measurements. The process to derive the zero-point corrections is
iterative. For each source of the training sample, Phosphoros
determines the best model (with respect to the observed fluxes) fixing
the redshift, and estimates in each band the offset in terms of the
ratio between the predicted and observed flux,
:math:`r=f_m^i/f^i_{obs}`. The overall offset of each band is then
computed by *averaging* over all the sample sources. Offsets are
finally used to correct photometric measurements
(:math:`f^i_{obs}\rightarrow \langle r\rangle f^i_{obs}`). The process
is repeated until reaching a tolerance threshold for the discrepancies
or a maximum number of iterations.

The *averaging* methods to compute the band overall offsets are the
following: mean, weighted mean, median (default option) and weighted
median. The weighted mean/median use the signal-to-noise value in the
band as weight.


Photometric Corrections in the GUI
------------------------------------------------

Zero-point corrections can be computed in the sub-panel
``4. Photometric Zero-Point Corrections`` of the ``Compute Redshifts``
window.

Select the ``Enable Photometric Zero-point Corrections`` tab.

Users can compute new zero-point corrections by clicking on the
``Compute New Corrections`` button. A pop-up window will appear, as
shown in :numref:`zpc`.

.. figure:: /_static/advanced_steps/zero_point_v13.png
    :name: zpc
    :align: center 
    :width: 800px
    :height: 400px
	     
    Computing zero-point correction with the GUI

The top of the window displays the configuration (catalog type,
parameter space, filters, etc.)  that will be taken into account for
the zero-point corrections. Three further steps are required:

- ``1. Traning Catalog Selection:`` select the training sample
  through the ``Browse`` tab, along with the column name where the
  spectroscopic redshifts are stored.

  The Milky Way absorption correction will be applied to the training
  sample following the option selected in the ``1. Luminosity Filter
  and Extrinsic Absorption`` sub-panel. If the ``Use Galactic E(B-V)
  Column`` option is selected, the ``E(B-V)`` column must be present
  in the training sample; if ``Look-up Galactic E(B-V) in Planck Dust
  Map`` is choosen, the column ``PLANCK_GAL_EBV`` is computed and
  added to the training sample (or to a new file according to the
  user's choice).

- ``2. Algorithm:`` define the algorithm parameters (i.e.  the
  ``Number of Iterations`` and the ``Tolerance`` thresold, used to
  decide when algorithm interations are stopped) and the ``Selection
  Method`` (i.e. the method to compute the overall band corrections).

- ``3. Run:``, choose the file to export zero-point corrections. The
  recommended (default) name is ``<Parameter Space>_<Method>``. The
  file is in ASCII format, with the ``.txt`` extension. In the
  standard configuration, it is stored in the directory::

    > $PHOSPHOROS_ROOT/IntermediateProducts/<Catalog Type>/
  
Otherwise, users can select already existing files with zero-point
correction values through the drop down menu below ``Selecting
Exisiting Compatible Corrections``, and visualize or **modify** them
through the ``View / Edit Corrections`` button.


  
Photometric Corrections in the CLI
------------------------------------------------

Zero-point corrections can be computed using the Phosphoros action
``compute_photometric_corrections`` (or ``CPC``).

Action parameters can be passed with a configuration file through the
``--config-file`` action parameter. If not specified, Phosphoros reads
by default the following configuration file::

  /path_to_PhosphorosCore_installation_directory/conf/PhzExecutables/PhosphorosComputePhotometricCorrections.conf

Configuration files for this action can be generated through the
Phosphoros GUI using the ``Save Config. File`` button present in the
``4. Photometric Zero-Point Corrections`` sub-panel (see :numref:`zpc`).
  
The main parameters of this action concern the training sample and the
algorithm configuration parameter. An example of configuration file is
following::

  catalog-type=<name>
  input-catalog-file=<file name>
  output-phot-corr-file=<output file name>

  normalization-filter=COSMOS/B_Subaru 
  normalization-solar-sed=solar_spectrum 

  source-id-column-name=<column name> # or source-id-column-index=<number>
  spec-z-column-name=<column name> # or spec-z-column-index=<number>
  spec-z-err-column-name=<column name> # or spec-z-err-column-index=<number>

  model-grid-file=<file name>
  galactic-correction-coefficient-grid-file=<file name>
  dust-column-density-column-name=<column name>
  
  phot-corr-iter-no=<value>
  phot-corr-tolerance=<value>
  phot-corr-selection-method=<value>

where the training sample (``input-catalog-file``) is searched below
``Catalogs/<Catalog Type>`` and the photometric correction output file
(``output-phot-corr-file``) is stored in
``IntermediateProducts/<Catalog Type>``.  The recommended output name
is ``<Parameter Space>_<Method>`` with the ``.txt`` extension (by
default is ``photometric_corrections.txt``). The model grid file must
be the same used for the redshift computation. It is expected to be
found in the directory ``IntermediateProducts/<Catalog Type>/ModelGrids/``.

The SED normalization parameters (see :ref:`SED Normalization
<scale-factor>`), the column names/indices of the source ID and of the
spectroscopic redshift are required by the action. On the contrary,
spectroscopic redshift errors are optional and if missing, they are
set to zero.

In order to include the Milky Way absorption correction, the filename
of the correction coefficients grid and the column name
containing the :math:`E_{B-V}^{MW}` color excess must be provided, as
shown above (see :ref:`galactic-absorption-CLI` for more details).

The default values for the algorithm parameters are: 5 for the number
of iterations ``phot-corr-iter-no``; :math:`10^{-3}` for the
tollerance threshold ``phot-corr-tolerance``; ``MEDIAN`` for the
`averaging` method.

.. warning::
   
   The ``CPC`` action has many more options, most of them present also
   in the ``compute_redshift`` action (see :ref:`compute-redshift-cli`
   and its advanced functionalities). All these options should match
   the ones used later for the redshift estimate (by the
   ``compute_redshift`` action) otherwise the zero-point correction
   would be meaningless.
