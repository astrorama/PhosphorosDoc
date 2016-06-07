Generating the model grid
=========================

GUI: Compute Redshift: 1. Model Grid Generation / Selection
-----------------------------------------------------------

This section described how to produce a grid of model photometry values for a given
``Catalog Type`` and ``Parameter Space`` (specified as indicated in previous sections. |br|

.. figure:: /_static/first_step/ModelGridSubPanel1.png
    :align: center

The "Compute Redshifts" window is organized into four possible successive steps. The first one is concerned with the
photometry model grid generation.
focus on the first one (red frame)::

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

 If you click on this button, the corresponding configuration file, which can be used to compute the grid from the command line.
 You can provide a filename (e.g. ``PhosphorosComputeModelGrid.conf``) which will be located in

 $HOME/Phosphoros/config/PhosphorosComputeModelGrid.conf

Note:
 The orange color (on the image) means that we have not produced any model grid
 for our selection yet. The color will turn in black immediately after your models 
 have been created. Each time you see this orange color at any sub-panel it means either you forget
 to set something or the setting is not applied yet.
 
CLI: Phosphoros compute_model_grid
----------------------------------

The photometry model grid can also be generated using the ``compute_model_grid`` action (which calls the﻿
PhosphorosComputeModelGrid C++ executable). The list of command line options can be displayed as follows::

 > Phosphoros compute_model_grid --help

but the easiest is probably to provide them through a configuration file (see :ref:`here <config-file-usage>` for the best
configuration file usage).
﻿
CLI: Phosphoros compute_model_grid configuration details
--------------------------------------------------------

TB Added later