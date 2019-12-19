Examining results
=====================

Output files produced by Phosphoros follow standardized formats (see
the :ref:`output_files_format` section) and can be handled by any
compliant software. Nevertheless, Phosphoros provides some tools to
facilitate the process of analysis and visualization of results. In
particular:


**(Statistics)** Phosphoros can compute the following statistical
information on the **redshift PDF** of sources in the
output catalog:

- Median

- Confidence interval at 70, 90 and 95%. This is computed

  1. centering the confidence interval at the peak of the distribution;
     
  2. taking the confidence interval with the minimum length.
     
- For the first two modes of the distribution, the tool finds the
  best-fitting Gaussian function and computes
  
  1. the sampled redshift with the highest probability;

  2. the mean of the fitted distribution;

  3. the redshift at the peak of the fitted distribution;

  4. the area below the fitted distribution.


**(Visualization)** Phosphoros can display the following plots:

- ``Figure 1`` A plot comparing the photometric redshifts
  (:math:`photoZ`) estimated by Phosphoros with the reference
  redshifts (:math:`specZ`; typically spectroscopic redshifts)
  provided by users. Below it, a plot with the relative difference
  :math:`(photoZ-specZ)/(1+specZ)` as a function of :math:`specZ`;

- ``Figure 2`` The distribution of the relative difference
  :math:`(photoZ-specZ)/(1+specZ)`. Some basic statistics are computed
  and shown in the plot.

- ``Figure 3`` The ``Figure 1`` plot is interactive, and allows
  users to examine the redshift PDF of sources in the plot. By a
  single click on a source, its ID will be presented at the top left
  of the window and the 1D PDF will be plotted in a separated
  window. Double clicking will perform the same and it will also print
  at the terminal the source values at all the columns in the input
  and output catalogs. *(it doesn't work now)*


.. figure:: /_static/quickstart/SPECZ-PHZ.png
   :align: center
   :scale: 60 %

  
.. warning::

    The tool to visualize results can be used only for those catalogs
    for which reference redshifts are known.

.. note::

   All these plots are standard matplotlib plots and come with a
   navigator toolbar, making available default functionalities like
   zooming, etc.

.. note::

    Phosphoros also provides a tool for visualizing multi-dimensional
    likelihoods and posterior distributions. At the moment, it is
    available only in the CLI. The description of the tool is out of
    the scope of the *Basic Steps* chapter. We refer the reader to the
    :ref:`posterior-investigation` section.

    
How to use these tools with the GUI and the CLI is the topic of the
following sections.
    
Examining results with the GUI
------------------------------------

The tools to examine results are provided by the GUI in the ``Post
Processing`` panel.

Select the catalog type of results to be analyzed (through the
``Results for Catalog`` drop-down menu). All the folders belonging to
that catalog type and present in the database will appear in the
``List of processing results``.

- Clicking on ``PDF stat`` opens a window with the list of statistics
  to be computed. By default, all the possible statistics are
  selected. Users can deselect those that are not of interest.

  The ``Compute`` tab at the bottom runs the process. The results are
  then save in a .FITS file named ``Z-1D-PDF_Statistic.fits`` and
  located in the same directory as the output catalog::

    > $PHOSPHOROS_ROOT/Results/<Catalog Type>/<Catalog File Name>/

.. figure:: /_static/basic_steps/Post_processing.png
   :align: center
   :scale: 60 %
	   

- Clicking on ``Plot Z against Zref`` opens a window with the required
  action parameters.

  Both the best-fit redshift or the redshift at the 1D PDF peak can be
  used for comparison with the reference redshift (see the ``Redshift
  Column`` drop down menu).

  Users must select the file where reference redshifts are found,
  the column names of the source ID and of the reference redshift
  (see ``Reference Redshift Catalog``).

  Again the ``Compute`` tab at the bottom runs the process and three
  windows with the above plots open. Moreover, some statistics on the
  :math:`(photoZ-specZ)/(1+specZ)` distribution are shown (see the
  figure below).

.. figure:: /_static/basic_steps/Plot_Z_against_Zref.png
   :align: center
   :scale: 70 %
  
.. note::

   For very large catalogs, there is the option to not display
   plots. Phosphoros will only print the computed statistics.

Examining results with the CLI
------------------------------------

In the CLI two different actions are present in order to compute
statistical information and to visualize results.

**Statistics**

The ``process_output_pdz`` (or ``POP``) action calls the ProcessPDF
C++ executable and extracts from the redshift PDFs of the output catalog
the statistical information described above.

Users have to provide the qualified name of the output catalog by the
``--input-cat`` action parameter, for example, as::

  > Phosphoros POP --input-cat=Phosphoros/Results/<Catalog Type>/<Catalog File Name>/phz_cat.fits

The name and the location of the output file (by default ``out.fits``
and located in the ouput catalog directory) can be set by the
``--output-cat`` option.

Some statistical information can be excluded by the output by the
``--excluded-output-columns`` option.

See the full list of options with the usual ``--help`` generic action
parameter.

**Visualization**

The Phosphoros tool for visualizing results can be launched by using
the ``plot_specz_comparison`` action (or the shortcut ``PSC``).

Users have to provide the directory containing the Phosphoros results
by using the ``--phosphoros-output-dir`` (or ``-pod``) parameter. The
tool itself will automatically detect all the available results in the
directory (like 1D PDFs) and it will handle all the possible output
formats.

.. note::

   By default, the tool plots the redshift of the best-fit model,
   i.e. column named ``Z`` in the output catalog. If users want to
   use the redshift corresponding to the peak of the 1D-PDF, they
   should pass the option ``-pcol=1DPDF-Peak-Z``.

.. warning::

    If users have leftover results from previous executions (e.g., 1D
    PDFs in separate files), the tool will not recognize that they are
    belonging to a different run. In this case the directory should be
    cleaned before runnning the analysis.

Phosphoros does not copy the reference redshift in the output
catalog. That means that users need to specify the catalog file
which contains the reference redshift.  This is done by using the
following options:

* ``--specz-catalog`` (or ``-scat``) for the catalog file name, in FITS
  or ASCII format.
  
* ``--specz-cat-id`` (or ``-sid``) for the name of the column that
  contains the source ID (default: ``ID``)
  
* ``--specz-column`` (or ``-scol``) for the name of the column that
  contains the reference redshift (default: ``ZSPEC``).

.. warning::

    Phosphoros will use the source ID columns to match the catalog
    rows of the different files. Only rows with matching IDs in all
    files are plotted by the tool.

.. warning::

   By default, the PSC tool opens new windows and it
   does not terminate until the windows are closed. The
   tool is therefore unusable in scripts. If users want to use the
   tool in a script, they can simply pass the ``--no-display`` (or ``-nd``)
   parameter, which will instruct the tool to only print the
   statistics on the screen and terminate directly after, without
   opening any extra windows. In this way, the tool can be run from
   a script and the standard output streams be parsed to retrieve the
   statistics.


See the full list of options with the usual ``--help`` generic action
parameter.


Connecting with TOPCAT
-------------------------------------

The Phosphoros ``plot_specz_comparison`` (or ``PSC``) tool is SAMP
[#f2]_ enabled and it can communicate with a TOPCAT instance. You can
enable this functionality by using the parameter ``-samp`` (or launch
the GUI using ``Phosphoros GUI -samp``).  In this case, Phosphoros
will search for the first instance of TOPCAT and it will open in it
the related catalogs. From that moment on, all the selections on the
plot will be forwarded to TOPCAT and the corresponding rows will be
highlighted. The interaction is bidirectional, meaning that if you
select a row in TOPCAT, the source will be highlighted in the plot.
*(tbc)*


.. tip

    For TOPCAT to broadcast the row selection you have to check the ``Broadcast
    Row`` box:

.. image:: /_static/first_step/TopcatBroadcastRow.png
    :align: center
    :scale: 50 %

.. note::

   If multiple instances of the Phosphoros PSC tool are launched with
   the SAMP functionality enabled (and connected to the same TOPCAT
   instance), all selections will be reflected to all the plot
   windows.
   
.. note::

   If you are using DockerPhosphoros, TOPCAT has to be launched from
   the Docker container.


   
.. rubric :: Footnotes

.. [#f2] SAMP, the Simple Application Messaging Protocol, is a
	 messaging protocol that enables astronomy software tools to
	 interoperate and communicate (see, e.g., arXiv:1501.01139).

