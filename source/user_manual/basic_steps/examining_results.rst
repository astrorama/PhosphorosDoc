Examining results
=====================

Output files produced by Phosphoros follow standardized formats (see
the :ref:`Results Format<result_files_format>` section) and can be handled by any
compliant software. Nevertheless, Phosphoros provides some tools to
facilitate the process of analysis and visualization of results. In
particular:

.. _statistics:

Statistical Analysis
-------------------------
     
Phosphoros can compute the following statistical information on the
**redshift PDF** of sources in the output catalog (in brackets, we
report the name used in Phosphoros to identify the redshift point
estimators; see below):

- Median (``Z-1D-PDF_Statistic-MEDIAN``)

- Confidence interval at 70, 90 and 95%. This is computed 

  1. centering the confidence interval on the mean of the distribution;
     
  2. taking the confidence interval with the minimum length.

  |br|     
  
- For the first two modes of the distribution, the tool finds the
  best-fitting Gaussian function and computes
  
  1. the sampled redshift with the highest probability
     (``Z-1D-PDF_Statistic-PHZ_MODE_1_SAMP`` or
     ``Z-1D-PDF_Statistic-PHZ_MODE_2_SAMP``); 

  2. the mean of the fitted distribution
     (``Z-1D-PDF_Statistic-PHZ_MODE_1_MEAN`` or
     ``Z-1D-PDF_Statistic-PHZ_MODE_2_MEAN``); 

  3. the redshift at the peak of the fitted distribution
     (``Z-1D-PDF_Statistic-PHZ_MODE_1_FIT`` or
     ``Z-1D-PDF_Statistic-PHZ_MODE_2_FIT``); 

  4. the area below the fitted distribution.

.. warning::

   The analysis can only be performed if output catalogs contain the
   redshift PDFs of sources (see the :ref:`computing-redshifts`
   section).
   

Statistical analysis with the GUI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``Post Processing`` panel in the GUI allows users to apply
Phosphoros tools for the statistical analysis of output catalogs (see
:numref:`bpostpro`).

Select the catalog type of results to be analyzed (through the
``Results for Catalog`` drop-down menu). All the folders belonging to
that catalog type and present in the database (below the directory
``$PHOSPHOROS_ROOT/Results/<Catalog Type>``) will appear in the ``List
of processing result``.

.. figure:: /_static/basic_steps/Post_processing.png
    :name: bpostpro 
    :align: center
    :scale: 50 %
	   
    Example of the ``Post Processing`` panel in the GUI	   

Clicking on ``PDF stat`` opens a window with the list of statistics
that can be computed (see :numref:`bstat`). By default, all the
possible statistics are selected. Users can deselect those that are
not of interest.

The ``Compute`` tab at the bottom runs the process. The results are
then save in a .FITS file named ``Z-1D-PDF_Statistic.fits`` and
located in the same directory as the output catalog::

  > $PHOSPHOROS_ROOT/Results/<Catalog Type>/<Catalog File Name>/

.. figure:: /_static/basic_steps/Post_proc_stat.png
    :name: bstat
    :align: center
    :scale: 60 %
	    
    Window of the GUI with the statistical information that Phosphoros can compute
	    
	   
Statistical analysis with the CLI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``process_output_pdz`` (or ``POP``) action performs the
statistical analysis of output catalogs. It calls the ProcessPDF C++
executable and extracts from the redshift PDFs of output catalogs
the statistical information described above.

Users have to provide the qualified name of the output catalog by the
``--input-cat`` action parameter. For example::

  > Phosphoros POP --input-cat=$PHOSPHOROS_ROOT/Results/<Catalog Type>/<Catalog File Name>/phz_cat.fits

The name and the location of the output file (by default ``out.fits``
and located in the same directory as the ouput catalog) can be set by
the ``--output-cat`` option.

The computation of some statistical information can be excluded by the
``--excluded-output-columns`` option.

See the full list of options with the usual ``--help`` action
parameter.


Visualization
-----------------------

Phosphoros provides tools for the visualization of results. In
particular, the following plots can be produced:

- A plot comparing photometric redshifts (:math:`photoZ`) estimated by
  Phosphoros with reference redshifts (:math:`specZ`) provided by
  users. Below that, a plot with their relative difference,
  :math:`(photoZ-specZ)/(1+specZ)`, as a function of :math:`specZ` is
  also shown (left plot in :numref:`bvis1`). Users can choose among
  different point estimators of the photometric redshift
  :math:`photoZ` (like the redshift of the best-fit model, the peak of
  the redshift PDF, and the statical estimators described above in the
  :ref:`statistics` sub-section). Colors in the :math:`photoZ\,{\rm
  vs}\,specZ` plot are associated to the number density of objects,
  blue at the lowest density and dark red at the highest density.

- The histogram of the relative difference
  :math:`(photoZ-specZ)/(1+specZ)`. Some basic statistics are computed
  and shown in the plot (right-top plot in :numref:`bvis1`).

- The :math:`photoZ\,{\rm vs}\,specZ` plot is interactive, and allows
  users to examine the 1D PDF of model parameters for the sources in
  the plot (right-bottom plots in :numref:`bvis1`). By a single **click
  on a source**, in fact, its ID will be presented at the top left of
  the window and all the 1D PDFs that have been computed will be
  displayed in separated windows, up to eight plots (i.e., the PDF of
  *z*, SED, :math:`E_{B-V}` and reddening curve for both the
  likelihood and posterior distribution). 
  
.. figure:: /_static/quickstart/SPECZ-PHZ_v018.png
   :name: bvis1
   :align: center
   :scale: 60 %

   *(left)* Photometric vs Reference redshifts and their relative
   difference; *(right-top)* distribution of the relative difference;
   *(right-bottom)* the redshift and :math:`E(B-V)` PDF of the
   selected source in the *left* plot.

.. figure:: /_static/basic_steps/stacked_PHZ.png
   :name: bvis2
   :align: center
   :scale: 60 %
	   
   *(right-top)* Density scatter plot of the stacked PDFs in :math:`specZ`
   bins; *(left-top)* number sources in :math:`specZ` bins;
   *(right-bottom)* bias per :math:`specZ` bin; *(left-bottom)*
   fraction of the stacked PDF around its mean value per :math:`specZ`
   bin.
	   
- A density scatter plot obtained by stacking the redshift PDFs of
  input sources in reference redshift (:math:`specZ`) bins. The
  contour level at 90% and 68% of the stacked PDFs are also plotted
  (left-top plot in :numref:`bvis2`).

- The histogram of the number of sources per :math:`specZ` bin
  (right-top plot in :numref:`bvis2`).

- The bias of the stacked PDFs with respect to the reference redshifts
  per :math:`specZ` bin (left-bottom plot in :numref:`bvis2`). In the plot,
  the bias is computed as difference between the mean of the stacked
  PDF with the bin center. However, the bias can be also computed
  using the maximum (``MAX``), the median (``MED``) or the fit
  (``FIT``) [#f1ex]_ of the stacked PDFs.

- The fractions of the stacked PDFs enclosed in a :math:`0.05(1+z)`
  interval (``F005``) or in a :math:`0.15(1+z)` interval (``F015``)
  around the mean of the stacked PDF per :math:`specZ` bin (where
  :math:`z` is the center of the bin). As for the bias, the mean can
  be replaced with the median, the maximum or the fit of the
  stacked PDF (right-bottom plot in :numref:`bvis2`).

.. note::

   Similar plots as in :numref:`bvis2` can be also generated for
   *shifted redshift PDFs*. For each input source, the shifted PDF is
   obtained by traslating the PDF to have the reference redshift as
   origin. Again, shifted PDFs are then stacked in redshift bins. In
   the ideal case, the density scatter should be centered in zero at
   all redshifts.

.. figure:: /_static/basic_steps/PIT_PHZ.png
   :name: bvis3 
   :align: center
   :scale: 60 %
	   
   *(left)* PIT plot; *(right)* distribution of the CRPS.

In order to assess the performance and the quality of the predicted
redshfit PDFs in the output catalog, the following two plots can be
also useful (see, e.g., :cite:`Her00`; :cite:`Dis18`):

- The Probability Integral Transform (PIT) plot of the redshift
  PDFs (Left plot in :numref:`bvis3`).

- The distribution of the Continuous Ranked Probability Score (CRPS)
  of the redshift PDFs (Right plot in :numref:`bvis3`).
   
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

	 
.. figure:: /_static/basic_steps/Post_proc_plot.png
   :name: bvis4
   :align: center
   :scale: 70 %
	   
   ``Post Processing`` window of the GUI for the visualization of results

    
Visualization with the GUI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``Post Processing`` panel in the GUI allows users to apply
Phosphoros tools for the visualization of results. Clicking on
``Plots`` opens a window with the required action parameters (see
:numref:`bvis4`).

By the ``Point Estimate Redshift Column`` drop-down menu, different
point estimators of the photometric redshift can be selected for the
comparison with the reference redshift. They are the redshift
associated with the best-fit model (``Z``), the
peak of the redshift PDF (``1DPDF-Peak-Z``), and all the statistical
estimators described in the above :ref:`statistics` sub-section.

In ``Reference Redshift Catalog``, users have to select the file where
reference redshifts are found, the column name of the source ID and
of the reference redshift. However, if a reference redshift column in
the input catalog has been provided in the ``Catalog Setup`` panel
(see the :ref:`Catalog Setup <mapping>` section), Phosphoros will
automatically fill these fields.

In ``Option`` users can decide which plots to produce. Clicking on
``Point estimate scatter plot and stat.``, Phosphoros will display the
plots shown in :numref:`bvis1`. For very large catalogs, there is the
option to not display any plots. Phosphoros will only print basic
statistics for the :math:`(photoZ-specZ)/(1+specZ)` distribution.

Clicking on ``Stacked PDF, PIT and CRPS plots``, users can manually
select the plots to display (see :numref:`bvis2` and :numref:`bvis3`
above). By default, all plots but the ``PIT`` and ``CRPS`` ones are
selected. Moreover, plot parameters -- such as the number of redshift
bins, of histogram bins and the method for the redshift estimate --
can be choosen using the corresponding drop-down menus.

The ``Compute`` tab at the bottom runs the process and a window per
plot opens.

Visualization with the CLI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Two different actions are defined for visualization purposes: the
``plot_specz_comparison`` (or ``PSC``) action for the plots in
:numref:`bvis1` and the ``plot_stacked_pdz`` (or ``PSP``) action for the
plots in :numref:`bvis2` and :numref:`bvis3`.

**The PSC action**

Users have to provide the directory containing the Phosphoros results
by using the ``--phosphoros-output-dir`` (or ``-pod``) parameter. The
tool itself will automatically detect all the available results in the
directory (like 1D PDFs) and it will handle all the possible output
formats.

.. note::

   By default, the tool plots the redshift of the best-fit model,
   i.e. column named ``Z`` in the output catalog. If users want to use
   a different redshift estimator, they should pass the option
   ``-pcol=<PHZ column name>``. For example, for the redshift
   corresponding to the peak of the 1D-PDF, the option is
   ``-pcol=1DPDF-Peak-Z``.     

.. warning::

    If users have leftover results from previous executions (e.g., 1D
    PDFs in separate files), the tool will not recognize that they are
    belonging to a different run. Therefore the directory should be
    cleaned before runnning the analysis.

Phosphoros does not copy the reference redshifts in the output
catalog. That means that users need to specify the catalog file
which contains the reference redshifts. This is done by using the
following options:

* ``--specz-catalog=`` (or ``-scat=``) the catalog file name, in FITS
  or ASCII format.
  
* ``--specz-cat-id=`` (or ``-sid=``) the name of the column that
  contains the source ID (default: ``ID``)
  
* ``--specz-column=`` (or ``-scol=``) the name of the column that
  contains the reference redshift (default: ``ZSPEC``).

.. warning::

    Phosphoros will use the source ID columns to match the catalog
    rows of different files. Only rows with matching IDs in all files
    are plotted by the tool.

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


See the full list of options with the usual ``--help`` action
parameter. Configuration files can be used through the
``--config-file`` option.


**The PSP action**

Users have to provide the qualified name of the output catalog (in
FITS format) containing the redshift PDFs through the
``--pdz-catalog-file`` option. The name of the relevant columns inside
this file can be specified by the following options:

* ``--pdz-col-id=`` the name of the column that
  contains the source ID (default: ``ID``).
  
* ``--pdz-col-pdf=`` the name of the column containing the redshift
  PDF (default: ``Z-1D-PDF``).
  
* ``--pdz-col-pe=`` the name of the column containing the redshift
  estimator. This can be the redshift of best-fit model, ``Z``,  or the redshift
  at the peak of the PDF, ``1DPDF-Peak-Z``, or one of the redshift
  estimators discussed in the :ref:`statistics` sub-section (default:
  ``Z``).

.. warning::

   The ``PSP`` action is not enabled when output catalogs are in ASCII format
   or the redshift PDFs are saved in a separated file.
  
Similarly, there are action parameters for the file containing the
reference redshifts:

* ``--refz-catalog-file=`` the qualified name of the catalog file
  including the reference redshifts, in FITS format. If not specified,
  Phosphoros will look for reference redshifts into the file defined
  by the ``--pdz-catalog-file`` option.
  
* ``--refz-col-id=`` the name of the column that contains the source
  ID (default: ``ID``).
  
* ``--refz-col-ref=`` the name of the column that contains the
  reference redshifts (default: ``Z-TRUE``).
  
.. warning::

    Phosphoros will use the source ID columns to match the catalog
    rows of the different files. Only rows with matching IDs in all
    files are plotted by the tool.


The following action parameters concern how to produce the plots:
  
* ``--stack-bins=`` the number of redshift bins for the stacking of
  the PDFs (default: ``20``).

* ``--hist-bins=`` the number of bins for the histograms in the
  PIT and CRPS plots (default: ``20``).
  
* ``--stacked-point-estimate=`` the type of redshift estimate
  computed from the stacked PDFs. Options are ``MAX``, ``FIT``,
  ``MEAN`` and ``MED`` (default: ``MEAN``).

By default, all possible plots will be displayed. In order to disable one
of them, it is enough to set the ``<name>-plot`` option to ``False``,
where the ``<name>`` of each plot can be found with the
usual ``--help`` option. For example, setting ``--ref-bias-plot=False`` will
disable the *bias per redshift bin* plot.


.. _connecting-with-topcat:

Connecting with TOPCAT
-------------------------------------

The Phosphoros ``plot_specz_comparison`` (or ``PSC``) tool is SAMP
[#f2]_ enabled and it can communicate with a TOPCAT instance. You can
enable this functionality by using the parameter ``-samp``.  In this
case, Phosphoros will search for the first instance of TOPCAT and it
will open in it the related catalogs (see :numref:`btopcat`). From
that moment on, all the selections on the plot will be forwarded to
TOPCAT and the corresponding rows will be highlighted. The interaction
is bidirectional, meaning that if you select a row in TOPCAT, the
source will be highlighted in the plot.

.. tip

    For TOPCAT to broadcast the row selection you have to check the ``Broadcast
    Row`` box:

.. figure:: /_static/first_step/TopcatBroadcastRow_v12.png
    :name: btopcat
    :align: center
    :scale: 70 %
	    
    TOPCAT window
	    
.. note::

   If multiple instances of the Phosphoros PSC tool are launched with
   the SAMP functionality enabled (and connected to the same TOPCAT
   instance), all selections will be reflected to all the plot
   windows.
   
.. note

.. If you are using DockerPhosphoros, TOPCAT has to be launched from
   the Docker container.


   
.. rubric :: Footnotes

.. [#f1ex] In the ``FIT`` case, the redshift estimate is computed by
	   fitting the maximum of the stacked PDF by a parabolic
	   function and taking its maximum. This is similar to the
	   ``MAX`` estimate but more precise.

.. [#f2] SAMP, the Simple Application Messaging Protocol, is a
	 messaging protocol that enables astronomy software tools to
	 interoperate and communicate (see, e.g., arXiv:1501.01139).

.. bibliography:: references_basic_res.bib 
