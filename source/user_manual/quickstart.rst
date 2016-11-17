.. _quickstart:

************************
Quickstart in 15 minutes
************************

If you have never used Phosphoros and you want to whet your appetite, this 15
minutes quickstart tutorial is intended just for you! It will give you a brief
overview of the Phosphoros workflow and guide you through a simple example of
computing photometric redshifts, without explaining in detail each step. At the
end of the quickstart you will have an idea how it feels like to use Phosphoros.

This quickstart assumes thet you have a functional version of Phosphoros already
installed. If you do not, before you continue, you should install Phosphoros by
following the instructions :ref:`here <phosphoros-install>`.

Getting the quickstart data
===========================

All the data required to run the quickstart are provided as a single tar.gz
file, which can be found in the :ref:`data` page. You just have to uncompress
the file in a directory called ``Phosphoros`` under your home directory. The
following commands will get the file and uncomress it::
    
    cd ~
    mkdir -p Phosphoros
    cd Phosphoros
    wget http://www.isdc.unige.ch/euclid/phosphoros/data/other/quickstart.tar.gz
    tar -xzf quickstart.tar.gz
    
.. tip:: 
    Phosphoros uses the ``~/Phosphoros`` directory as the default root directory.
    If you want to use a different directory, you can set the environment variable
    ``PHOSPHOROS_ROOT`` to the directory you want. In this case, you will have to
    uncompress the quickstart data in this directory.
    
Executing Phosphoros
====================

To start Phosphoros you need to run the command::
    
    Phosphoros GUI
    
This will open the main window with the Graphical User Interface of Phosphoros,
as seen at the following image.

.. figure:: /_static/quickstart/MainWindow.png
    :align: center

At the top part of the main window, you can see the navigation menu. This menu
is visible at all times, and it allows you to navigate through the different
Phosphoros panels.

Template fitting in two words
=============================

Phosphoros is a tool for estimating the photometric redshift using the template
fitting method. Template fitting consists of two steps. During the first step,
which has to be performed only once, we define a set of models, for which we
compute their photometries. You can imagine the models as a multi-dimensional
parameter space, with axes the SED template, the reddening and the redshift.
Each point of this parameter space contains the photometries of the model.

During the second step, we take the photometries of a source of unknown redshift
and we compare them with our models (using the :math:`\chi^2` distance). Based on
which models match better the observed photometries, we estimate the photometric
redshift.

The different options of Phosphoros for performing the above steps are explained
throught the User Manual and are not further explained during this quickstart.

The quickstart data installed previously, contain already the descriptions
required for both steps of the template fitting. The following sections will
navigate you through these settings, so you get an idea of what each step does.

Examining the input catalog
===========================

The quickstart data contain a single catalog file, `Catalogs/Quickstart/catalog.txt`.
This is an ASCII catalog, but Phosphoros can read both ASCII and FITS tables.
You can examin the catalog file with your favorite tool (topcat, vi, etc). It
contains photometries for 8 filters and a column with the spectroscopic redshift,
which can be used for estimating the accuracy of the predicted photometric
redshift.

Filter transmissions
--------------------

The first thing Phosphoros needs, are the transmission curves of the filters, so
it can compute the photometries of the models. You can see these photometries
by selecting `Configuration -> Aux. Data -> Filter Transmissions` in the GUI.

.. figure:: /_static/quickstart/FilterTransmissions.png
    :align: center

If you want to further investigate the filter transmissions, you can find them
as files in the directory `AuxiliaryData/Filters/Quickstart`. Later on this
User Manual (:ref:`setup-input-data`) you are going to learn how to add your own
filter transmissions.

Catalog column mapping
----------------------

The second thing Phosphoros needs to know about the table, is which column in
the catalog file maps to the photometry of each filter. This mapping is also
already done for you in the quickstart data. You can see it by selecting 
`Catalog Types` at the navigation menu.

.. figure:: /_static/quickstart/FilterMapping.png
    :align: center

Later in this User Manual, you will learn more about how to organize your catalogs 
(:ref:`directory-organization`) and how to map the columns to the filters
(:ref:`catalog-column-mapping`).

For the moment, you should first click the `Edit` button, then click the
`Select File and Import Columns` and select your catalog file. This step needs
to be done because your home directory is different than the one in the path
stored in the quickstart data. When you finish you have to click the `Save`
button to persist your modification.

Examining the parameter space
=============================

As explained earlier, during the first step of the template fitting, Phosphoros
is going to build the photometries for all the models which will be used for
the :math:`\chi^2` computation. A full explanation of how to define this
parameter space is out of the scope of this quickstart tutorial and it will be
explained in detail later (:ref:`parameter-space-definition`). For the moment,
to get an idea how this parameter looks like, you can select the `Parameter Spaces`
panel of Phosphoros, highlight the `Quickstart Parameter Space` and click the
`Open` button.

.. figure:: /_static/quickstart/ParameterSpace.png
    :align: center
    
This will open a window showing the axes of the parameter space. There you can
see that the Cosmos templates are used as templates, the calzetti reddening law
is used for the extinction with E\ :sub:`(B-V)` in the range 0 to 2 and the
redshift is computed for the range 0 to 6, with 0.1 steps.

Building the models
===================

At the previous steps you had a look of the setup included in the quickstart
compressed file. Now you are going to use Phosphoros for running the two steps
of the template fitting. The execution of all these steps is done at the
`Compute Redshifts` panel of Phosphoros.

.. figure:: /_static/quickstart/ComputeRedshifts.png
    :align: center

This panel contains four collapsable sub-panels, one for each operation you can
perform with Phosphoros. The titles of these sub-panels are color-coded, so if
you have to take some action in one of them, its tile will be presented in orange
letters. For example, at the moment we have not perform yet the first step of
the model fitting (computing the photometries of our models), so the sub-panel
`1. Model Grid Generation / Selection` is orange (because we can not compute
photometric redshifts for our catalog without performing this step first).

To build the models you just have to click on the `1. Model Grid Generation / Selection`
label to expand the sub-panel and then click the `(Re)-Generate the Grid` button.
Note that when this operation will finish, the name of the panel will turn black,
indicating that you can go on with computing your photometric redshifts.

.. tip::
    
    You do not need to rebuild your model photometries, as long you do not modify
    your models parameter space. Phosphoros will check all already generated models
    and, if you already have a compatible one, it will allow you to use it for
    computing the photometric redshifts.

Compute the redshifts
=====================

Now that you have build your models you are ready to compute your first photometric
redshifts using Phosphoros! To do that select the `4. Input/Output Files` in the
`Compute Redshifts` panel.

.. figure:: /_static/quickstart/InputOutputFiles.png
    :align: center

Here you can setup the input and the output parameters. Note that the
catalog.txt file included with the quickstart data is already selected as the
input catalog, but you can select any ASCII or FITS table which contains the
same column names.

As you can notice, Phosphoros has already set the output folder for you. This is
done based on some rules for helping you to organize your outputs (and avoid
overriding them). You can find more details about this organization :ref:`here <directory-organization>`.
Note that you can change the output folder to any directory you like.

On the rest of the panel, you can select the types of outputs you want to be
produced  in the output directory (best fitted model, 1D PDFs, multi-dimensional
posterior, etc). For this tutorial you should select to generate the output
catalog in FITS format and to generate the 1D PDF for the Redshift.

.. tip::
    
    Do not select the likelihood or posterior outputs, as this will result to the
    creation of very big files. These outputs are intended for investigating
    specific cases, as it is explained later in the User Manual (:ref:`posterior-investigation`).

To compute the photometric redshifts for your catalog you just have to press the
`Run` button at the bottom right corner of Phosphoros and you are done!

.. _quickstart_visualize_results:

Visualizing the results
=======================

Even though the output files of Phosphoros can be handled by any software which
manages tables (like topcat), Phosphoros provides some post-processing tools to
facilitate this process.

.. warning::
    
    At the moment, the post-processing tools of Phosphoros are available only
    via the command line. You will have to close the Phosphoros GUI and open a
    terminal at your Phosphoros root directory to continue the quickstart.
    
The most useful plot for visualizing your results (from the moment the input
catalog does contain the spectroscopic redshift) is the SPECZ-PHZ plot. Using
this plot you can see how well Phosphoros performed with predicting the results.
To see this plot for your results you will have to execute the command::
    
    Phosphoros PSC -scat Catalogs/Quickstart/catalog.txt -pod Results/Quickstart/catalog
    
.. tip::
    
    To see a full list of the options and what they mean you can run::
        
        Phosphoros PSC --help
        
This will open three windows, the SPECZ-PHZ plot, the distribution histogram and
the redshift 1D PDF.

.. figure:: /_static/quickstart/SPECZ-PHZ.png
    :align: center

.. figure:: /_static/quickstart/Histogram.png
    :align: center
    
These plots are standard matplotlib plots, so some default functionalities (like
zooming, etc) are available. If you select a point, you will see at the top left
corner the ID of the source it represents and its redshift 1D PDF will be plotted.
If you double click a point, all its column information will be printed at the
terminal.

.. figure:: /_static/quickstart/PDF.png
    :align: center
    
.. tip::
    
    If you use topcat, you can launch it in advance and add the option `-samp`
    at the `Phosphoros PSC`. This will automatically load the related tables
    into topcat and it will allow for cross-selection of the sources between the
    two softwares.

Summary
=======

During this quickstart tutorial you had a first look of how working with Phosphoros
feels like. Phosphoros provides much more advanced options for improving your
photometric redshift results, which have not been explain here. The following
chapters of the User Manual will navigate you through a more detailed description
of how to use Phosphoros and will explain in details all the advanced features,
so you can achieve optimal photometric redshift estimations.