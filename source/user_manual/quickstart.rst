.. _quickstart:

************************
Quickstart in 15 minutes
************************

If you have never used Phosphoros and you want to whet your appetite, this 15
minutes quickstart tutorial is intended just for you! It will give you a brief
overview of the Phosphoros workflow and guide you through a simple example of
computing photometric redshifts, without explaining in detail each step. At the
end of the quickstart you will have an idea how it feels like to use Phosphoros.

This quickstart assumes that you have a functional version of Phosphoros already
installed. If you do not, before you continue, you should install Phosphoros by
following the instructions :ref:`here <phosphoros-install>`.

Template fitting in two words
=============================

Phosphoros is a tool for estimating photometric redshifts using
template fitting method. The method consists of two steps. During the
first step, which has to be performed only once, we define a set of
models, for which we compute photometry in specific wavelength
bands. You can imagine models as a multi-dimensional parameter space,
whose axes are SED template, redshift and reddening (color excess and
extintion curve). Each point of this parameter space contains modeled
photometry.

During the second step, we take the photometry of a source of unknown
redshift and we compare them with our models (using the :math:`\chi^2`
distance). Based on which models match better the observed
photometries, we estimate the photometric redshift.

.. The different options of Phosphoros for performing the above steps
   are explained throught the User Manual and are not further
   explained during this quickstart.

.. The quickstart data (next section) contain already all the
   requirements for both steps of the template fitting.

The following sections will navigate you through the quickstart data,
settings, and a basic data analysis, in order to get an idea on how
each step is performed with the Phosphoros Graphical User Interface
(GUI).

Getting the quickstart data
===========================

All the data required to run the quickstart are provided as a single tar.gz
file, which can be found in the :ref:`data` page. You just have to uncompress
the file in a directory called ``Phosphoros`` under your home directory. The
following commands will get the file and uncompress it::
    
    > cd ~
    > mkdir -p Phosphoros
    > cd Phosphoros
    > wget http://www.isdc.unige.ch/euclid/phosphoros/data/other/quickstart.tar.gz
    > tar -xzf quickstart.tar.gz
    
.. note::
   
    Phosphoros uses the ``~/Phosphoros`` directory as the default root directory.
    If you want to use a different directory, you can set the environment variable
    ``PHOSPHOROS_ROOT`` to the directory you want.

..    In this case, you will have to uncompress the quickstart data in this directory.
    
Executing Phosphoros
====================

To start Phosphoros you need to run the command::
    
    > Phosphoros GUI
    
This will open the main window with the Graphical User Interface (GUI)
of Phosphoros, as seen at the following image.

.. figure:: /_static/quickstart/MainWindow.png
    :align: center
    :scale: 30%	    
..     :height: 500px  

At the top part of the main window, you can see the navigation menu. This menu
is visible at all times, and it allows you to navigate through the different
Phosphoros panels.

Examining the input catalog
===========================

The quickstart data contain a single catalog file,
``Catalogs/Quickstart/quickstart.fits``.  This is an FITS catalog, but
Phosphoros can read both ASCII and FITS tables. You can examine the
catalog file with your favorite tool (e.g., TOPCAT). It contains
photometry for 8 filters, including errors, and a column with
spectroscopic redshifts, which can be used for estimating the accuracy
of predicted photometric redshifts.

Filter transmissions
--------------------

Phosphoros needs the transmission curve of filters to compute modeled
photometry. You can see the available filter transmission curves by
selecting ``Configuration -> Aux. Data -> Filter Transmissions`` in
the GUI.

.. figure:: /_static/quickstart/FilterTransmissions.png
    :align: center
    :scale: 30%	    

Transmission curves are found as files in the directory
``AuxiliaryData/Filters/Quickstart``. Later on this User Manual
(:ref:`concepts_setup`) you will learn how to add your own filter
transmission curves.

Mapping filters to photometry
----------------------------------

Phosphoros needs to know also which filter corresponds to the
photometry column in the catalog file. This mapping operation is
already done for you in the quickstart data. You can see it by
selecting ``Catalog Setup`` at the navigation menu.

.. figure:: /_static/quickstart/FilterMapping.png
    :align: center
    :scale: 30%	    

Later in the User Manual, you will learn more about how to organize
your catalogs (:ref:`directory-organization`) and how to map columns
to filters (:ref:`mapping`).

For the moment, you should first select ``Quickstart`` from the
``Catalog:`` drop-down menu, then click the ``Select File and Import
Columns`` button and select your catalog file. This step needs to be
done because your home directory is different than the one in the path
stored in the quickstart data. When you finish you have to click the
``Save`` button to persist your modification.


Examining the parameter space
=============================

During the first step of the template fitting method, Phosphoros
builds the photometry for all the models which will be used for the
:math:`\chi^2` computation. A full explanation of how to define this
parameter space is out of the scope of this quickstart tutorial and it
will be explained in detail later
(:ref:`parameter-space-definition`). For the moment, to get an idea
how a parameter space looks like, you can select the ``Parameter
Space`` panel of Phosphoros, select the `Quickstart` parameter space
and click the ``Edit`` button.

.. figure:: /_static/quickstart/ParameterSpace.png
    :align: center
    :scale: 30%	    
    
This will open a window showing the axes of the parameter space. There
you can see that the `Cosmos` templates are used as SED templates, the
*calzetti* reddening law is used for the extinction with E\ :sub:`(B-V)`
in the range 0 to 2 and the redshift is computed for the range 0 to 6,
with 0.1 steps.

Building the grid of models
==============================

So far you had a look of the setup included in the quickstart
compressed file. Now you are going to use Phosphoros for running the
two steps of the template fitting. The execution of them is done at
the ``Compute Redshifts`` panel of Phosphoros.

.. figure:: /_static/quickstart/ComputeRedshifts.png
    :align: center
    :scale: 30%	    

This panel contains five collapsable sub-panels, one for each operation you can
perform with Phosphoros. The titles of these sub-panels are color-coded, so if
you have to take some action in one of them, its title will be presented in orange
letters. For example, at the moment we have not perform yet the first step of
the model fitting (computing modeled photometry), so the sub-panel
``2. Grids Generation`` is orange.

To build the grid of models you just have to click on the ``2. Grids
Generation`` label to expand the sub-panel and then click the
``(Re)-Generate the Grid`` button.  Note that when this operation will
finish, the name of the panel will turn black, indicating that you can
go on with computing your photometric redshifts.

.. note::
    
    You do not need to rebuild your modeled photometry, as long you do
    not modify your parameter space. Phosphoros will check all the
    already generated grids of models and, if you already have a
    compatible one, it will allow you to use it for computing the
    photometric redshifts.

Compute photometric redshifts
=============================

Now that you have build your models you are ready to compute your
first photometric redshifts using Phosphoros! To do that select the
``5. Input/Output Files`` in the ``Compute Redshifts`` panel.

.. figure:: /_static/quickstart/InputOutputFiles.png
    :align: center
    :scale: 30%	    

Here you can setup the input and the output parameters. Note that the
catalog included with the quickstart data is already selected as input
catalog. Moreover, Phosphoros has already set the output folder for
you. This is done based on some rules which should help you to
organize your outputs (and avoid overriding them). You can find more
details about this organization in :ref:`directory-organization`.  You
can however change the output folder to any directory you like. You
can also select the format (ASCII or FITS table) of the output catalog.

In the rest of the panel, you can select additional outputs to be
produced in the output catalog and directory (best-fit model, 1D PDFs,
multi-dimensional distributions, etc). For this tutorial you should
select as ``Output Format`` FITS and to generate the 1D PDF (of the
Likelihood or the Posterior distribution) for ``Redshift``.

.. tip::
    
    Do not select the multi-dimensional outputs, as this will result in
    the creation of very big files. These outputs are intended for
    investigating specific cases, as it is explained later in the User
    Manual (:ref:`posterior-investigation`).

To compute the photometric redshifts for your catalog you just have to press the
``Run`` button at the bottom right corner of Phosphoros and you are done!

.. _quickstart_visualize_results:

Visualizing the results
=======================

Even though the output files of Phosphoros can be handled by any
software which manages tables (like TOPCAT), Phosphoros provides some
post-processing tools to facilitate this process.

The most useful plot for visualizing your results (as long as the
input catalog does contain the spectroscopic redshift) is the
**photoZ-specZ plot**. Using this plot you can see how well Phosphoros
performed in predicting redshits.

To see the plot for your results you have to select the ``Post
Processing`` panel, and click on the ``Plot Z against Zref`` button. A
pop-up window opens where you have to provide the path for the input
catalog, and the column name of the source ID and the spectroscopic
redshift.   
        
Pressing the ``Compute`` button will open three windows, the
photoZ-specZ plot, the distribution histogram of their relative
differences and the redshift 1D PDF for a specific source (at the
beginning it will be a zero constant line because no source is
selected).

If you select a point in the photoZ-specZ plot, you will see at the
top left corner the ID of the source and its redshift 1D PDF will be
plotted in the third plot. If you double click a point, all its column
information will be printed at the terminal.

.. figure:: /_static/quickstart/SPECZ-PHZ.png
    :align: center
    :scale: 50%	    

.. note::

   These plots are standard matplotlib plots, so some default
   functionalities (like zooming, etc) are available.


Summary
=======

During this quickstart tutorial you had a first look of how the
Phosphoros GUI works. Phosphoros provides much more advanced options
for improving your photometric redshift results, which have not been
explain here. The following chapters of the User Manual will navigate
you through a more detailed description of how to use Phosphoros and
will explain in details all the advanced features, so to achieve
optimal photometric redshift estimates.
