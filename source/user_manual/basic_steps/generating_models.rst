Generating the model grid
=========================

GUI: Compute Redshift: 1. Model Grid Generation / Selection
-----------------------------------------------------------

This section described how to produce a grid of model photometry values. The ``Catalog Type`` and ``Parameter Space``
defined in previous can be recalled by providing the corresponding names in the first two fields of the GUI main window.
On the right side, you can also disable some of the filters, if you want to performed a particular analysis with a reduce set of
photometric bands.  |br|

.. figure:: /_static/first_step/ModelGridSubPanel1.png
    :align: center

The "Compute Redshifts" window is organized into four possible successive steps. The first one is concerned with the
photometry model grid generation::

 1- Model Grid Generation / Selection (in orange)

To produce your models you should provide the following information:

1. **IGM absorption type** |br|

 Select one of the IGM absorption types (Madau, Meiksin, Inoue or Off ) to be applied.

 (Reference TO Be Added)

2. **Model Grid File** |br|

 Give a filename for storing your model grid. By default, a filename is automatically
 generated concatenating the ``Catalog Type``, ``Paramter Space`` and ``IGM absorption Type`` names.

 The generated file ``Grid_Chalenge2_Parameter_Space_1_MADAU.dat`` is stored in the following directory::
 
 $HOME/Phosphoros/IntermediateProducts/"Catalog Type"/ModelGrids

 In our example ``Catalog Type`` is replaced by ``Challenge2``

3. **(Re) Generate the Grid**

 Click on this button to generate your photometry models.
 
4. Get Config File (optional)

 This button allows to output a configuration file, which can then be used to compute the grid from the command line.
 You can enter a filename (the standard is ``PhosphorosComputeModelGrid.conf``). The file will appear in

 $HOME/Phosphoros/config/PhosphorosComputeModelGrid.conf

Note:
    The orange coloring indicates that no model grids have been produced for the relevant specification selection yet.
    After the calculation is completed, the color turns to black indicating that the values have been computed
    and stored in the ``Model Grid File`` where the values can be directly read for subsequent steps of the analysis.

    If you change anything in the specifications, the colour turns to orange again reminding that a new grid must be
    generated before continuing the analysis.
 
CLI: ``compute_model_grid (CMG)``
---------------------------------

The photometry model grid can also be generated using the ``compute_model_grid`` action (which calls the﻿
PhosphorosComputeModelGrid C++ executable). The list of command line options can be displayed as follows::

 > Phosphoros compute_model_grid --help

but the easiest is probably to provide them through a configuration file (see :ref:`here <config-file-usage>` for
configuration file best usage).

Part of the details of the PhosphorosComputeModelGrid.conf files is presented in previous :ref:`section <PhosphorosComputeModelGrid_configuration_section>`.

A complete and fully documented PhosphorosComputeModelGrid.conf is available as part of the installed software in the following location::

    /path_to_PhosphorosCore_installation_directory/conf/PhzExecutables/PhosphorosComputeModelGrid.conf

With a system installation, ``/path_to_PhosphorosCore_installation_directory`` should be something like
``/opt/euclid/PhosphorosCore/0.5/InstallArea/x86_64-fc23-gcc53-o2g/" where ``0.5`` is PhosphorosCore version and
``x86_64-fc23-gcc53-o2g`` reflects some of the compilation options. These two fields takes different values in different installations.

