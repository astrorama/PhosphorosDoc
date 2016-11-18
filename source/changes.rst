.. _changes:

Changes in Phosphoros
*********************

Phosphoros 0.6
==============

New features:

* Likelihood and posterior multi-dimensional grids are kept in logarithmic scale. Conversion to linear scale is done only for the marginalization. (issue #11051)
* Support for fixing the redshift from an input catalog column values (issue #11109)
* Volume prior effectiveness can now be controlled (issue #11120)
* Luminosity prior effectiveness can now be controlled (issue #11120)
* Support for 1D PDF output for all axes (SED, Reddening Curve, E(B-V) and redshift)
* PhosphorosAddEmissionLines has now an option to output only the emission lines


GUI:

* New tool (PlotPhotometryComparison), which plots the observed and best fitted model photometries over the model SED for comparison
* The PlotSpeczComparison tool now shows the 1D PDFs for all axes
* The PlotSpeczComparison tool now gets as input the Phosphoros results directory instead of the individual files


Output:

* Likelihood and posterior output files are in logarithmic scale instead of linear (issue #11051)
* Option to store the 1D PDFs as a catalog column (issue #11124)
* Best fitted model output columns are now optional


Bug fixes:

* Absolute Luminosity computation in flux bug is fixed
* Issue #11178: The user luminosity functions are now treated as the X axis is cosmology agnostic (M - 5logh)
* A bug with computing wrongly the normalization factor while combining the 1D PDFs of a sparse grid is fixed. This fixes the NaN values in the output PDFs.
* Issue #11118: Phosphoros GUI crashes when opening the Sub-Spaces of the parameter space


Phosphoros 0.5
==============

New features:

* Generic Prior support: Axis Priors and Multi-dimensional Prior
* Emission Lines support
* Luminosity SED grouping can now be done by groups (issue #10498)

GUI:

* Rearranged main panels and improved the communication between them (issues #10709)
* Support for multiple ranges for redshift and E(B-V) (issue #10556)
* Support for individual values for redshift and E(B-V) (issue #10556)
* Redesigned SED and Reddening Curve selection (issue #10616)
* Support for cosmological parameters (issue #10561)
* Redesigned Catalog Type Definition page (issues #10579 #10628 #10745)
* Redesigned Luminosity SED grouping panel (#10617)

Output:

* The chi square output has been replaced by the posterior logarithm

Bug fixes:

* When using sparse grids, the correct one is selected after the prior application (issue #10225)
* Issue #10309 : the computation of photometric redshifts never ends when 'bad fluxes' are present and for some SEDs
* Issue #10311 : zero-point corrections computation failure due to specz outside the model range
* The flux conversion from ergs/s/cm^2/Hz to micro-Jansky is corrected (this bug was resulting to wrong scale factors)
* Issue #10310 : Phosphoros PSC can now handle input catalogs with different ID column name