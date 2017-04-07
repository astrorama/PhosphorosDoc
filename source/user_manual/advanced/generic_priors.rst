.. _generic-priors:
    
Generic Priors
==============
   
Even though Phosphoros provides some default priors functionality, it also
allows for custom, pre-computed, user-defined priors. Using these priors, the
user can try different ideas without having to modify Phosphoros code. Currently
two types of generic priors are allowed, the axes priors and the
multi-dimensional generic prior.

.. note::
    
    Currently, generic priors can only be used via the |CLI|. |GUI| support will
    be implemented in a future release of Phosphoros.
    
.. warning::
    
    All the generic priors explained bellow are applied *after* the full
    likelihood grid has been computed. This means that if your prior defines
    big areas of zero probability, the computations for these areas will still
    be done, slowing down Phosphoros. In this case, instead of using the generic
    priors functionality, consider defining a sparse grid, as explained in the
    :ref:`sparse-grid` section.

Axes Priors
-----------

Axes priors are applied on a single axis of the parameter space, meaning that
the likelihood of all models for a single value of this axis will be scaled with
the same prior value.

These priors are represented by files in the filesystem, in the directory
``$PHOSPHOROS_ROOT/AuxiliaryData/AxisPriors/<axis_name>``, where ``axis_name``
is one of ``sed``, ``red-curve``, ``ebv`` or ``z``. The files in
these directories are organized the same way as the rest of the auxiliary data,
as explained in the :ref:`aux-data` section. There are two different types of
files in these directories, depending the type of the axis.

Numerical axes
^^^^^^^^^^^^^^

Because the E\ :sub:`(B-V)` and redshift axes are numerical, their priors can
be given as datasets representing a function. The prior files for these axes are
ASCII tables (for more format details see :ref:`axes-priors`).

As these priors represent a function, Phosphoros will use interpolation to
compute the prior values for the exact axis knots, so the first column of the
table does not have to match exactcly the axis knots. Note that this means that
the same prior file can be used for multiple grids.

.. warning::
    
    If an axis knot is outside the range of the given prior table, Phosphoros
    will a prior of zero value and all models belonging to this slice of the
    parameter space will get zero probability.

To use the numerical axes priors, you should copy your prior files in the
related directory under the AuxilaryData and use the parameters 
:code:`--axis-function-prior-ebv` and :code:`--axis-function-prior-z` when
calling the :code:`compute_redshifts` action.

For example, if you want to use a prior which will penaltize hi E\ :sub:`(B-V)`
values, you create the file :code:`test.txt` in the directory
:code:`$PHOSPHOROS_ROOT/AuxiliaryData/AxisPriors/ebv` with the contents::
    
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

Then you can compute the photometric redshifts by using the command::
    
    Phosphoros CR --axis-function-prior-ebv test ......
    
.. tip::
    
    Note that the parameter is the filename **without** the extension

Non-numerical axes
^^^^^^^^^^^^^^^^^^

The SED and reddening curve axes are not numerical, so Phosphoros cannot use the
same type of functions like the ones mentioned above. Instead, the priors of
these two axes are again table files, but now the first column is a string, and
it must match exactly the names of the axis knots. Note that to use a file as a
non-numerical axis prior, it must contain entries for all the knots of your axis
(containing extra is allowed), otherwise Phosphoros will not know how to scale
some of the knots and will reject the file.

To use the non-numerical axes priors, you should copy your prior files in the
related directory and use the parameters ``--axis-weight-prior-sed`` and
``--axis-weight-prior-red-curve`` when calling the ``compute_redshifts`` action.

For example, if you have used three different variations of the calzetti law for
reddening, you will have to apply a prior to balance this fact. In this case,
you should create the file ``calzetti-fix.txt`` in the directory
``$PHOSPHOROS_ROOT/AuxiliaryData/AxisPriors/red-curve``, with the contents::
    
    SB_calzetti         1.
    SB_calzetti_bump1   1.
    SB_calzetti_bump2   1.
    SMC_prevot          3.
    
Then you can compute the photometrid redshifts by using the command::
    
    Phosphoros CR --axis-weight-prior-red-curve calzetti-fix .....

.. tip::
    
    Note that the parameter is the filename **without** the extension

.. _multi_dim_generic_prior:

Multi-dimensional Prior
-----------------------

The axes priors described above, obviously cannot express priors which depend
on multiple parameters. For example, it is not possible to describe a prior with
different E\ :sub:`(B-V)` curve for different SED templates. For these types of
priors, Phosphoros allows for multi-dimensional generic priors, which provide
the prior value for each cell of the parameter space.

These priors are represented by FITS files, the format of which is described
:ref:`here <grid-prior-format>`. Phosphoros searches for these files in the
directory ``$PHOSPHOROS_ROOT/AuxiliaryData/GenericPriors``. Note that the prior
parameter space must match exactly the parameter space of the models photometry
grid you use as input when you run Phosphoros to compute the photometric
redshifts. As constructing these prior FITS files from sratch can be quite
complicated, Phosphoros provides a tool (action ``create_flat_grid_prior`` or
``CFGP`` for short), which gets as input a model grid and constructs a prior
FITS file with all the cells set to 1.

For example, if you have used the default naming for the model grid of the
quickstart tutorial (``model_grid.dat``), you can generate a flat prior with the
command::
    
    Phosphoros CFGP --catalog-type Quickstart --out-grid-name test.fits

Note that the file is going to be created in the correct directory under
AuxilaryData. You can then use your favorite tool for editing FITS arrays (we
recommend astropy) and for setting the actual values of your prior.

.. tip::
    
    You should use the HDUs with the axes knot values to build your prior
    correctly. Do not try to guess the values of the axes from their indices.
    
Once you have build your prior, you can use it when computing the photometric
redshifts by using the parameter ``--generic-grid-prior``::
    
    Phosphoros CR --generic-grid-prior test.fits
    
.. tip::
    
    Note taht the parameter is the filename **with** the extension. This is one
    of the exceptions for the files in the AuxilaryData directory.

.. warning::
    
    Building the multi-dimensional generic prior is a cumbersome task and the
    FITS files you produce are not reusable for different parameter spaces. For
    this reason, if you should always favor the axes priors over the
    multi-dimensional one.
