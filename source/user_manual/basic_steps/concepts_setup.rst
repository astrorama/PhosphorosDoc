.. _concepts_setup:

Important concepts and data setup
========================================

.. _catalog-type:

Catalog type
------------------------------

Modern surveys typically make their catalog data available through a
number of files, either because of the big size and/or because there
are different samples (e.g., with or without spectroscopic
redshift). Assuming that these files are standard for a given survey,
with the same column names in particular, the concept of **catalog
type** is introduced. We consider that all catalog files having
identical column names, with photometric band names referring to same
filters, belong to the same catalog type.

The catalog type concept is exploited to define the naming convention
in the Phosphoros directory structure used to organize files (see the
next section). It is also useful for the :ref:`mapping operation
<mapping>` (that connects input photometry to the corresponding filter
transmission curves) because this operation can be defined only once
for all files of the same catalog type.


.. _directory-organization:

Directory Organization
--------------------------------

All data files are organized into a standardized directory structure
(see :numref:`dirstruct`). This allows to greatly reduce the amount of
specifications as the code automatically knows where to look for input
data or where to write output files, without necessarily relying on
additional user configurations.

.. figure:: /_static/basic_steps/Phos_Dir_structure.pdf 
    :name: dirstruct
    :align: center
    :scale: 50 %

    Standard directory structure of Phosphoros
	    
In most cases, there is no need to alter the standard directory
structure, nor even to understand all details. The simplest is to used
the default configuration (see :ref:`here <directory_howto_section>`
for changing the standard structure).

Phosphoros Root Directory
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The root directory is the location of the top-level Phosphoros
directory. By default, it is ``$HOME/Phosphoros``. This location can
be simply modified by setting the ``PHOSPHOROS_ROOT`` environment
variable to a different directory.

.. ref:`using-dockphos`


..    setting the ``PHOSPHOROS_ROOT``
      environment variable or with the start command as::

      > DockPhos.py start -d /<new Phosphoros root directory name>

      See the :ref:`Using a different Phosphoros root directory
      <docker-installation>` section for more details.

Below the root directory, there are five main directories:

``Catalogs``, ``AuxiliaryData``, ``IntermediateProducts``, ``Results``
and ``Config``.

Catalogs
^^^^^^^^^^^^^^^^

Input catalog files contain multi-band photometric flux measurements,
with their errors. Rows refer to different sources. One source ID
column (e.g., ``OBJECT_ID``) must be present. The catalog format is
either ASCII or FITS as described in the :ref:`Catalog format
<format-catalogs>` section. Fluxes must be provided in :math:`\mu`\ Jy
unit (AB magnitudes are also accepted).

Input catalogs are placed into sub-directories according to their ``catalog
type``. For example, catalogs belonging to the ``COSMOS`` catalog type
are found into::

      > $PHOSPHOROS_ROOT/Catalogs/COSMOS/

.. tip::

   Input catalog files in FITS format can be examined using, for
   example, the TOPCAT software [#f1]_.

    
Auxiliary Data
^^^^^^^^^^^^^^^^^^^^^^

All input files that are not catalogs are referred to as **auxiliary
data**. They include all the required information to build the grid of
models. Auxiliary data needed in the Phosphoros database are:

* **Filter transmission curves**, which characterise the telescope
  full :ref:`transmission curves <filter-curves>` as a function of
  wavelength, including the telescope optic, the filter itself and the
  detector efficiency. Values range between 0 and 1. They are
  typically found in sub-directories of the ``Filters`` directory::

       > $PHOSPHOROS_ROOT/AuxiliaryData/Filters/

  Filter transmission curves can refer either to photon-counting or
  energy-measuring systems. By default, Phosphoros assumes
  photon-counting systems (see the :ref:`Auxiliary Data
  Format <auxiliary_format>` section to know how to handle
  energy-measuring systems).

* **Spectral Energy Distribution (SED) templates**, which consist in
  restframe :ref:`SED templates <templates>` of galaxies, stars, QSOs,
  etc. They are expected in erg/s/cm\ :sup:`2`/|AAm|, and they are
  found in sub-directories of::

       > $PHOSPHOROS_ROOT/AuxiliaryData/SEDs/

  Based on SED templates, Phosphoros generates a grid
  of modeled photometry that are compared with photometric
  measurements.

  .. note::

     Before generating the model grid, Phosphoros normalizes all SED
     templates to the solar luminosity at 10pc distance with respect
     to a given filter transmission (that users can choose). In
     this way, Phosphoros can provide physically well defined values
     of the scale factor in output catalogs.

* **Reddening Curves**, which provide the attenuation curves required
  to compute the :ref:`intrinsic absorption
  <intrinsic-interstellar-dust>` caused by interstellar dust in
  galaxies. They are found into::

       > $PHOSPHOROS_ROOT/AuxiliaryData/ReddeningCurves/

All these input files must be ASCII tables, with the wavelength in
|AAm| as first column and the specific values as second column.

Optional functionalities in Phosphoros require additional auxiliary
data that are also located in sub-directories of the ``AuxiliaryData``
directory.

..
 For example, files containing luminosity functions are located in
 the ``LuminosityFunctionCurves`` directory, **axis** and **generic
 priors** in the ``AxisPriors`` and ``GenericPriors`` directories,
 respectively.

 ..
    Additional auxiliary data can be also present, such as
    luminosity functions, axes priors and multi-dimensional generic
    priors, etc. They are required to use Phosphoros optional
    functionalities (see the :ref:`Advanced Features
    <user-manual-advanced>` section). Additional sub-directories can
    be then created to organize optional auxiliary data files in the
    most logical way. Users can complete or re-arranged these
    sub-directories to match their preferred organization scheme. More
    detailed information on Auxialiary data format can be found in the
    :ref:`File Format Reference <format-reference-section>` section.

Information on the auxiliary data format can be found in the
:ref:`File Format Reference <format-reference-section>` chapter.


Intermediate Products
^^^^^^^^^^^^^^^^^^^^^^^^^^

Intermediate products are all the relevant files produced by
Phosphoros before the execution of the ``Redshift Estimate``
step. They can be reused for different runs. Typical intermediate
products are the grid of models, the grid of luminosity models, the
filter mapping, etc. They are organized per catalog type, e.g. for the
``Cosmos`` catalog type::

      > $PHOSPHOROS_ROOT/IntermediateProducts/Cosmos/

When using Phosphoros through the GUI you will never need to open
the ``IntermediateProducts`` folder. If you use the CLI you may have
to locate files to be provided to the next computation step.

      
Results
^^^^^^^^^^^^^^^^^^^^

The main product of Phosphoros is an output source catalog that
includes redshift estimates, best-fit models and, optionally,
1D PDFs of model parameters (see the :ref:`Compute Redshifts
<computing-redshifts>` section). File format can be ASCII or
FITS. Output data are organized per catalog type, e.g.::

      > $PHOSPHOROS_ROOT/Results/Cosmos/

Configuration Files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

:ref:`Configuration files <config-file-usage>` include the list of
command options required to run Phosphoros executables in
the |CLI|. They are typically found into::

      > $PHOSPHOROS_ROOT/config/

This folder contains also the GUI internal configuration
(``$PHOSPHOROS_ROOT/config/GUI/``), which you should not alter by
hand.

.. Explain the logic behind the organization of the Phosphoros directories. This
    should include the catalog-type concept. Here we should not explain every single
    one of the directories, but focus more on the concept and mention the most used
    ones. We should also mention the PHOSPHOROS_ROOT environment variable.*

.. _data-pack:
    
Phosphoros *internal database*
-----------------------------------------

..
  In order to make input and auxiliary data available to data analysis,
  users need first to create the Phosphoros directory structure.
  The most convenient way to do this is to download the tar file
  from *(?)*, which contains the default Phosphoros directory
  structure and sets of Filters, SEDs and Reddening curves *(?)*, and
  to expand it at the working location, i.e.::

      > cd $HOME or cd $PHOSPHOROS_ROOT
      > wget https://github.com/astrorama/phosphoros-quickstart/archive/master/quickstart.tar.gz
      > tar -xzf Challenge2Data.tar.gz

In order to make input and auxiliary data available to data analysis,
they first have to be imported inside the Phosphoros directory
structure. When launching Phosphoros for the first time, it will
automaticall create the folder structure under the
``$PHOSPHOROS_ROOT`` folder.

The standard procedure is to import input catalogs and auxiliary data
files, such as filter transmission curves or SEDs, into the Phosphoros
*internal database*. All the operations such as importing, moving and
deleting files can be done using the shell commands such as ``cp``,
``mv`` or ``rm`` (or using the GUI, which can import or delete
folders). Users can also create or re-arrange sub-directories in the
Phosphoros structure to match their preferred organization scheme by
the ``mkdir`` shell command or by the GUI.

The Phosphoros Auxiliary Data Pack can be dowloaded from the
Phosphoros repository through the GUI (see :ref:`config`) or the CLI
(see :ref:`CLI: Download Data Pack<cli-data-pack>`). Files will be
automatically located in the proper directories. This data pack
contains a set of filter transmissions for the main recent
UV/optical/IR surveys, the commonly adopted reddening curves and a
large library of SED templates (a full description of the data pack
can be found in the :ref:`data-repository` chapter).

Much of the data manipulated by Phosphoros can be reused in different
analyses. The directory structure described above is designed to keep
the input, intermediate and output data files of an arbitrary number
of analyses.

..
   It is however not making use of any real databases (such as mysql)
   as it just relies on the file system organisation.
   
   If catalog files are places at the appropriate location (according
   to their catalog types), intermediate data products and final
   results are sorted in such a way as to co-exist with equivalent
   files obtained from other analyses. The idea is to *save* results
   of as many analyses as wanted in a logical organisation. There are
   also options to overwrite or delete any Phosphoros output.

..
  The standard procedure is to *import* input catalogs and auxiliary
  data files, such as filter transmission curves or SED templates,
  into this kind of underlying database. This can be simply done by
  shell command lines, such as ``cp``, ``mv`` or ``rm``.

..
  As it relies on the file system, any operation with the GUI (such
  as ``importing``, ``moving`` or ``deleting`` files) performs
  equivalently as a simple shell command line (as ``cp``, ``mv`` or
  ``rm``).


.. rubric :: Footnotes

.. [#f1] see http://www.star.bris.ac.uk/~mbt/topcat/



..    
    All the auxiliary dara are located in::

      > $HOME/Phosphoros/AuxiliaryData/<auxiliary data name>

    where the name of the sub-directory depends on the data type and
    is given in the Table below.

    +------------------------+---------------------------+
    | Auxiliary data         | sub-directory name        |
    +========================+===========================+
    | Filter curves          | Filters/                  |
    +------------------------+---------------------------+
    | SED templates          | SEDs/                     |
    +------------------------+---------------------------+
    | Reddening curves       | ReddeningCurves/          |
    +------------------------+---------------------------+
    +------------------------+---------------------------+
    | Luminosity functions   | LuminosityFunctionCurves/ |
    +------------------------+---------------------------+
    | Axis priors            | AxisPriors/               |
    +------------------------+---------------------------+
    | Generic priors         | GenericPriors/            |
    +------------------------+---------------------------+
