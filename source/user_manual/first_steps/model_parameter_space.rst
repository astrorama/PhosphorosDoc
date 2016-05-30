Defining the model parameter space
==================================

In template fitting algorithm, photometric redshifts are derived by finding the best match between the observations and a number of
precomputed model photometric values. One of the main configuration is therefore the specification of the to-be-considered
parameter space, which comprises a number of axes, such as redshift, SED and reddening law. For each of these axes, a
number of values or a range and a step have to be provided. The first step of Phosphoros is then to compute a vector of model
photometric values (one for each filter) for each cell of this space. This is then called the model photometric grid.
This calculation do not depend on the observations. It can therefore be achieved before hand, once for
all sources of the catalog.

In a general and typical case, a sparse parameter space can be defined for a Phosphoros analysis. This section example
explains how to specify a parameter space composed of three sub-spaces, one of the Elliptical (and S0), a second for the
Spiral and a third for the Star Burst Galaxies. The main reason in our example to split the parameter space in three
sub-spaces is that each of them have different specification for reddening curves and B(B-V) values.

GUI: Parameter Spaces
---------------------

In order to define a new parameter space, first click on ``Parameter Spaces``, second on ``New`` and third provide a name for
this new parameter space. Then you can either ``Save`` or define a ``New`` sub-space (you can use these two actions on any order).

.. image:: /_static/first_step/ParameterSpaces_New.png
   :align: center

When selecting ``New`` at the sub-space level (number 4 in above picture), a new pop-up window opens, similar to that displayed below.

.. image:: /_static/first_step/ParameterSpaces_SubSpace.png
   :align: center

Through this window, you have to provide a name for the sub-space (Star Burst Galaxies in the above example) and specify values
for ``SED``, ``Reddening Curve``, ``E(B-V)`` and ``Redshift`` ranges. The ``SED`` and ``Reddening Curve`` pages simply allows to
select a sub-set of the possible values which are available on the system (see :ref:`Auxiliary Data Section <aux-data>`
for more information). The above picture shows how to enter values of ranges of values for the ``E(B-V)`` and ``Redshift``
axes. Values can be entered as a comma-separated list and the ``Add`` or ``Delete`` options can be used to define (or
remove) new ranges of values.


CLI how-to
----------

.. image:: /_static/construction.png
   :align: center
   :scale: 50 %
   
..
    Explain the related configuration options, which map to the same example
    shown at the GUI

﻿
..   config/GUI/ParameterSpace/Chalenge2_Parameter_Space_1.xml