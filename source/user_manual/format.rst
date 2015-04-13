.. _file-format:

*********************
File Format Reference
*********************

.. _dataset_file_format:

Dataset File Format
===================

There are three types of dataset files, SEDs, Reddening Curves and Filters, all
being inputs of the *Build Templates Library* step. All these files are ASCII
space separated tables with two columns. The first column contains wavelength
values expressed in |AA|. The second column depends on the file type:

- SED : Flux in erg/s/cm\ :sup:`2`/|AA|
- Reddening Curve : The values of the :math:`k_{(\lambda)}`
- Filter : Transmission value in range [0,1]

The file can contain any number of comments, starting with the symbol **#**. If
the first line of the file is a one-word comment, it is going to be used as the
name of the dataset. If there is no such line in the file, the name of the file
itself is used as the name of the dataset (without the extension).

.. _photometry_grid_format:

Photometry Grid Format
======================

Because of size limitations, the photometry grid file is stored in an internal
Phosphoros format. Access from the C++ language can be done by using the
Phosphoros *PhzDataModel* module. Access outside C++ can be performed with the
Phosphoros action :ref:`display_templates <display_template_options>`.

.. _catalog_format:

Catalog Format
==============

The Phosphoros input catalogs can be either ASCII or FITS tables (Phosphoros
will try to auto-detect the type). They must contain the following columns:

- ID : The ID of the source
- Filter Flux : One column for each filter, containing the Flux in |mu|\ Jy
- Filter Error : One column for each filter, containing the Flux error
- SpecZ : The spectroscopic redshift (only for training catalogs)

Phosphoros provides flexible description of the catalog columns via |CLI|
parameters, either by their names or by their indices, so it does not require
specific order or naming of the catalog columns. For a detailed description of
these parameters see :ref:`here <config-section-DZP-training>`.

.. _phot-corr-format:

Photometric Zero Point Corrections Format
=========================================

This file is an ASCII table with two columns. The first column is the filter
fully qualified name (including the group information) and the second one is
the photometric correction value. Note that the corrections are Flux corrections
and not magnitude.

.. _best-fit-catalog-format:

Best Fit Catalog Format
=======================

The best fit catalog is an ASCII table with the best fitted models. It contains
the following columns:

- ID : The ID of the source
- SED : The name of the SED template
- ReddeningCurve : The name of the reddening curve used for applying the extinction
- E(B-V) : The E(B-V) value used for applying the extinction
- Z : The redshift
- Scale : The scale factor |alpha|, the model has been scaled with

.. _pdf-fits-format:

PDF\ :sub:`(Z)` Format
======================

This is a FITS file which contains the PDF\ :sub:`(Z)` for all sources. Each
PDF\ :sub:`(Z)` is a binary table HDU with two columns, the first of which
represents the redshift and the second the probability. The name of each HDU is
the ID of the corresponding source and it can be used for searching the PDFs.
Alternativelly, the order of the HDUs matches the order of the sources in the
input catalog (starting from the first extension HDU).

.. _likelihood-fits-format:

:math:`\mathcal{L}_{(SED, RedCurve, E_{(B-V)}, Z)}` Format
==========================================================

Phosphoros (when this output is enabled) produces one likelihood FITS file for
each source of the catalog. The name of the file is the ID of the source, with
the extension *fits*. It contains the following HDUs:

- Primary : A 4-dimensional array containing the likelihood (order of axes: Z, E\ :sub:`(B-V)`, RedCurve, SED)
- Z : A single column binary table with the values of the Z axis
- E(B-V) : A single column binary table with the values of the E(B-V) axis
- Reddening Curve : A single column binary table with the values of the Reddening Curve axis
- SED : A single column binary table with the values of the SED axis

Note that Phosphoros provides a tool for visualising files of this type. This
tool is the ``display_likelihood`` action. More details about how to use this
action can be found :ref:`here <display-ikelihood-options>`.