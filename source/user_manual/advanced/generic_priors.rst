.. _generic-priors:
    
Generic Priors
==============
   
Although Phosphoros provides some default prior functionality,
pre-computed user-defined priors are also allowed. In this way, users
can try different ideas without having to modify the Phosphoros
code. Currently two types of generic priors are allowed, axes priors
and multi-dimensional generic priors.

.. note::
    
    Currently, generic priors can only be used via the CLI.

.. The |GUI| support will be implemented in a future release of Phosphoros.
    
.. warning::
    
    Generic priors are applied **after** the full likelihood grid has
    been computed. This means that if a prior defines big areas of
    zero probability, the computations for these areas will still be
    done, slowing down the Phosphoros execution. In this case, instead
    of using the generic priors functionality, a sparse grid should be
    considered.

    .. , as explained in the :ref:`sparse-grid` section.

Axes Priors
---------------

Axes priors are applied on a single axis of the parameter space,
meaning that the likelihood of all models with the same value of the
axis will be scaled with the same prior value.

Phosphoros reads axes priors from files located in the directory::

  > $PHOSPHOROS_ROOT/AuxiliaryData/AxisPriors/<axis_name>/

where ``axis_name`` is one of the following names: ``sed``,
``red-curve``, ``ebv`` or ``z``. The files in these directories are
expected to be ASCII tables with two columns, the axis value and the
prior value (for more details see :ref:`Axes Priors Format
<axes-priors>`).

..
  organized in the same way as the rest of the auxiliary data (see the
  :ref:`Directory Organization <directory-organization>` section) 
     
Axes priors can be defined in two different ways, depending on the
type of axes:

..
  Numerical axes
  ^^^^^^^^^^^^^^

1. **Numerical axes**

   Color excess E\ :sub:`(B-V)` and redshift are numerical
   parameters. Phosphoros uses interpolation to compute prior values
   for the exact axis knots. So axis values at which the prior is
   computed do not have to match exactly the axis knots, and
   the same prior file can be used for multiple grids.

   .. warning::
    
     If an axis knot is outside the range of the given prior table,
     Phosphoros will assign a prior of zero value to it and all models
     belonging to this slice of the parameter space will get zero
     probability.

   Numerical axes priors are applied in the ``compute_redshifts``
   (or ``CR``) action using the action parameters::

     axis-function-prior-ebv=<filename>
     axis-function-prior-z=<filename>

   where the argument is the file containing the prior values (in the
   directory ``$PHOSPHOROS_ROOT/AuxiliaryData/AxisPriors/ebv``
   or ``z``). For example, if you want to use a prior which will
   penalize high E\ :sub:`(B-V)` values, you set
   ``axis-function-prior-ebv=ebv_prior`` and create a file
   ``ebv_prior.txt`` like::
    
    0.0 	0.988813044611
    0.2 	0.998750780925
    0.4 	0.969233234476
    0.6 	0.903707077873
    0.8 	0.809571648668
    1.0 	0.696804775496
    1.2 	0.576229073672
    1.4 	0.457833361772
    1.6 	0.3495006002
    1.8 	0.256340151415
    2.0 	0.180639851619

   .. note::
    
    The filename of prior functions is provided **without** the
    extension.

2. **Categorical axes**

   SEDs and reddening curves are categorical parameters, and
   Phosphoros cannot use the same approach as for numerical
   parameters. The priors of these two axes are provided again as
   tables, but now the first column is a string, and it must
   match exactly the name of the axis knots. Tables must contain
   entries for all the knots of your axis (extra entries are instead
   allowed).

   To use these priors, users should call the ``compute_redshifts``
   action with the parameters::

     axis-weight-prior-sed=<filename>
     axis-weight-prior-red-curve=<filename>

   For example, if three different variations of the *Calzetti* law
   for reddening have been used, users might want to apply a prior to
   balance this fact. In this case, they can create a
   ``redcurve_prior.txt`` file in the directory
   ``$Phosphoros_Root/AuxiliaryData/AxisPriors/red-curve`` like::
    
    SB_calzetti         1.
    SB_calzetti_bump1   1.
    SB_calzetti_bump2   1.
    SMC_prevot          3.
    
   and set ``axis-weight-prior-red-curve=redcurve_prior``.
    
   .. note::
    
     Again, the filename is provided **without** the extension.


.. _multi_dim_generic_prior:

Multi-dimensional Priors
------------------------------

The axes priors described above cannot express priors which depend on
multiple parameters. For example, it is not possible to describe
priors on E\ :sub:`(B-V)` that are different for different SED
templates. In this case, Phosphoros allows for multi-dimensional
generic priors, which provide a prior value for each cell of the
parameter space.

Multi-dimensional priors must be provided in FITS files, whose format
is described in the :ref:`Priors Format Reference
<grid-prior-format>`. Phosphoros searches for these files in the
``Phosphoros_Root/AuxiliaryData/GenericPriors`` directory.

The prior parameter space must match exactly the parameter
space of the grid of models that will be used to compute
photometric redshifts. As constructing these prior FITS files from
scratch can be quite complicated, Phosphoros provides a tool, the
``create_flat_grid_prior`` (or ``CFGP`` for short) action, that gets
as input a grid of models and constructs a prior FITS file with all the
cells set to 1.

..
  For example, if you have used the default naming for the model grid of the
  quickstart tutorial (``model_grid.dat``), you can generate a flat prior with the
  command::
    
    Phosphoros CFGP --catalog-type Quickstart --out-grid-name test.fits

For example, users can generate a flat prior (named ``multi_priors.fits``)
with the command::

  Phosphoros CFGP --out-grid-name=multi_priors.fits --model-grid-file=<path>/<model grid name>

where the ``--model-grid-file`` parameter asks for the qualified filename
of the grid of models.

The file ``multi_priors.fits`` will be created in the correct
directory, i.e. ``AuxilaryData/GenericPriors/``. Users can then use
their favorite tool for editing FITS arrays and for setting the actual
prior values.

.. tip::

    We recommend **Astropy**, a Python library for astronomy, to
    manage FITS files [#f_gen1]_ .

..    
    Users should use the Header Data Unit (HDU) of the generated FITS
    file with the axes knot values to build your prior correctly. Do
    not try to guess the values of the axes from their indices.
    
Once you have build the prior FITS file, you can use it when computing
the photometric redshifts by including in the configuration file the action parameter::
    
    generic-grid-prior=multi_priors.fits
    
.. note::
    
    The argument of the ``--generic-grid-prior`` option is the
    filename **with** the .FITS extension.

.. This is an exception for files in the ``AuxilaryData/`` directory.

.. warning::
    
    Building the multi-dimensional generic prior is a cumbersome task
    and the FITS files you produce are not reusable for different
    parameter spaces. For this reason, users should always favor the
    axes priors over the multi-dimensional ones.

.. rubric :: Footnotes

.. [#f_gen1]  See the web page: https://docs.astropy.org/en/stable/io/fits/
