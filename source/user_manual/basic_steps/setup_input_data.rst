Setting up input data
=====================

..  First explain what the input data are. At this level we should limit it to the
    catalogs, filters, SEDs and reddening curves. We should not describe the formats
    of the files, but have links to the format reference section.

Catalogs
--------

The main Phosphoros input data is a **catalog** file. A catalog is a table including
as columns photometric measurements obtained through a number of different filters,
with their corresponding errors, either in flux or magnitude. Rows refer to different sources
and one column named ``ID`` must be present. The catalog format is either ASCII or FITS
as described in this section (TBD). Fluxes must be provided in |mu|\ Jy unit.

Input catalog files can be examined using TOPCAT (http://www.star.bris.ac.uk/~mbt/topcat/) for example.
They must be placed in the $PHOSPHOROS_ROOT directory ($HOME/Phosphoros by default) before starting any
analyses, using shell commands (mv or cp). Files must be organized into sub-directories according to
their different type (see above catalog type concept :ref:`section. <catalog-type>`).

For example, considering a set of catalogs from the ``COSMOS`` and the ``Euclid Challenge 2`` types, the
corresponding files must be located into::

    > $HOME/Phosphoros/Catalogs/COSMOS/...

and::

    > $HOME/Phosphoros/Catalogs/Challenge2/...

respectively.

.. _aux-data:

Auxiliary Data
--------------

All input files which are not catalogs are referred to as ``Auxiliary`` data. They comprise the following types:

**Filter transmission curves**: characterise the full transmission in the range [0, 1], including the
    telescope optic, the filter itself and the detector efficiency.

**Spectral Energy Distribution (SED)**: in erg/s/cm\ :sup:`2`/|AA| which are to be integrated through the
    filters to compute the modelled photometric values.

**Reddening Curve**: providing the :math:`k_{(\lambda)}` values required to compute the ``internal``
    absorption caused by the interstellar matter in the galaxy.

The input files corresponding to these three types of data must be formatted as ASCII tables, with |AA| wavelengths
in the first column and specific values in the second one.

CLI: Managing Auxiliary Data
----------------------------

In order to make auxiliary data available to data analysis, they first have to be imported inside the Phosphoros directory structure.

The most convenient way to do this is to download a tar file containing already sets of Filters, SEDs and Reddening curves and to expand it at the correct location. i.e.::

    > cd ~
    > cd Phosphoros (or cd $PHOSPHOROS_ROOT)
    > wget https://github.com/astrorama/phosphoros-quickstart/archive/master/quickstart.tar.gz 
    > tar -xzf quickstart.tar.gz

Files are arranged inside three directories ``Filters``, ``SEDs`` and ``ReddeningCurves`` below ``$HOME/Phosphoros/AuxiliaryData``.
Optional ``group`` sub-directories can be added to organize auxiliary files in the most logical way. Users can complete or re-arranged
these sub-directories to match their preferred organization scheme.

Any auxiliary files following the supported formats (REFERENCE NEEDED) can also be added and arranged using shell commands
such as ``mkdir``, ``mv``, ``cp`` or ``rm``.

GUI: ``Configuration`` : ``Aux. Data Management``
-------------------------------------------------

The Phosphoros GUI can also be used to display and managed auxiliary data files. Start the GUI with ``Phosphoros GUI``
and click on ``Configuration`` and ``Aux. Data`` tabs. This is not yet the place for selecting filters or SEDs for a
particular analysis (for this see :ref:`Parameter Space Definition <parameter-space-definition>` below).

First, the GUI provides a view of all available auxiliary data (if any) as shown on the screenshot below for the case
of the ``Filter Transmission``. Similar views can be displayed for the ``SEDs``, ``Reddening Curves`` and ``Luminosity
Function Curves`` auxiliary files.

.. figure:: /_static/first_step/AuxDataManagement.png
    :align: center

Second, there are functionalities to ``Import`` new files or to ``Create (Sub)-Group``. Selecting ``Import`` opens a
new window showing how to import a single file or a entire directory content.
