.. _zero-point-correction:

Photometric Zero-Point Corrections
=====================================

Photometric zero-point corrections aim to correct flux measurements by
band-specific photometric offsets, which can lead to systematic shifts
in the observed colors, and consequently in the redshift
estimates. These corrections take into account remaining zero-point
calibration issues in one or more bands, and also potential mismathces
between galaxy colours and the templates used to model them.

In order to derive zero-point corrections, Phosphoros needs a training
sample of the input catalog with (spectroscopic) redshift
measurements. The process to derive the zero-point corrections is
iterative. For each source of the training sample, Phosphoros
determines the best model (with respect to the observed fluxes) fixing
the redshift, and estimates in each band the offset in terms of the
ratio between the predicted and observed flux,
:math:`r=f_m^i/f^i_{obs}`. The overall offset of each band is then
computed by *averaging* over all the sample sources. Offsets are
finally used to correct photometric measurements
(:math:`f^i_{obs}\rightarrow \langle r\rangle f^i_{obs}`). The process
is repeated until reaching a tolerance threshold for the discrepancies
or a maximum number of iterations.

The *averaging* methods to compute the band overall offsets are the
following: mean, weighted mean, median (default option) and weighted
median. The weighted mean/median use the signal-to-noise value in the
band as weight.


Photometric Corrections in the GUI
------------------------------------------------

Zero-point corrections can be computed in the sub-panel
``4. Photometric Zero-Point Corrections`` of the ``Compute Redshifts``
window.

Select the ``Enable Photometric Zero-point Corrections`` tab. Users
compute zero-point corrections by clicking on the ``Compute New
Corrections`` button. A pop-up window will appear, as shown below.

.. figure:: /_static/advanced_steps/zero_point.png
    :align: center
    :width: 800px
    :height: 400px
..    :scale: 30 %

The top of the window displays the configuration (catalog type,
parameter space, filters, etc.)  that will be taken into account for
the zero-point correction. Three further steps are required:

- ``1. Traning Catalog Selection:`` select the training sample
  through the ``Browse`` tab, along with the column name where the
  spectroscopic redshifts are stored.

- ``2. Algorithm:`` define the algorithm parameters (i.e.  the
  ``Number of Iterations`` and the ``Tolerance`` thresold, used to
  decide when algorithm interations are stopped) and the ``Selection
  Method`` (i.e. the method to compute the overall band corrections).

- ``3. Run:``, choose the file to export zero-point corrections. The
  recommended (default) name is ``<Parameter Space>_<Method>``. The
  file is in ASCII format, with the ``.txt`` extension. In the
  standard configuration, it is stored in the directory::

    > $PHOSPHOROS_ROOT/IntermediateProducts/<Catalog Type>/
  
Users can select already existing files with zero-point correction
values through the drop down menu below ``Selecting Exisiting
Compatible Corrections``, and visualize or modify them through the
``View / Edit Corrections`` button.
  
Photometric Corrections in the CLI
------------------------------------------------

Zero-point corrections can be computed using the Phosphoros action
``compute_photometric_corrections`` (or ``CPC``).

The main parameters of this action concern the input training catalog
and the algorithm configuration parameters. An example of them is::

  --input-catalog-file=<file name>
  --spec-z-column-name=<column name>
  --spec-z-err-column-name=<column name>
  
  --output-phot-corr-file=<output file name>
  --phot-corr-iter-no=5
  --phot-corr-tolerance=0.001
  --phot-corr-selection-method=MEDIAN

where the input catalog is searched below ``Catalogs/<Catalog Type>``
and the output is stored in ``IntermediateProducts/<Catalog Type>``.
The recommended output name is ``<Parameter Space>_<Method>`` with the
``.txt`` extension. The default value for the number of iterations
``phot-corr-iter-no`` is 5, while for the tollerance threshold
``phot-corr-tolerance`` is :math:`10^{-3}`. Spectroscopic
errors are optionals and if missing, they are set to zero.

The ``CPC`` action has many more options, most of them present also in
the ``compute_redshifts`` action. They include parameters for the
Galactic absorption correction, priors, axes collapsing, etc. We refer
the reader to the :ref:`Compute redshifts with CLI
<compute-redshift-cli>` section and the :ref:`user-manual-advanced` chapter for more details.
