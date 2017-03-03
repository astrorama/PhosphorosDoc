Examining the results
=====================

Even though the files produced by Phosphoros are following standardized formats
(see :ref:`output_files_format`) and can be handled by any compliant software,
Phosphoros does provide some tools to facilitate this process. This section
explains how to use these tools. Note that at the moment, these tools are only
available via the |CLI|.

.. note::

    Phosphoros does provides a tool for visualizing the multi-dimensional
    likelihood and posterior. The description of this tool is out of the scope
    of this *First Steps* part of the user manual. To learn how to use this tool
    refer to the :ref:`posterior-investigation` section.

``plot_specz_comparison`` action
--------------------------------

The Phosphoros tool for visualizing the results can be launched by using the
action ``plot_specz_comparison`` or the shortcut ``PSC``. To see a full list of
the available options you can run::

    Phosphoros PSC --help

By default, this tool will show a plot of the Phosphoros predicted redshift over
a reference redshift (most of the time the spectroscopic redshift), a plot with
the distribution of the (PhotoZ-SpecZ)/(1+SpecZ), which also contains some
statistics, and plots for any 1D PDF outputs. You can see an example of these
windows at the :ref:`quickstart<quickstart_visualize_results>` section. Note
that all the plots are standard matplotlib plots, so all the default
functionality (like zooming, etc) is available.

.. warning::

    You can only use this tool to visualize results for which you have a
    reference redshift.

Specifying the Phosphoros results to plot
-----------------------------------------

The PSC tool gets the directory containing the Phosphoros results by using the
``--phosphoros-output-dir`` (or ``-pod``) parameter. The tool itself will
automatically detect all the available results in the directory (like 1D PDFs)
and it will handle all the possible output formats. Note that the default PHZ
column name (``Z``) will plot the redshift of the best fitted model. If you want
to see the peak of the 1D-PDF, you should pass the parameter ``-p 1DPDF-Peak-Z``.

.. warning::

    If you have leftover results from a previous execution (for example 1D PDFs
    in separate files), the tool will not recognize that they are belonging to
    a different run. In this case you should clean the directory before you run
    your analysis.

Specifying the reference redshift
---------------------------------

Phosphoros does not copy in the output catalog the reference redshift. That
means that you will need to specify the catalog file which contains the reference redshift.
This is done by using the following options:

* ``--specz-catalog`` (or ``-scat``) : The catalog file (in FITS or ASCII format)
* ``--specz-cat-id`` (of ``-sid``) : The name of the column that contains the IDs
  (default: ``ID``)
* ``--specz-column`` (of ``-scol``) : The name of the column that contains the
  reference redshift (default: ``ZSPEC``)

.. warning::

    Phosphoros will use the ID columns of the different files to match the
    catalog rows. Only rows with matching IDs in all files are going to be
    plotted by the tool.

Interacting with the plot
-------------------------

The SpecZ-PhotoZ plot is interactive, meaning that all the plotted sources can
be clicked. Single clicking a source will highlight it, its ID will be presented
at the top left of the window and the 1D PDF plots will be updated. Double
clicking will perform all the above and it will also print at the terminal the
values of all the columns for all the input files.

.. _connecting-with-topcat:

Connecting with TOPCAT
----------------------

Phosphoros PSC tool is SAMP enabled and it can communicate with a TOPCAT
instance. You can enable this functionality by using the parameter ``-samp``.
In this case, Phosphoros will search for the first instance of TOPCAT and it
will open in it the related catalogs. From that moment on, all the selections
on the plot will be forwarded to TOPCAT and the corresponding rows will be
highlighted. The interaction is bidirectional, meaning that if you select a row
in TOPCAT, the source will be highlighted in the plot.

.. tip::

    For TOPCAT to broadcast the row selection you have to check the Broadcast
    Row box:

    .. image:: /_static/first_step/TopcatBroadcastRow.png
       :align: center
       :scale: 50 %

Note that if you launch multiple instances of the Phosphoros PSC tool with the
SAMP functionality enabled (and they are connected to the same TOPCAT instance),
all selections will be reflected to all the plot windows.

Usage in scripts
----------------

The default behavior of the Phosphoros PSC tool renders it unusable in scripts,
because it shows the windows with the plots and it does not terminate until the
windows are closed. If you want to use the tool in a script, you can give the
parameter ``--no-display`` (or ``-nd``), which will instruct the tool to only print
the statistics on the screen and terminate directly (without opening any extra
windows). This way you can run the tool from your script and parse the stdout
stream to retrieve the statistics.