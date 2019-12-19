.. _overview:
	     
Phosphoros overview
======================

Phosphoros is a Python script packaging a number of C++ executables
that implement a photometric redshift template fitting algorithm (see,
e.g. :cite:`Lan96`; :cite:`Bol00`; :cite:`Ilb06`).
In short, multi-band photometric measurements from an input catalog of
sources are compared to a grid of modeled photometry. For each source,
the algorithm computes the likelihood that a model is representative
of the observed photometry via a classical :math:`\chi^2`
calculation. Prior information can also be taken into account using
Bayesian inference. The main outputs of Phosphoros are the *best-fit*
model for input sources (in terms of redshift, spectral energy
distribution, etc.), and the redshift Probability Density Function
(PDF), which provides the probability of a source to be at a given
redshift.

Phosphoros involves two main steps (discussed in detail in the
:ref:`Methodology <methodology>` section):

**1 Grid of Models** As a first step, it generates a *large* set of
models for which photometry are computed. Models are characterized by
four parameters: redshift, restframe Spectral Energy Distribution
(SED) template, intrinsic reddening curve and intrinsic color excess
:math:`E_{(B-V)}` value. The grid of models is created through the
following operations (in this order):

* Building a library of restframe SED templates.

* (optional) Adding emission lines to templates.

* Correcting restframe SED templates for the effect of intrinsic dust
  attenuation.

* Redshifting SED templates to the observed frame.

* (optional) Applying intergalactic medium (IGM) redshift-dependent
  absorption. 

* Integrating modeled templates through a set of filter transmission
  curves.

..  (see the :ref:`emission-lines` section).  

Each model is characterized by a vector of computed photometric
values, one value for each selected filter.

**2 Redshift Estimate**

* The likelihood :math:`\mathcal{L}` of models is determined by
  computing the :math:`\chi^2` between observed and modeled
  photometry.

* (optional) Priors on input parameters are taken into account and
  the posterior distribution of a model is computed.

* The output is a catalog containing the best-fit model for each
  input source and its redshift at the redshift PDF peak. Optionally,
  1D PDFs for the model parameters can be saved, along with the
  multi-dimensional likelihood or posterior distribution.

..  (:ref:`Advanced Feature <user-manual-advanced>`).  

Phosphoros includes also:
  
**Additional functionalities**, to be applied after generating the
grid of models and before the redshift estimate, as:

* (optional) Correction for the SED reddening due to Milky Way dust
  absorption.

* (optional) Zero-point correction, accounting for calibration issues
  and mismatches between galaxy colors and templates.

..  (see the :ref:`galactic-absorption-advanced` section). 
..  (see the :ref:`zero-point-correction` section).

**Tools to examine results**, producing:

* Statistical analysis of the redshift PDF of input sources.

* Plots comparing reference and estimated redshifts.

The following picture provides a sketch of main Phosphoros steps with
their inputs and outputs, including optional functionalities.


.. figure:: /_static/basic_steps/Flowchart_Phosphoros.pdf
    :align: center
    :scale: 50 %

Phosphoros is usable via two execution modes, i.e. the **Command Line
Interface** (**CLI**) and the **Graphical User Interface**
(**GUI**). The CLI allows to call pre-defined Phosphoros executables
coupled with a list of parameters. A convinient way to use command
lines is to create :ref:`configuration files <config-file-usage>`. On
the other hand, the GUI allows for a more user-friendly interactive
execution of Phosphoros. Users are encouraged to run Phosphoros with
the GUI.


.. bibliography:: references_basic_over.bib

.. Here, in the **Basic Steps** chapter, we covers the following topics:
..
   #. Some important Phosphoros data organization concepts and setup
      (:ref:`link <concept-setup>`)
   #. How to execute Phosphoros in the GUI mode (:ref:`link
      <execution-gui-all>`)
   #. How to execute Phosphoros in the CLI mode (:ref:`link
      <cli-explain>`)
   #. Graphical tools to examine Phosphoros main results (:ref:`link
      <examining-results>`)

..
   More advanced features are illustrated in the :ref:`Advanced
   Features <user-manual-advanced>` section, while formats of input
   and output files are described in the :ref:`File format reference
   <format-reference-section>` section.

..
   #. A brief description of the main steps in the Phosphoros algorithm 
       (:ref:`link <algorithm-basics>`) 
..
   #. The mapping between catalog column and filter names (:ref:`link <mapping>`)
   #. The parameter space definition (:ref:`link <parameter-space-definition>`)
   #. How to generate the photometric model grid, the first execution step (:ref:`link <generating-model-grid>`)
   #. How to compute redshift, the second execution step (:ref:`link <computing-redshifts>`)
..
    It starts with a paragraph explaining the three kind of steps: model grid
    generation, optional steps and redshift computation.

    Introduces the concept of the parameter space. Explains that the models are
    the computed photometries.

    This is at theoretical level. Diagrams should be used, files or directories
    not.
