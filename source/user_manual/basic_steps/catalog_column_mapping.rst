Catalog Type: Mapping filters to column names
=============================================

In order to compute modelled photometry values, Phosphoros needs filter transmission curves which correspond to the filters
used to obtained the observed photometric values. In general, the names of the observed photometric bands, typically
reflected in the catalog column names may not match the names of the filter transmission files. Consequently, the mapping of
these names must be specified. The easiest is to use the GUI although an ASCII file can also be used providing it is located
at the right place in the Phosphoros directory structure.

GUI:  ``Catalog Types``
-----------------------

Before achieving the mapping, Phosphoros must be aware of the different names. It can read the names of the
available filter files (i.e., those located in $PHOSPHOROS_ROOT/AuxiliaryData/Filters/), but It needs to be instructed about
the catalog column names. They can be provided by filling appropriate GUI cells, but there is an easier way. Phosphoros can read
column names from a catalog taken as a model for this occasion.

Let us first make sure that a sample, model catalog (for Challenge 2 in this case) is available at the correct location::

    > cd ~
    > cd Phosphoros (or cd $PHOSPHOROS_ROOT)
    > cd Catalogs
    > mkdir Challenge2 (this will be the catalog type reference)
    > cd Challenge2
    > wget http://www.isdc.unige.ch/phosphoros/data/Challenge_2/Challenge2TrainingSmallCatalog.fits.gz
    > gzip -d Challenge2TrainingSmallCatalog.fits.gz

Please note that ``Challenge2`` has been chosen as a ``catalog type`` name. This means that all catalogs with identical
column names (i.e. all files from challenge 2 in our case) must be located below ``$HOME/Phosphoros/Catalogs/Challenge2/``.
Now Phosphoros also knows about the ``catalog type`` name (but only after restarting the GUI).

Restart the GUI, click on ``Catalog Types``, select the ``Challenge2`` catalog type and ``Edit``.

.. figure:: /_static/first_step/CatalogType_Edit.png
    :align: center

Next click on ``Select File and Import Columns``, browse and select the ``Challenge2TrainingSmallCatalog.fits`` which
was just inserted.

.. figure:: /_static/first_step/CatalogType_SelectCat.png
    :align: center

The column name providing source ID can be entered through the ``Source ID Column``. 

If the catalog has some sources with missing photometry (sources which where not observed in all catalog's bands), you have to check the corresponding control and provide the value of the flag. By doing so you instruct the program to skeep photometry having the flag value in the flux column. 

If the catalog contains some objects whith upper limit (sources which where observed but not detected in some band, for which the provided photometry is the upper limit of the flux and not the nominal flux), you have to check the corresponding control and ensure that the catalog follow the upper limit convention. The program will considere as an upper limit a photometry which corresponding error has a negative value.
 
Then, press ``Select Filters``.

.. figure:: /_static/first_step/CatalogType_SelectFilters.png
    :align: center

Another window opens where filter names can be selected. When the filter selection is completed, pressing ``Save`` closes
the window and, as shown below, fills automatically the ``Filter Transmission Curve`` column. Each of the ``Flux Column Name`` and ``Error Column Name``
cells now features a drop down menu (after clicking on the cells) which can be used to specify the appropriate Flux and FluxError column names.

.. figure:: /_static/first_step/CatalogType_ColumnNames.png
    :align: center

When names have been entered for all filter, this process must terminated by clicking on the ``Save`` middle-frame button.
Please note that you can always add or remove filters after a first mapping has been completed, by going back to the ``Select Filters`` option.

CLI: Editing or creating a ``filter_mapping.txt`` file
------------------------------------------------------

With the very last step of previous section, i.e. clicking on the ``Save`` middle-frame button of the Catalog Type definition,
an ASCII file named ``filter_mapping.txt`` is created in the following directory::

  $HOME/Phosphoros/IntermediateProducts/"Catalog_Type"/filter_mapping.txt


where ``Catalog_Type`` directory corresponds to your catalog type name (i.e., ``Challenge2`` in our example).

You can always edit this file to make corrections. Alternatively, you can create such a file with your favorite editor
(rather than using the GUI). When launched, the GUI will automatically load any ``filter_mapping.txt`` file located in
the appropriate directory, providing it respect the proper formatting.

In our example, the filter mapping looks like::

    DECAM/g FLUX_G FLUXERR_G
    DECAM/i FLUX_I FLUXERR_I
    DECAM/r FLUX_R FLUXERR_R
    DECAM/z FLUX_Z FLUXERR_Z
    EUCLID_DC1/vis FLUX_VIS FLUXERR_VIS
    vista/H FLUX_H FLUXERR_H
    vista/J FLUX_J FLUXERR_J
    vista/Y FLUX_Y FLUXERR_Y


It includes 3 columns:

 - **Column 1**: The qualified name of the file containing the filter transmission curve (e.g. directory name / filter name)|br|
 - **Column 2**: The Catalog flux column name corresponding to your filter |br|
 - **Column 3**: The Catalog flux error column name corresponding to your filter |br|

Please note that when you modify any of the GUI files using another editor, you always have to restart the GUI for the
changes to be taken into account.
