.. _directory_howto_section:

How to modify the default directory structure
=============================================

Although discouraged, users have the possibility to alter the standard
directory structure, and in particular the location of the five
directories containing input/output data below the Phosphoros root
directory (see the :ref:`Basic Steps: Directory Organization
<directory-organization>`).

In the ``Configuration-->General`` panel of the GUI, users can
choose with the ``Browse`` button a new directory structure for
Phosphoros, i.e. the directories where to search input catalogs
and auxiliary data or where to store intermediate products and
results (:numref:`changedir`).

.. figure:: /_static/basic_steps/General_v12.png
   :name: changedir
   :align: center
   :scale: 50 %
	   
   ``Configuration`` panel of the GUI where to modify the directory
   structure 
	   

In the CLI mode, if the Phosphoros directory structure has changed,
users have to specify the location of needed directories every time an
action is run. The best option, again, is to use configuration files
generated with the GUI. Otherwise, for each action to run, ``Directory
options`` have to be set in the appropriate way. For example, running
the ``compute_model_grid`` action, the following options are
required::

  phosphoros-root=<top level directory>
  aux-data-dir=<directory containing the auxiliary data>
  intermediate-products-dir=<directory containing intermediate results>

or with the ``compute_redshifts`` action::

  phosphoros-root=<top level directory>
  aux-data-dir=<directory containing the auxiliary data>
  catalogs-dir=<directory containing the catalog files>
  results-dir=<directory containing the final PHZ results>
  intermediate-products-dir=<directory containing intermediate results>

Users can check the information required by each action using the
``--help`` option.
