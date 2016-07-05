
Phosphoros overview
===================

The Phosphoros template fitting algorithm involve two main step. First, a relatively large grid of photometric
models is computed by integrating red-shifted and absorbed Spectral Energy Distributions (SEDs) through the filter
transmission curves. This first step is refer to as ``compute model grid (CMG)`` both in the command line and the GUI options.
Second, the likelihood of each model characterized by a vector of computed photometric values (one value for each
photometric filter) is derived by measuring the distance (though a Chi2 value for example) to the observed photometric
values. The second step is called ``compute redshift (CR)``. Thew following picture provides a sketch of these main steps
with their inputs and outputs as well as one of the optional step.

.. figure:: /_static/first_step/DataFlowOverview.png
    :align: center
    :scale: 50 %

The ``basic steps`` section of this user manual covers:

#. Some important Phosphoros data organization concepts (:ref:`link <important-concept>`)
#. How to launch Phosphoros both in GUI or command line modes (:ref:`link <executing-phosphoros>`)
#. The input data setup (:ref:`link <setup-input-data>`)
#. The mapping between catalog column and filter names (:ref:`link <catalog-column-mapping>`)
#. The parameter space definition (:ref:`link <parameter-space-definition>`)
#. How to generate the photometric model grid, i.e., the first execution step (:ref:`link <generating-model-grid>`)
#. How to compute redshift, i.e., teh second execution step (:ref:`link <compute-redshift>`)
#. Additional tools for looking at the main results (:ref:`link <examining-results>`)

Probability Density Functions (PDFs) providing probabilities as a function of redshift is the typical output, but
PDFs in any of the other considered parameters can also be produced as well as the best fitted model.

More advance Phosphoros features, such as the ``Photometric Zero Point Correction`` illustrated in the above picture are
described in the ``Advanced Features`` :ref:`section <user-manual-advanced>`

   
..
    It starts with a paragraph explaining the three kind of steps: model grid
    generation, optional steps and redshift computation.

    Introduces the concept of the parameter space. Explains that the models are
    the computed photometries.

    This is at theoretical level. Diagrams should be used, files or directories
    not.