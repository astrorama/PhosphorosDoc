Defining the model parameter space
==================================

In template fitting algorithm, photometric redshifts are derived by finding the best match between the observations and a number of
precomputed model photometric values. One of the main configuration is therefore the specification of the to-be-considered
parameter space, which comprises a number of axes, such as redshift, SED and reddening law. For each of these axes, a
number of values or a range and a step have to be provided. The first step of Phosphoros is then to compute a vector of model
photometric values (one for each filter) for each cell of this space. This is then called the model photometric grid.
This calculation do not depend on the observations. It can therefore be achieved before hand, once for
all sources of the catalog.

GUI how-to
----------

.. image:: /_static/construction.png
   :align: center
   :scale: 50 %

..
    Show an example with multiple ranges and values

CLI how-to
----------

.. image:: /_static/construction.png
   :align: center
   :scale: 50 %
   
..
    Explain the related configuration options, which map to the same example
    shown at the GUI