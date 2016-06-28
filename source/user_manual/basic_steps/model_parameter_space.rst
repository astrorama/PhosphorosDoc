Defining the model parameter space
==================================

In template fitting algorithm, photometric redshifts are derived by finding the best match between the observations and a number of
precomputed model photometric values. One of the main configuration is therefore the specification of the to-be-considered
parameter space, which comprises a number of axes, such as redshift, SED and reddening law. For each of these axes, a
number of values or a range and a step have to be provided. The first step of Phosphoros is then to compute,  for each cell
of this space, a vector of model photometric values (one for each filter). This is then called the model photometric grid.
This calculation do not depend on the observations. It can therefore be achieved before hand, once for
all sources of the catalog.

In a general and typical case, a sparse parameter space can be defined for a Phosphoros analysis. In this section, we show
how to specify a parameter space composed of three sub-spaces, one of the Elliptical (and S0), a second for the
Spiral and a third for the Star Burst Galaxies. The main reasoning to split the parameter space in three
sub-spaces in our example is that each of them have different specification for reddening curves and B(B-V) values.

The GUI-based below instructions only provide an illustration on how to enter specifications. Make sure you complete the
full specification of the three parameter sub-spaces before continuing to the next section. Only the command line below
section shows a complete configuration set.

GUI: ``Parameter Spaces``
-------------------------

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

.. _PhosphorosComputeModelGrid_configuration_section:

CLI: ``PhosphorosComputeModelGrid.conf`` file
---------------------------------------------

The name of the C++ executable which will first use the parameter space configuration is ``PhosphorosComputeModelGrid``.
As a consequence, Phosphoros GUI saves the parameter space specifications in the configuration file of this executable, which
standard name is``PhosphorosComputeModelGrid.conf``. A complete and fully documented PhosphorosComputeModelGrid.conf is
available as part of the installed software in the following location::

    /path_to_PhosphorosCore_installation_directory/conf/PhzExecutables/PhosphorosComputeModelGrid.conf

With a system installation, ``/path_to_PhosphorosCore_installation_directory`` should be something like
``/opt/euclid/PhosphorosCore/0.5/InstallArea/x86_64-fc23-gcc53-o2g/" where ``0.5`` is PhosphorosCore version and
``x86_64-fc23-gcc53-o2g`` reflect some of the compilation options. These two field will vary in different installations.

Below you find a listing of the file used in this User Manual::

    phosphoros-root=/home/dubath/Phosphoros
    thread-no=6

    catalog-type=Challenge2

    filter-name=DECAM/g
    filter-name=DECAM/i
    filter-name=DECAM/r
    filter-name=DECAM/z
    filter-name=EUCLID_DC1/vis
    filter-name=vista/H
    filter-name=vista/J
    filter-name=vista/Y

    igm-absorption-type=MADAU

    output-model-grid=Grid_Chalenge2_Parameter_Space_1_MADAU.dat

    sed-include-Elliptical Galaxies=Cosmos/Ell1_A_0
    sed-include-Elliptical Galaxies=Cosmos/Ell2_A_0
    sed-include-Elliptical Galaxies=Cosmos/Ell3_A_0
    sed-include-Elliptical Galaxies=Cosmos/Ell4_A_0
    sed-include-Elliptical Galaxies=Cosmos/Ell5_A_0
    sed-include-Elliptical Galaxies=Cosmos/Ell6_A_0
    sed-include-Elliptical Galaxies=Cosmos/Ell7_A_0
    sed-include-Elliptical Galaxies=Cosmos/S0_A_0

    sed-include-Spiral Galaxies=Cosmos/Sa_A_0
    sed-include-Spiral Galaxies=Cosmos/Sa_A_1
    sed-include-Spiral Galaxies=Cosmos/Sb_A_0
    sed-include-Spiral Galaxies=Cosmos/Sb_A_1
    sed-include-Spiral Galaxies=Cosmos/Sc_A_0
    sed-include-Spiral Galaxies=Cosmos/Sc_A_1
    sed-include-Spiral Galaxies=Cosmos/Sc_A_2
    sed-include-Spiral Galaxies=Cosmos/Sd_A_0
    sed-include-Spiral Galaxies=Cosmos/Sd_A_1
    sed-include-Spiral Galaxies=Cosmos/Sd_A_2
    sed-include-Spiral Galaxies=Cosmos/Sdm_A_0

    sed-include-Star Burst Galaxies=Cosmos/SB0_A_0
    sed-include-Star Burst Galaxies=Cosmos/SB10_A_0
    sed-include-Star Burst Galaxies=Cosmos/SB11_A_0
    sed-include-Star Burst Galaxies=Cosmos/SB1_A_0
    sed-include-Star Burst Galaxies=Cosmos/SB2_A_0
    sed-include-Star Burst Galaxies=Cosmos/SB3_A_0
    sed-include-Star Burst Galaxies=Cosmos/SB4_A_0
    sed-include-Star Burst Galaxies=Cosmos/SB5_A_0
    sed-include-Star Burst Galaxies=Cosmos/SB6_A_0
    sed-include-Star Burst Galaxies=Cosmos/SB7_A_0
    sed-include-Star Burst Galaxies=Cosmos/SB8_A_0
    sed-include-Star Burst Galaxies=Cosmos/SB9_A_0

    reddening-curve-name-Elliptical Galaxies=calzetti

    reddening-curve-name-Spiral Galaxies=SMC_prevot

    reddening-curve-name-Star Burst Galaxies=SB_calzetti
    reddening-curve-name-Star Burst Galaxies=SB_calzetti_bump1
    reddening-curve-name-Star Burst Galaxies=SB_calzetti_bump2

    ebv-value-Elliptical Galaxies=0.000000

    ebv-range-Spiral Galaxies=0.000000 0.050000 0.010000
    ebv-range-Spiral Galaxies=0.050000 0.300000 0.050000
    ebv-range-Spiral Galaxies=0.300000 1.000000 0.100000

    ebv-range-Star Burst Galaxies=0.000000 0.050000 0.010000
    ebv-range-Star Burst Galaxies=0.050000 0.300000 0.050000
    ebv-range-Star Burst Galaxies=0.300000 1.000000 0.100000

    z-range-Elliptical Galaxies=0.000000 6.000000 0.050000

    z-range-Spiral Galaxies=0.000000 6.000000 0.050000

    z-range-Star Burst Galaxies=0.000000 6.000000 0.050000

In this file, you first find some of the specification detailed in previous sections such as the phosphoros root
directory or the name of the catalog type. The parameter space specification starts with the following statement::

    sed-include-Elliptical Galaxies=Cosmos/Ell1_A_0

After the prefix ``sed-include-``, the name ``Elliptical Galaxies`` of a group of SEDs is defined and a particular SED,
``Cosmos/Ell1_A_0`` in this case, is added to this group. Each similar statement adds another SED to the same group.

The SED specification can also be done *negatively* starting with a::

    sed-group-Elliptical Galaxies=Cosmos

statement, which define a ``Elliptical Galaxies`` group, which includes first all SEDs from the ``Cosmos`` group. After this statement,
particular SED can be excluded from the group with declaration such as::

    sed-exclude-Elliptical Galaxies=Cosmos/Sa_A_0

As also shown in the above file, the reddening curve, the E(B-V), and the redshift specifications for each group of SED
are entered using the following prefixes::

    reddening-curve-name-*
    ebv-value-*
    ebv-range-*
    z-value-*
    z-range-*

The ``*value*`` prefix must be followed by a single value, while the ``*range*`` ones must be completed with ``start, stop, step``
triplets. Please note that no ``z-value-`` expressions are used in the above example.