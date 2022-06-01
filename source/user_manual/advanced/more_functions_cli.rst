.. _additional-functionality-CLI:

Additional Functionalities in the CLI
==================================================

.. _cli-data-pack:

Download or update the Data Pack
------------------------------------------------------

The ``update_data_package`` (or ``UDP``) comand line action allows
users to (re-)download the last available version of the auxiliary
data pack from the Phosphoros repository (containing, i.e., SEDs,
filters, Reddening curves, etc.). Files are automatically located in
the proper directories according to the Phosphoros internal data
organization (see :ref:`directory-organization`).

This action is also useful if users altered the data locally and they
want to replace them with the current data pack on the repository.

.. _order-sed:

Order the SED templates
--------------------------------------------------

The ``order_seds`` (or ``OS``) command line action reads SED templates
in a directory specified by the user and writes the ordered filenames
into the ``order.txt`` file at the SEDs location. For example, based
on the COSMOS library, the command::

  > Phosphoros OS -d $PHOSPHOROS_ROOT/AuxiliaryData/SEDs/CosmosEll/

will generate the following ``order.txt`` file ::

  Ell7_A_0.sed
  Ell6_A_0.sed
  Ell5_A_0.sed
  Ell4_A_0.sed
  Ell3_A_0.sed
  Ell2_A_0.sed
  Ell1_A_0.sed

Users can choose the first file in the list with the option
``-s``. For example, setting ``-s Ell3_A_0.sed``, the first SED name
in the file will be ``Ell3_A_0.sed``.

This action is useful when the SED template PDFs are examined,
so they can be found in a suitable order.

.. note::

   The ordering can be also inputed manually by creating/editing the
   ``order.txt`` file

.. _explore_aux_cli:

Explore Auxiliary Data from CLI
-----------------------------------------

Users can explore Auxiliary data (i.e., filters, SED templates and
reddening curves) that are available in the database using the
following actions: ``display_filters`` (or ``DF``), ``display_seds``
(or ``DS``) and ``display_reddening curves`` (or ``DRC``).

For example, the command::

  > Phosphoros DF

show all the filters available in the database; while::

  > Phosphoros DF --group=SC456 

shows all the filters within the ``SC456`` group (directory). Finally::

  > Phosphoros DF --data=SC456/g.ascii

show the content of the filter file ``g.ascii``, belonging to the
``SC456`` group.

The ``display_seds`` and ``display_reddening curves`` actions have the
same parameters as ``display_filters``.


.. _sed-template-retrieve:

Retrieve the SED template of a single model
----------------------------------------------------------

The ``compute_model_sed`` (or ``CMS``) action allows users to compute,
given a SED template, the modeled SED for a set of parameters
:math:`\{`\ Z, :math:`E_{(B-V)}`, reddening curve\ :math:`\}`. The
computed SED will be displayed on the terminal. For example::

  > Phosphoros
  CMS --sed-name=CosmosSp/S0_A_0 --normalization-filter=DECAM/r
  --normalization-solar-sed=solar_spectrum 
  --reddening-curve-name=calzetti --ebv-value=0.2 --z-value=2

The ``--sed-name`` option specifies the SED template to be considered
and it is searched below the ``AuxiliaryData/SEDs`` directory. The SED
is normalized through the standard normalization options,
``--normalization-filter`` and ``--normalization-solar-sed`` (see,
e.g., :ref:`scale-factor`). All these options are mandatory. The SED
parameters are specified through the optional options
``reddening-curve-name`` (the file is searched in the
``AuxiliaryData/ReddeningCurves`` directory), ``--ebv-value`` and
``--z-value``. If not provided, the default values will be taken,
i.e. ``calzetti``, ``0`` and ``0``, respectively.

IGM absorption can be also included using the
``--igm-absorption-type`` parameter. Allowed arguments are: ``OFF``
(default), ``MADAU``, ``MEIKSIN``, ``INOUE``.

.. warning::

   The filename of SED templates and reddening curves must be given
   **without extension**.

.. tip::

   Users can store a modelled SED in the following way::

     > Phosphoros CMS  <options>  >> <SED file name>

.. _investigate-model-grids:

Investigate model grids
------------------------------------

Users can display the parameters of an existing grid of models
using the ``display_model_grid`` (or ``DMG``) action. The qualified
name of the model grid file has to be provided with the
``--model-grid-file`` option. In the standard configuration, this is
simply done as::

  > Phosphoros DMG --catalog-type=<Catalogue_Type> --model-grid-file=<filename>

An example of the output of this action is::

  Photometry info
  ---------------
  IGM absorption method: MADAU
  Photometry filters:
      DC3/H
      DC3/J
      DC3/Y
      DC3/g
      DC3/i
      DC3/r
      DC3/u
      DC3/vis
      DC3/z

  Parameter Space info
  --------------------
  Number of regions: 3
  Regions names:  "Elliptical" "Spiral" "Star Burst"
  Total number of SED templates: 31
  Total range of E(B-V): [0, 1]
  Total range of Redshift Z: [0, 3]
  Total number of models: 45225

In the example, there are three sub-space regions. For a given region,
users can display the values of a specific model parameter using the
``--region`` action option followed by the parameter name (``--sed``,
``--redcurve``, ``--ebv`` or ``--z``). As example, adding
``--region="Spiral" --ebv`` in the command line, you find something
like::

  Info for parameter space region "Spiral"
  ----------------------------------------

  Axis E(B-V) (6)
  Index	Value
  0	        0
  1	        0.1
  2	        0.2
  3	        0.3
  4	        0.4
  5	        0.5
  
Modeled photometry of a specific parameter cell can be shown by
``--phot=<arg>``, where the arguments are the 0-based indexes of the
axis nodes, which are available from the output of this action with
the ``--region`` option (in the box above, the E(B-V) indices are the
first column). As example, adding ``--region="Star
Burst" --phot=5,0,4,10`` you get the photometry in the corresponding
parameter cell, as shown below::

  Info for parameter space region "Star Burst"
  ----------------------------------------

  Cell (5,0,4,10) axis information:
  SED      CosmosSB/SB3_A_0
  REDCURVE SB_calzetti
  EBV      0.4
  Z        1

  Cell (5,0,4,10) Photometry:
  Quickstart/g	1.42545e-11
  Quickstart/H	5.60482e-10
  Quickstart/i	7.28291e-11
  Quickstart/J	3.43554e-10
  Quickstart/r	2.84818e-11
  Quickstart/vis	5.08239e-11
  Quickstart/Y	2.0981e-10
  Quickstart/z	1.37999e-10


More command line options can be found with the help command
(``Phosphoros DMG --help``).


.. _axis-collapse:

Axis Collapse options
----------------------------

Phosphoros derives the one-dimensional PDF of a model parameter by
projecting the multi-dimensional likelihoods or posterior
distributions to the parameter axis. The common example is the
redshift PDF.

Three possible techniques for the axes projection are implemented in
Phosphoros:

* **Marginalization** (``BAYESIAN``): Likelihood or posterior
  distributions are integrated (or summed for categorial
  variables such as SED templates or reddening curves) over
  the parameters *not of interest*. This is the default option.

* **Maximum likelihood** (``MAX``). The PDF of the parameter *of
  interest* is determined by taking the maximum likelihood corresponding
  to each value of that parameter.

* **Summing** method (``SUM``). Likelihood or posterior distributions
  are added up over the parameters *not of interest*. This method
  differs from marginalization when the grid of models for numerical
  variables is not uniformly sampled. Each point of the grid is
  assumed with the same weight.

Users can change the way to collapse axes through the following action
parameters of the ``compute_redshift`` action:

- for posterior distributions, ``--axes-collapse-type=<arg>``;

- for likelihood distributions, ``--likelihood-axes-collapse-type=<arg>``.

In both cases, the possible arguments are ``BAYESIAN`` (default), ``MAX``,
``SUM``.

.. note::

   The Axis Collapse options are only available with the CLI. In the
   GUI, axes projection is performed with the ``Marginalization``
   method for posterior distributions and with the ``Maximum
   likelihood`` method for likelihood distributions.

.. _effectiveness:

Prior effectiveness
------------------------

Phosphoros gives the opportunity to choose the *effectiveness*
(:math:`e_{ff}`) of a prior. This is a value between 0 and 1 that
modifies a prior :math:`p` as follows:

.. math::

    p = p_{max}*(1-e_{ff})+e_{ff}*p\,,

where :math:`p_{max}` is the maximum value of the prior.  For
:math:`e_{ff}<1`, priors have a broader shape, especially in the
low--probability range. For example, in the range where the original
prior was zero, it becomes :math:`p=p_{max}*(1-e_{ff})`.

The prior effectiveness can be applied to redshift distribution
priors, luminosity priors and volume prior, respectively, using the
following options of the ``compute_redshift`` action::

  Nz-prior-effectiveness=<value>
  luminosity-prior-effectiveness=<value>
  volume-prior-effectiveness=<value>
   
.. note::

   The Prior effectiveness option is only available with the CLI. With
   the GUI, it is always 1.

.. _reference-sample:

Build a reference sample
------------------------------------------------------

Phosphoros includes an action to build a reference sample that
provides, for each source of the output catalog, the redshift PDF and
the SED corresponding to the best-fit model. This action has been
specifically created for the usage of another tool named *NNPZ*.

The action to build the reference sample is ``build_reference_sample``
(or ``BRS``). It requires as inputs the qualified name of the output
catalog and the directory where the reference sample will be located
(the directory will be created by Phosphoros; if already exist,
Phosphoros will complain)::

  --phosphoros-catalog=<path>/<output catalog filename>
  --reference-sample-dir=<path>/<directory name>

More options are::
  
  --normalization-filter=<path>/<filter>
  --normalization-solar-sed=solar_spectrum
  --phosphoros-catalog-format=<arg>
  --igm-absorption-type=<arg>

They are used to specify the filter and the solar SED for the template
normalization, the format of the output catalog (``FITS`` or
``ASCII``; default= ``FITS``) and the type of IGM absorption to apply
(``OFF``, ``MADAU``, ``MEIKSIN``, ``INOUE``; default= ``OFF``).

This action generate three binary files including, respectively, the
source ID plus an index to identify sources in the other files; the
redshift PDFs; the SEDs computed from the best-fit models.

