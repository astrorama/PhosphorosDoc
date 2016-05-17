Generating the models
=====================

GUI
---

This section is for computing all our photometry models based on the
``Catalog Type``, the ``Parameter Space`` and the ``Filter Section`` you
supposed to have already defined in the previous sections. |br|

.. figure:: /_static/first_step/ModelGridSubPanel1.png
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

To produce the photometry models using the |CLI| interface proceed as follows::

 > Phosphoros compute_model_grid --output-model-grid="model_grid.fits"

Phosphoros will produce the model grid based on parameters stored in the
default configuration file named ``PhosphorosComputeModelGrid.conf`` and store
the result in the ``model_grid.fits`` file here given by the user.
The configuration file is located at the default installation location::

 <EUCLID_BASE>/<PROJECT_NAME>/<VERSION>/InstallArea/<BINARY_TAG>/conf/PhzExecutables/PhosphorosComputeModelGrid.conf

But you can give a different configuration file as follows::

> Phosphoros compute_model_grid --output-model-grid="model_grid.fits" --config-file="user_conf_file.conf"

In this example, Phosphoros will not use the default configuration file but
instead the one defined by the user and from the ``$HOME/Phosphoros`` location, if
no path is specify by the user. See also the default directory organization 
here: :ref:`directory-organization`.

There are a lof of more options you can play with, see the help for more
information::

 > Phosphoros compute_model_grid --help

Show the command. Mention the default configuration file name. Explain where the
files are created (and the reasoning behind the default naming).