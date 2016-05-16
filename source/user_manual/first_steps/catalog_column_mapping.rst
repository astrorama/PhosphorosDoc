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
available filter files (i.e., those located in $PHOSPHOROS_ROOT/AuxiliaryData/Filters/), but It needs to be instructed in
catalog column names. They can be provided by filling appropriate GUI cells, but there is an easier way. Phosphoros can read
column names from a catalog taken as a model for this occasion.

Let us first make sure that a sample, model catalog (for Challenge 2 in this case) is available at the correct location::

    > cd ~
    > cd Phosphoros (or cd $PHOSPHOROS_ROOT)
    > cd Catalogs
    > mkdir Challenge2 (this will be the ``catalog type`` reference)
    > cd Challenge2
    > wget http://www.isdc.unige.ch/phosphoros/data/Challenge_2/Challenge2TrainingSmallCatalog.fits.gz
    > gzip -d Challenge2TrainingSmallCatalog.fits.gz

Please note that ``Challenge2`` has been chosen as a ``catalog type`` name. This means that all catalog with identical
column names (i.e., all files from challenge 2 in our case) must be located below ``$HOME/Phosphoros/Catalogs/Challenge2/``.
Phosphoros also now knows about the ``catalog type`` name (but only after restarting the GUI).

Restart the GUI, click on ``Catalog Types``, select the ``Challenge2`` catalog type and ``Edit``.

.. figure:: /_static/first_step/CatalogType_Edit.png
    :align: center

Next click on ``Select File and Import Columns``, browse and select the ``Challenge2TrainingSmallCatalog.fits`` which
was just inserted.

.. figure:: /_static/first_step/CatalogType_SelectCat.png
    :align: center

The column name providing source ID can be enter through the ``Source ID Column``. Then, press ``Select Filters``.

.. figure:: /_static/first_step/CatalogType_SelectFilters.png
    :align: center

Another window opens where filter names can be selected. When the filter selection is completed, pressing ``Save`` closes
the window and, as shown below, fills automatically the ``Filter Transmission Curve`` column. Each of the ``Flux Column Name`` and ``Error Column Name``
cells now features a drop down menu which can be used to specify the appropriate Flux and FluxError column names.

.. figure:: /_static/first_step/CatalogType_ColumnNames.png
    :align: center

When names have been entered for all filter, this process must terminated by clicking on the ``Save`` middle-frame button.

CLI: Editing or creating a ``filter_mapping.txt`` file
------------------------------------------------------

.. image:: /_static/construction.png
   :align: center
   :scale: 50 %

..
    The result of the mapping is saved into an ASCII file located at ...