
****************************
Advanced Phosphoros Features
****************************

Here goes everything else, described in a manual way (not like a reference)

.. _directory_howto_section:

How to modify the default directory structure
=============================================

GUI
---

CLI
---

Axis Collapse options
=====================

Sparse Grid
===========

Explanation of what it is

GUI
---

CLI
---

Volume Prior
============

Luminosity Prior
================
Luminosity Prior Configuration
------------------------------
In order to apply a luminosity prior to the analysis one first have to define it. One must choose to work in **magnitude** or **flux**, specify for which **filter** the luminosity is computed (note that this filter is not necessary one of the source catalog) whether the luminosity has to be corrected for intrinsic reddening and provide a **luminosity function**. 

The luminosity function can be specified with:

- a Schechter parametrization, providing the alpha, phi* and M* or L*,
- a file storing the sampling of the object's density as a function of the luminosity. The files must contains two columns, the first represent the luminosity (either in magnitude AB or in flux [erg/s 1/Hz]) and the second is the object's density [1/Mpc³]. The provided files are stored in the *<AuxiliaryData>/LuminosityFunctionCurves* directory. 
 
Furthermore, the luminosity function is not requiered to be the same for all the SED-redshift parameter space. One can define redshift ranges and SED groups and provide one luminosity function for each couples {redshift range, SED group}. Note that the relative normalization between the different luminosity function is independent of the number of SED in the groups.

In order to run the redshifts computation one need to compute a Luminosity model grid which is a model grid for the filter used to compute the luminosity and with a parameter space restricted to redshift 0. The GUI will automatically compute this grid when launching the redshift computation. On the other hand CLI usage requires that one compute this grid before launching the redshifts computation. 

GUI
---
In order to use luminosity function curves, one should have beforehand added them in the *Configuration/Aux. Data Management/Luminosity Function Curves* page. 

For defining luminosity priors, one navigate to the *Compute redshifts* page, ensure that a catalog type and a parameter space have been selected and that the model grid has been computed, then under *3. Algorithme Options* it is possible to open the dedicated popup by clicking on the button *Configure Luminosity Prior*. The popup gives the possibility of creating new luminosity prior and managing existing ones. 

By default a newly created prior has a single luminosity function defined for a redshift range(matching the total range span by the parameter space redshifts and a single SED group (containing all the SED of the paramater space). Spliting the SED group and the redshift range is done in popups opened by the *Manage SED Groups* and *Manage Redshift Intervals* buttons. 

Once the redshifts ranges and the SED groups are defined one can specify each luminosity function by clicking on the corresponding cell and providing either the Schechter parameters or selecting a luminosity function curve. The GUI provides also the possibility of editing the Schechter parameters for all the luminosity functions in a popup launched by the *Bulk Schechter Edit* button. 

CLI
---
**Luminosity Model Grid** 

The luminosity model grid has to be computed in advance using the command *Phosphoros CLMG* from the Model Grid and the luminosity filter. Typicall call will looks like
:: 

Phosphoros CLMG --model-grid-file <model grid file name.dat>  --luminosity-filter <filter qualified name> --output-model-grid <luminosity model grid file name.dat> 

**Redshifts computation configuration**

The configurartion needed to define the luminosity prior are the following:
Global options:

- luminosity-filter=<Filter qualified name>
- luminosity-function-corrected-for-extinction= **NO** /YES
- luminosity-function-expressed-in-magnitude= **YES** /NO
- luminosity-model-grid-file=<Luminosity grid file>

SED group definition (note that all the SEDs used in the parameter space must be present in one (and only one) group):

- luminosity-sed-group-<SED_group_name>=<coma separated SED qualified names>

Luminosity function (note that the redshift range must span the entire range used in the parameter space):

- luminosity-function-min-z-<function id>=<z_min>
- luminosity-function-max-z-<function_id>=<z_max>
- luminosity-function-sed-group-<function_id>=<SED_group_name>

Schechter parametrization (in magnitude, if expressed in flux replace m0 by l0):

- luminosity-function-schechter-alpha-<function id>=<alpha>
- luminosity-function-schechter-phi0-<function id>=<phi>
- luminosity-function-schechter-m0-<function id>=<m>
Alternativelly one can specify the curve:
 
- luminosity-function-curve-<function id>=<luminosity funtion curve qualified name>

Enabling the Luminosity prior
-----------------------------
In the GUI check the *Luminosity Prior* box and select which of the defined prior has to be applied in the nearby dropdown.

In the CLI add the configuration *luminosity-prior=YES*


Generic Priors
==============

Axes Priors
-----------

Generic Prior
-------------

.. _posterior-investigation:
    
Posterior Investigation
=======================

Photometric Zero Point Corrections
==================================

Emission Lines
==============

Investigate model grids
=======================

Explore Aux Data from CLI
=========================

Order the SED templates
=======================

Retrieve the SED template of a single model
===========================================
