.. _physical-para:

Physical Parameters
=====================================

Physical parameters of input sources (such as age, stellar mass,
star-formation rate, etc.) can be estimated by Phosphoros during the
redshift computation if the relation between physical parameters and
SED templates is firstly defined by users.

In Phosphoros physical parameters are assumed to be a linear function
of the source luminosity (in solar luminosity unit):

.. math::
   :label: pp1

   pp = A*L/L_{\odot} + B

where *A* and *B* depend on the SED template and are fixed by users,
while :math:`L` is the luminosity in the filter used for the SED
template normalization.

In the redshift computation, Phosphoros determines the best-fit value
of physical parameters applying the above relation to the source
best-fit template. In addition, the 1D (or multi-dimensional) PDF of
physical parameters can be easily recovered from the multi-dimensional
posterior distribution, if it is computed with the ``Sampling`` option
(see :ref:`computing-redshifts`). In this case, in fact, along with
the parameters of the sampled models, Phosphoros stores the related
luminosity and physical parameters (see :ref:`Results
Format<result_files_format>`).

   
Physical Parameters in the GUI
------------------------------------------------

First of all, for any physical parameter, users must provide the *A*
and *B* values (Eq.  :eq:`pp1`) for each SED template of the
parameter space. This can be done in the ``Configuration-->Auxiliary
Data-->SEDs`` sub-panel by clicking on each SED template (see
:numref:`phpa`): a window opens showing the physical parameters that
have been already defined (if any). A new one can be defined at the
bottom of the window by filling the name, the unit, and the *A, B*
values.

.. figure:: /_static/advanced_steps/physic_para.png
    :name: phpa
    :align: center 
    :width: 800px
    :height: 400px
	     
    How to define new physical parameters with the GUI

.. note::

   Information on physical parameters is saved in the header of SED
   template files. For example, in the case shown in :numref:`phpa`,
   the mass-luminosity relation will be found in the first line of the
   ``Ell1_A_0`` SED file, as following::

     # PARAMETER : mass=0.1*L+1.3[solar mass]

   Users can use their favorite editor to modify the *A* and *B*
   values or add new physical parameters inside SED files.

.. warning::

   A physical parameter must be defined with the same unit in all the
   SEDs used in the parameter space, otherwise it is not considered
   in the redshift computation.

Once this operation has been completed, users only need to select the
physical parameters they want to include in the output of the redshift
computation. This is done in the ``Compute Redshifts-->Input/Output``
sub-panel by clicking on the ``Select Parameters`` (see
:numref:`phpa2`). A window opens with the list of available physical
parameters.

.. figure:: /_static/advanced_steps/physic_para2.png
    :name: phpa2
    :align: center 
    :width: 800px
    :height: 400px
	     
    How to select physical parameters to be computed

..
  After the run of the redshift computation, the output catalog will
  provide the best-fit value of the selected physical
  parameters. Moreover, if multi-dimensional posterior distributions
  are stored with the ``Sampling`` option (see
  :ref:`computing-redshifts`), the output products will include the
  luminosity and the physical parameter values corresponding to the
  sampled models (see :ref:`Results Format<result_files_format>`).


Physical Parameters in the CLI
------------------------------------------------

Here we assume that any physical parameter of interest is already
included in the header of SED template files, for example, through the
GUI.

..
   (see :numref:`pp_tab` as example of a FITS table)

Physical parameters can be estimated with the CLI in two simple
steps. The first step is to generate a FITS table that includes all
the desired physical parameters, their units, and the *A* and *B*
values for all the SED templates present in the parameter space. The
``build_pp_config`` (or ``BPPC``) action in Phosphoros performs this
task: it reads the header of the SED template files and writes a FITS
table with the required information. An example of configuration file
for the ``BPPC`` action is::

  physical-parameter=mass
  physical-parameter=age

  output-file=IntermediateProducts/<Catalog Type>/<name>

  sed-group=CosmosEll
  sed-group=CosmosSp 
  sed-group=CosmosSB

Here Phosphoros reads two physical parameters (*mass* and *age*) from
the SED template files located in the directories given by the
``sed-group`` options. More options for the template specification can
be found in the ``BPPC`` help (similarly to the ``compute_redshift``
action). The output FITS table will be located in the ``<Catalog
Type>`` intermediate products directory.

..
    figure:: /_static/advanced_steps/pp_tab.png
    :name: pp_tab
    :align: center 
    :width: 800px
    :height: 400px
	     
    Example of FITS table generated by the ``build_pp_config`` action.

Finally, Phosphoros computes physical parameters if the path to the
physical parameters FITS table is provided in the ``compute_redshift``
configuration file, through the option::

  physical_parameter_config_file=IntermediateProducts/<Catalog Type>/<name>



