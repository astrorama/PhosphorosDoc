.. _emission-lines:

Emission Lines
==============

Phosphoros provides an external tool for adding emission lines to
existing restframe SED templates. Users can afterwards use these
SEDs for performing the analysis.

.. warning::
    
    Users should use the Phosphoros tool to add emission lines only to
    synthetic templates, which do not already contain the emission
    lines, and not to SEDs that already contains them, as for example
    empirical SEDs.
    

Adding emission lines in the GUI
--------------------------------------------

The GUI allows users to add emission lines to SED templates. In
``Configuration-->AuxiliaryData-->SEDs`` sub-panel (see
:numref:`emlines`), users can click on the ``Add emission lines to SEDs``
button corresponding to the template directory to which they want to
add emission lines (selection of individual files is not possible). A
pop-up window opens, asking the scheme to use:

- the *Phosphoros* scheme, as explained in the :ref:`Methodology
  <emission-line-method>` section;

- a *Le Phare-like* scheme (see next section, or the *Le Phare*
  website [#f1em]_).
  
The new templates, including emission lines, will have to be
selected in the procedure to create or modify the parameter space
(see the :ref:`Parameter Space <parameter-space-definition>` section).

.. figure:: /_static/basic_steps/AuxDataManagement.png
   :name: emlines
   :align: center
   :scale: 70 %
	   
   ``Configuration`` panel of the GUI where to add emission lines
	   
This operation is non-destructive, meaning that it will not modify the
original SED template files, neither add anything to the given
directory. Instead, it will store the generated templates in a new
directory, named the same as the original with the postfix ``_el``
(for the *Phosphoros* scheme) or ``_lpel`` (for the *Le Phare-like*
scheme). This directory must not exist otherwise the ``Add emission
lines to SEDs`` button will be not available. The names of the
generated files are the same of the original SED files, with the same
format (see the :ref:`File format reference
<format-reference-section>` section).


Adding emission lines in the CLI
--------------------------------------------

The Phosphoros action for adding the emission lines is
``add_emission_lines`` (or the shortcut ``AEL``). The usual ``--help``
option provides the list of command line options.

.. The first thing to do, is to select the SED templates which the
   emission lines have to be added to. 

To simplify its usage, the action gets as input a directory with SED
templates and adds emission lines to all of them (selection of
individual files is not possible). Phosphoros will store the generated
templates in a new directory, named the same as the original with the
postfix ``_el``. The directory must not exist otherwise the script
will complain.

The input directory can be set by ``--sed-dir``. Using the Phosphoros
standard structure, it is enough to give the directory name as, for
example::
    
   > Phosphoros AEL --sed-dir CosmosSB

The tool can also be used for templates that are in any directory of
your file system. In this case, the absolute path of the directory has
to be provided. Phosphoros can be used therefore as a generic tool for
adding emission lines to SED templates.

.. warning::
    
    Relative paths are with respect to the Phosphoros SEDs
    directory and not to the current working directory! For
    directories under the current working directory you will have to
    give their absolute path.

Phosphoros already contains a default table for the emission line flux
ratios (see the :ref:`emission-line-method` section) that is located in::

  > /path_to_PhosphorosCore_installation_directory/auxdir/EmissionLines/emission_lines.txt
  
To use a customized table instead, the command line option
``--emission-lines`` allows users to specify a different table.

With the CLI, users can also change the relation between the flux of
the [:math:`H\alpha` ] line and of the UV continuum. In Phosphoros, for a given
template, :math:`f_{temp}`, the [:math:`H\alpha` ] line flux is computed as:

.. math::
    
      f_{[H\alpha]} = c_{H\alpha}\frac{\lambda_0\lambda_1}{\lambda_1-\lambda_0}
      \int_{\lambda_0}^{\lambda_1}
      f_{temp}(\lambda)d\lambda,

where the value of :math:`c_{H\alpha}` is set by the
``--reference-factor`` option (default: ``5.91e-6``); the two
wavelengths :math:`\lambda_0` and :math:`\lambda_1` (in |AAm|) define
the UV continuum range and are set by the ``--uv-range`` option
(default: ``1500,2800``).
   
.. note::

   Phosphoros CLI allows users to generate emission lines following
   the *Le Phare-like* scheme. In this case, the flux of the [OII]
   emission line is computed from the UV continuum, and the strength
   of the other lines is determined from their flux ratio with the
   [OII] line flux (see the table below). To this purpose, the
   configuration file should be like::

     sed-dir <SEDs directory name>
     suffix _lpel
     reference-factor 1.0e+13
     uv-range 2100,2500
     emission-lines emission_lines_lephare.txt

   and the ``emission_lines_lephare.txt`` table (below the
   ``Phosphoros/AuxiliaryData/`` directory) should contain the
   emission line flux ratios used by the Le Phare code:

   +--------------------+------------------------------+-------------+
   | Emission Line      | |lambda| [ |AAm| ]           | Line/[OII]  |
   +====================+==============================+=============+
   | :math:`H\alpha`    | 6562.80                      | 1.77        |
   +--------------------+------------------------------+-------------+
   | :math:`H\beta`     | 4861.32                      | 0.61        |
   +--------------------+------------------------------+-------------+
   | :math:`H\gamma`    | 4340.46                      | 0.0         |
   +--------------------+------------------------------+-------------+
   | :math:`H\delta`    | 4101.73                      | 0.0         |
   +--------------------+------------------------------+-------------+
   | OII                | 3727.00                      | 1.00        |
   +--------------------+------------------------------+-------------+
   | OIII               | 4958.91                      | 0.13        |
   +--------------------+------------------------------+-------------+
   | OIII               | 5006.84                      | 0.36        |
   +--------------------+------------------------------+-------------+

   
Phosphoros can add the emission lines either as a Dirac or a Gaussian
function. In the latter case, the FWHM is computed by
:math:`\lambda_{line}*\Delta v`, where the velocity dispersion
:math:`\Delta v` is controlled by the ``--velocity`` parameter. If the
parameter is absent, lines are added as dirac functions.
    
Some times it is useful to generate a file containing only the
emission lines, without the original SED template. This can be done by
the option ``--no-sed``.


.. rubric :: Footnotes

.. [#f1em] see ``http://www.cfht.hawaii.edu/~arnouts/LEPHARE/lephare.html``
