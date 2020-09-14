.. _executing-gui-mode:

The Graphical User Interface (GUI) execution mode
=======================================================

Launching the GUI
----------------------------

The Phosphoros Graphical User Interface (GUI) provides an interactive
and user-friendly execution mode. After starting Phosphoros (see
:ref:`Download and Installation <phosphoros-install>` section), the
GUI can be launched by the command line::

   > Phosphoros GUI

Once the GUI starts, a window opens (see :numref:`bmain`). At the top of this, a
navigation menu is visible with the main steps of Phosphoros:
``Configuration``, ``Catalog Types``, ``Parameter Space``, ``Compute
Redshifts`` and ``Postprocessing``. The menu is visible at all times,
and it allows you to navigate through the different Phosphoros
panels.

.. figure:: /_static/quickstart/MainWindow.png
    :name: bmain
    :align: center
    :scale: 40%	

    Starting window of the Phosphoros GUI
	    
Configuration
----------------------------

``Configuration`` allows users to see, and eventually to change, the
default directory configuration, and to check the available auxiliary
data present in the Phosphoros database. Three sub-panel can be
opened:
   
- **General** Here the Phosphoros root path is shown, along with the
  default directories for input/output data. No changes are required
  if users keep the standard directory structure. Otherwise, the path
  to new directories can be selected through the ``Browse`` tabs.

  Users can choose the number of unit of execution (or thread) within
  a process, by clicking on the button ``Override the Maximum Number
  of Threads``. Otherwise, the maximum number of available threads is
  automatically selected.

- **Auxiliary Data** The Phosphoros GUI can be used to display
  auxiliary data files. This is not yet the place for selecting
  filters or SEDs (this is done in :ref:`Parameter Space Definition
  <parameter-space-definition>`), but it provides a view of all
  available auxiliary data (if any). As an example, the screenshot of
  :numref:`bconfig` shows a possible case for the ``SEDs``. Similarly
  auxiliary files in ``Filter Transmissions``, ``Reddening Curves``
  and ``Luminosity Function Curves`` can be displayed.

  In the ``SEDs`` panel, emission lines can be added to a group of SED
  templates by clicking on the ``Add Emission Line to SEDs`` tab
  associated with it. A new directory (named as the original with the
  postfix ``_el``) is generated with SEDs including emission lines,
  without modifying the original templates. The new directory takes
  the name of the original one plus Emission lines are added as a
  delta function to SED templates (see the :ref:`emission-lines`
  section for more details).

  .. figure:: /_static/basic_steps/AuxDataManagement.png
     :name: bconfig
     :align: center
     :scale: 70 %
	     
     Example of the ``Configuration`` panel of the GUI showing SEDs
     present in the database 
	     
  The ``Import Folder`` tab opens a *finder* window and allows users
  to import a directory with its entire content to the location of the
  selected ``Auxiliary Data`` directory.
	    
   
- **Cosmology** The ``Cosmology`` tab displays the value of the
  cosmological parameters relevant for Phosphoros and allows users to
  change them. The default value are taken from Planck 2015 results
  :cite:`Planck2015` (including lensing and external data):
  :math:`H_0=67.74` [km/s/Mpc]; :math:`\Omega_M=0.3089`;
  :math:`\Omega_{\Lambda}=0.6911`.

.. _mapping:

Catalog Setup: Mapping filters to column names
--------------------------------------------------

In order to compute modeled photometry, Phosphoros needs the
transmission curve of filters used for the observed photometry. The
name of transmission curve files has to be connected with the
corresponding photometric band of input catalogs.

The GUI provides an easy way for **mapping** trasmission curves to
catalog column names. The mapping operation is mandatory and is
achieved in the ``Catalog Setup`` panel (see :numref:`bsetup`).

First of all, on the top of the window, users have to select the
desired catalog type or to create a new one by clicking on the ``new``
or ``duplicate`` buttons. Each catalog type corresponds to a folder
in the ``Catalogs`` directory, and duplicating or creating a new type
will create a new folder.

The input catalog is selected by ``Select File and Import Columns``
(Phosphoros automatically selects a reference input file belonging to
the catalog type). Moreover, the column name providing source ID must
be entered through ``Source ID Column``: the drop down menu shows all
the column names in the input catalog.

.. figure:: /_static/basic_steps/Catalog_Type.png
    :name: bsetup
    :width: 700px
    :align: center
    :height: 350px
   
    ``Catalog Setup`` panel and the filter mapping operation in the GUI
   
The mapping operation begins by pressing ``Select Filters``: a window
opens where the filter trsmission curves in the database can be
selected. When the filter selection is completed, pressing ``Save``
closes the window and, as shown below, fills automatically the
``Filter Transmission Curve`` column. Each of the ``Flux Column Name``
and ``Error Column Name`` cells now features a drop down menu (after
clicking on the cell) which can be used to specify the appropriate
Flux and FluxError column names.

If a catalog has some sources with missing photometry (sources that
were not observed in all catalog bands), users have to check the
corresponding control (``Missing photometry flagged as:``) and provide
the value of the flag. By doing so, the program is instructed to skip
photometry having the flag value in the flux column. The corresponding
filters are then ignored in the :math:`\chi^2` calculation.

.. note::

   Missing photometry flags must be numbers. Symbolic values as NaN,
   NULL or INF are not accepted by Phosphoros.

If the catalog contains sources that are not detected in one or more
bands (i.e., the provided photometry is an upper limit of the flux and
not the nominal flux), the ``upper limit`` control has to be
checked. The user has to ensure that the catalog follows the upper
limit convention, i.e.  photometry are considered upper limit when
their error have negative values. Upper limits are taken into account
in the :math:`\chi^2` calculation, as described in the :ref:`Template
fitting method <template-fitting>` section.

Few optional fields are present in the top-right of the ``Catalog
Setup`` panel: the column names of

* source coordinates (the right ascension ``RA (Deg)`` and declination
  ``DEC (Deg)``);

* the Milky Way extinction along source line of sight (``MilkyWay
  E(B-V)``).

These information are only required if the Milky Way absorption
correction is applied (see :ref:`Galactic Absorption
<galactic-absorption-cli>` section). In particular, sources
coordinates are needed if the *Planck* Galactic dust reddening map is
used for the correction. Otherwise, if the Milky Way extintion is
provided in the input catalog, users have to fill just the ``MilkyWay
E(B-V)`` tab.

Moreover, when present in the input catalog, the column name
containing reference redshifts (e.g., spectroscopic redshifts) can be
also inserted in the ``Reference Z`` tab. This is useful for the
``Post Processing`` analysis.

The mapping process is terminated by clicking on the ``Save``
middle-frame button.  Please note that you can always add or remove
filters after a first mapping has been completed, by going back to the
``Select Filters`` option.

After saving, an ASCII file named ``filter_mapping.txt`` is created in
the following directory::

  > $PHOSPHOROS_ROOT/IntermediateProducts/<Catalog Type>/filter_mapping.txt

where, in the previous example, ``<Catalog Type>=Quickstart``. The
file is a table with the qualified name of transmission curve files,
the flux and flux error column names in the input catalog (see
:ref:`filter-mapping` in the ``File Format Reference`` chapter).

You can always edit this file to make corrections. Alternatively, you
can create it with your favorite editor (rather than using the
GUI). When launched, the GUI will automatically load any
``filter_mapping.txt`` file located in the appropriate directory,
providing it respects the proper formatting.

.. note::

   When you modify any of the GUI files using another editor, you
   always have to restart the GUI so that changes are taken into
   account.

.. note::

   The mapping operation is carried out only once for all input
   catalogs belonging to the same catalog type.

.. _parameter-space-definition:

Defining the model parameter space
-------------------------------------------

..
  In :ref:`template fitting <template-fitting>` algorithms,
  photometric redshifts are derived by finding the best match between
  observations and a number of precomputed model photometric values.

An important step in Phosphoros is the specification of the model
parameter space. Phosphoros parameters are four: redshift, restframe
SED template, intrinsic color excess :math:`E_{(B-V)}` and intrinsic
reddening law. For each of them, a grid of *values* has to be provided
by users. Phosphoros then computes, for each cell of the parameter
space, a vector of modeled photometry, one value for each filter. This is
called the **grid of models**. This calculation do not depend
on observations and it can be achieved beforehand.

Clicking on ``Parameter Space``, users can check the sets of parameter
spaces that are already present in the Phosphoros database
(``Parameter Space`` drop down menu). They can be modified,
duplicated or deleted; or a new one can be created (see :numref:`bpara`).

Here, we show how to define a new parameter space and its
specifications. This is done for a parameter space composed of three
groups of SED templates: Elliptical, Spiral and Starburst. Click on
``New`` (on the top of the window) and provide a name for it. Then you
can select ``New`` at the ``Sub-Spaces of the Parameter Space`` level,
and a new pop-up window opens, similar to that displayed below.

.. figure:: /_static/basic_steps/Parameter_Space.png
    :name: bpara
    :align: center
    :scale: 50 %
	    
    Setting a parameter space in the GUI
	    
Through this window, you have to provide a name for the sub-space
(``Elliptical``, for example) and specify the ``SED``, ``Reddening
Curve``, ``E(B-V)`` and ``Redshift`` parameters. The ``SED`` and
``Reddening Curve`` panels simply allows to select a sub-set of the
possible data available on the system. For the ``E(B-V)`` and
``Redshift`` parameters, users can enter values, as a comma-separated
list, or ranges of values (minimum, maximum value and step) through
the ``Add Range`` option (see the above picture). Make sure to
complete the full specification of the three groups before continuing
to the next section.

The operation is terminated clicking on the ``Save`` button.

.. _generating-model-grid:

Generating the model grid
---------------------------------

Previous sections described how to set up Phosphoros database. In the
``Compute Redshifts`` panel, instead, Phosphoros executables are run
in order to compute the grid of models and to estimate photometric
redshifts.

At the top of the ``Compute Redshifts`` panel, users can select
previously defined catalog types and parameter spaces to use in
the following analysis.

.. figure:: /_static/basic_steps/ModelGrid.png
    :name: bmgrid
    :width: 700px
    :align: center
    :height: 350px
   
    How to generate a grid of models in the GUI
   
The panel is organized into five successive sub-panels (see
:numref:`bmgrid`). The first two concern the model grid generation
(``1. Extrinsic Absorption`` and ``2. Grids Generation``). Sub-panels
3 and 4 (``3. Prior``, ``4. Photometric Zero-Point Corrections``) are
optional functionalities and are described in the :ref:`Advanced
Features <user-manual-advanced>` section. Finally, the fifth sub-panel
(``5. Input/Output``) sets up the input and output files.

.. note::
   
    Sub-panels title can be black, orange or red. The orange/red color
    in one of the five steps means that some actions are required
    before Phosphoros could run to compute redshifts. For example, if
    ``2. Grids Generation`` is orange, no model grids have been
    produced for the selected specification yet. After the grid
    calculation is completed, the color turns to black indicating that
    the values have been computed and stored in a file that can be
    read in the subsequent steps of the analysis. The red color of
    ``2. Grids Generation`` means that model grid and Galactic
    correction grid are incompatible with each other.

    If you change anything in the specifications, the colour turns to
    orange again reminding that a new grid must be generated before
    continuing the analysis.
    

In order to produce a grid of models users have to go through with two steps:

- **Extrinsic Absorption**

  Here, corrections for intergalactic medium (IGM) and Milky Way
  absorption can be included in the analysis. These are optional
  functionalities.

  Users can select one of the following prescriptions for the IGM
  absorption correction -- ``Madau``, ``Meiksin`` or ``Inoue`` (see
  the :ref:`Intergalactic medium absorption <igm-absorption>`
  explanation) -- or ``OFF``, if no correction will be applied.

  There are two options for Milky Way absorption correction (see the
  :ref:`Galactic absorption <galactic-absorption>` section). Galactic
  color excess :math:`E(B-V)` values can be read from the input
  catalog (select ``Use Galactic E(B-V) Column``). In this case, users
  must have provided the corresponding column name in the ``Catalog
  Setup`` panel. The second option (``Look-up Galactic E(B-V) in
  Planck Dust Map``) fetches color excess from the *Planck* reddening
  map. The column name of source coordinates must have been provided
  in the ``Catalog Setup`` panel. If the required information are not
  given, the previous options are not available to users.

  .. warning::

     In the case the color excess is read from the input catalog,
     Phosphoros assumes that those values have been derived using
     mean sequence B5 stars. If not, they should be scaled by the
     band-pass correction (see the :ref:`galactic-absorption`
     section). This operation can be only done in the |CLI| mode.
     
  .. note::

    The IGM absorption correction is applied to SED templates before
    computing modeled photometry. On the contrary, for Milky Way
    absorption, correction coefficients are applied directly to
    modeled photometry, i.e. after computing the grid of models (see
    the :ref:`Galactic absorption <galactic-absorption>` section).

	
- **Grids Generation**

  In order to generate the grid of models, users have to specify a
  filename for storing the output. By default, a filename is
  automatically generated concatenating ``Grid`` with the parameter
  space name and the selected IGM prescription (e.g.,
  ``Grid_Test_Parameter_Space_MADAU``). The output file is stored in
  the following directory::
 
    > $PHOSPHOROS_ROOT/IntermediateProducts/<Catalog Type>/ModelGrids/

  Clicking on the ``(Re)-Generate the Grid`` button generates the grid
  of models, while on ``Save Config. File`` a configuration file with
  all the command line options needed to generate the grid of models
  with the |CLI|.

  If the Milky Way absorption correction has been selected in the
  previous step, the grid of correction coefficients has to be
  generated using the corresponding ``(Re)-Generate the Grid``
  button. The coefficients grid file is stored in the directory::

   > $PHOSPHOROS_ROOT/IntermediateProducts/<Catalog Type>/GalacticCorrectionCoefficientGrids/

  The default name follows the model grid name plus ``_MW_Param``. As
  before, click on ``Save Config. File`` to store the configuration
  file.

..
   Phosphoros requires as input the Fitzpatrick's Milky Way absorption
   law [Fit99]_ that is looked for in::

   > $HOME/Phosphoros/AuxiliaryData/ReddeningCurves/F99/F99_3.1.dat

   (see also the :ref:`File format reference <format-reference-section>`
   section).


.. _computing-redshifts:
    
Computing Redshifts
-----------------------------

The sub-panel five, ``5. Input/Output Files``, is the last step before
estimating the best-fit model and the photometric redshift for input
sources. Here, users have to specify the input catalog to analyze and
the outputs to be generated by Phosphoros (:numref:`bredshift`).

.. note::

   So far, users were not required to specify any input
   catalog. Previous steps in fact need to know only the catalog type
   which the input catalog belongs to.

.. figure:: /_static/basic_steps/ComputeRedshift.png
    :name: bredshift
    :align: center
    :scale: 40 %
	    
    Setting input/output of Phosphoros for the redshift computation in
    the GUI 
	    
Users need to fill the following information:

- **Input Catalog**
 
  As input catalog Phosphoros selects the catalog provided in the
  ``Catalog Setup`` panel. Different choices can be done using the
  ``Browse`` tab, as long as they belong to the Catalog Type defined
  above.

  On the right side, ``Filter Selection`` allows users to disable some
  of the previously selected filters. This is useful if users want to
  performed particular analyses with a reduce set of photometric bands.

  Checking on ``Fix Redshift from input catalog``, Phosphoros can also
  run with fixed redshifts, i.e. on a catalog where redshift is
  known for all sources, for example from spectroscopy. This can be
  useful to derive, for example, the source best fit SED and/or physical
  properties such as age, star-formation rate etc. The input catalog
  column containing the reference redshifts has to be selected from
  the ``Input catalog fixed redshift column`` drop-down menu.

  |br|
 
- **Output catalog**

  Phosphoros results are stored in an output file named ``phz_cat``
  that is by default located into::
 
    > $PHOSPHOROS_ROOT/Results/<Catalog Type>/<Catalog File Name>/
 
  where the ``Catalog File Name`` is the name of the input catalog
  file without the extension. Users can however choose another
  location by clicking on the ``Browse`` button. The output catalog
  can be saved either in FITS or in ASCII format.

  Columns from the input catalog can be also copied into the output
  catalog (``Output Content``). The ``Copy Columns (0)`` tab indicates
  that no input columns are selected. Click on it and a window will
  appear with the list of all input catalog columns. Select
  columns to be copied. The number in the ``Copy Columns`` tab will be
  updated.

  In addition, users can include in the output catalog the best-fit
  model parameters from the likelihood or posterior distribution or
  from both, selecting ``Best likelihood model`` and/or ``Best
  posterior model``.
 
  Typical ouput catalogs include the following information (see
  :ref:`File format reference <format-reference-section>` section for
  more details on output files):

  * the source ID,
  * the best model (:math:`z`, SED, E(B-V), reddening cuve) from the
    likelihood and/or posterior distribution,
  * the amplitude of the likelihood and/or posterior distribution at the
    maximum,
  * the normalization factor :math:`\alpha`,
  * the redshift value at the peak of the redshift PDF.
 
  |br|
 
- (Optional) **1D PDF**

  1D PDF of model parameters (from the likelihood and/or the posterior
  distribution) can be computed and stored for each source by
  selecting the desired parameters. Using the ``Generate 1D PDF as``
  tab, 1D PDFs can be saved as columns of the output catalog
  (containing vector data) or as individual FITS files, one per
  parameter (see :ref:`File format reference
  <format-reference-section>` section).

  In the GUI, 1D PDFs from a likelihood are generated using a *Maximum
  Likelihood* method, while 1D PDFs from a posterior distribution by
  marginalizing with respect to the other model parameters (see
  :ref:`axis-collapse` for more details).

  |br|
 
- (Optional) **Multi-Dimensional Output**
 
  Here, users can enable the generation of FITS files containing the
  likelihood and/or the posterior distributions, one per source. This
  action will produce a large volume of data (see the :ref:`File format
  reference <format-reference-section>` section).

  Multi-dimensional outputs can be investigated using the appropriate
  Phosphoros tool in the |CLI| (see the
  :ref:`posterior-investigation`).
       

After setting ``Input/Ouput``, users are ready to start the
computation of photometric redshifts, clicking on the ``Run``
button. All results are written into the ``Output Folder`` defined
above.
 
.. note::

   Users do not need to go through all the points above. Select just
   the ones you need. If the ``Run`` button is inactive, it means that
   something is not setup yet and the computing can not be done. In
   such case, just hover the mouse pointer on the button and a tool
   tip will apears with a list of the missing steps.

The ``Save Config. File`` exports the settings into a configuration
file. The file is stored into::

   > $PHOSPHOROS_ROOT/config/PhosphorosComputeRedshift.conf



.. bibliography:: references_basic_gui.bib
