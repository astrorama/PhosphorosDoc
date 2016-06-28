Examining the results
=====================

Even though the files produced by Phosphoros are following standardized formats
(see :ref:`output_files_format`) and can be handled by any compliant software,
Phosphoros does provides some tools to facilitate this process. This section
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
a reference redshift (most of the time the spectroscopic redshift) and a second
plot with the distribution of the (PhotoZ-SpecZ)/(1+SpecZ), which also contains
some statistics. You can see an example of these windows at the
:ref:`quickstart<quickstart_visualize_results>` section. Note that all the
plots are standard matplotlib plots, so all the default functionality (like
zooming, etc) is available.

.. warning::

    You can only use this tool to visualize results for which you have a
    reference redshift.

Selecting the catalogs
----------------------

Phosphoros does not copy in the output catalog the reference redshift. That
means that, most of the time, you will need to specify two catalog files, one
with your analysis outputs and one which contains the reference redshift. These
files can be given by using the ``--files`` (or ``-f``) parameter, followed by
the names of the files, separated by space.

By default, Phosphoros is going to match the catalog files based on a column
named ``ID``. If the ID column of your catalog files is different, you can use
the ``--id`` (of ``-i``) parameter to specify its name. This parameter gets as
arguments a list of the ID column names for the files given by the ``-f``
parameter, separated by space. Alternatively, you can give a single name, which
will be used for all files.

.. warning::

    Phosphoros will use the ID columns of the different files to match the
    catalog rows. Only rows with matching IDs in all files are going to be
    plotted by the tool.

The default columns used by Phosphoros for the photometric redshift and the
reference redshift are the ``Z`` and ``ZSPEC`` accordingly. You can modify these
names by using the ``--phz`` (or ``-p``) and ``--specz`` (or ``-s``) options to
much the ones in your catalogs.

.. tip::

    The default PHZ column name (``Z``) will plot the redshift of the best
    fitted model. If you want to see the peak of the 1D-PDF, you should pass the
    parameter ``-p 1DPDF-Peak-Z``

Interacting with the plot
-------------------------

The SpecZ-PhotoZ plot is interactive, meaning that all the plotted sources can
be clicked. Single clicking a source will highlight it and its ID will be
presented at the top left of the window. Double clicking the it will also print
at the terminal the values of all the columns for all the input files.

If during your analysis you have selected to generate also the 1D PDF file, you
can pass it to the visualization tool with the parameter ``-pdf``. In this case,
when you double click a source, its 1D PDF plot will open in a new window.

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
parameter ``--display`` (or ``-d``), which will instruct the tool to only print
the statistics on the screen and terminate directly (without opening any extra
windows). This way you can run the tool from your script and parse the stdout
stream to retrieve the statistics.