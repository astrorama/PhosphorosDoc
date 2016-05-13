Some important concepts
=======================

.. Explain the logic behind the organization of the Phosphoros directories. This
  should include the catalog-type concept. Here we should not explain every single
  one of the directories, but focus more on the concept and mention the most used
  ones. We should also mention the PHOSPHOROS_ROOT environment variable.


.. _catalog-type:

Catalog type
------------

Modern surveys typically make their catalog data available trough a number of 
files, either because of the big size and/or because there are different samples (with 
or without spectroscopic redshift for example). Assuming that these files are
standard in a given survey, with the same column names in particular, the concept 
of ``catalog type`` is introduced. We consider that all catalog data files having
identical column names, with photometric band names referring to the same filter
belong to the same ``catalog type``.

One of the required configuration task is the mapping of the catalog photometry column names to the
files providing the filter transmission curves. This mapping can
be defined only once for all files of the same type ``catalog type``.

The ``catalog type`` concept is also exploited to define the directory structure naming convention
used to organize Phosphoros files and described in the next section.

.. _directory-organization:

Directory Organization
----------------------

All data files are organized into a standardized directory structure. This allows to greatly
reduce the amount of specifications as the code automatically knows where to look for
input data and where to write output files without necessarily relying on additional user configuration.

Below the root directory ($HOME/Phosphoros by default), there are five main directories.

**Catalogs**:
    input catalogs files located into sub-directories according to their ``catalog types`` (e.g. Cosmos
    catalog files would be located into "$HOME/Phosphoros/Catalogs/Cosmos/")

**AuxiliaryData**:
    auxiliary data includes (1) filter transmission curves, (2) Spectral Energy Distribution (SED) templates, (3) reddening curve files, etc.

**IntermediateProducts**
    intermediate products organized per ``catalog type`` which can be reused for different runs

**Results**
    resulting data products also organized per ``catalog type``

**config**
    configuration files

In most cases, there is no need to alter the standard directory structure, nor even to understand all details. The simplest is to used the default configuration.
The location of the top-level directory can simply be specified by setting the PHOSPHOROS_ROOT environment variable.

In case of particular needs, the location of the above five directories can be changed using options which are described in the following advanced :ref:`section. <directory_howto_section>`

.. Explain the logic behind the organization of the Phosphoros directories. This
    should include the catalog-type concept. Here we should not explain every single
    one of the directories, but focus more on the concept and mention the most used
    ones. We should also mention the PHOSPHOROS_ROOT environment variable.*

Phosphoros "internal" data
--------------------------

Much of the data manipulated by Phosphoros can be reused in different analyses. The directory structure described in previous section
is designed to hold the input, intermediate and output data files of an arbitrary number of analyses. It can be seen as an kind of underlying "internal data base". It is
however not making use of any real databases (such as mysql) as it just relies on the file system organisation.

When catalog files are put at the appropriate location (according to their catalog types), intermediate data products and final results are sorted in such a way as to co-exist
with equivalent files obtained from other analyses. The idea is to ``save`` the results of as many analyses as wanted side by side in as logical an organisation as possible.
There are also however options to overwrite or delete any Phosphoros outputs.

The standard procedure is to ``import`` auxiliary data files, such as filter transmission or SEDs, into this kind of underlying
data base. As it relies on the file system, any operation performs with the GUI such as ``importing``, ``moving`` of ``deleting`` files as a simple shell command line
equivalent (cp, mv or rm).