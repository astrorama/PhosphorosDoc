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
``Configuration-->AuxiliaryData-->SEDs`` sub-panel, users can select
the template directories which they want to add emission lines to
(selection of individual files is not possible), and click on the
``Add emission lines to SEDs`` button. The new templates including
emission lines will have to be then selected in the procedure to
create or modify the parameter space (see the :ref:`Parameter Space
<parameter-space-definition>` section).

  .. figure:: /_static/basic_steps/AuxDataManagement.png
     :align: center
     :scale: 70 %
	   
This operation is non-destructive, meaning that it will not modify the
original SED template files, neither add anything to the given
directory. Instead, it will store the generated templates in a new
directory, named the same as the original with the postfix
``_el``. This directory must not exist otherwise the ``Add emission
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
templates and it adds emission lines to all of them (selection of
individual files is not possible). Phosphoros will store the generated
templates in a new directory, named the same as the original with the
postfix ``_el``. The directory must not exist otherwise the script
will complain.

The input directory can be set by ``--sed-dir``. Using the Phosphoros
standard structure, it is enough to give the directory name as, for
example::
    
   > Phosphoros AEL --sed-dir CosmosSB

The tool can also be used for templates which are in any directory of
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

The numerical factor that relates the [OII] line and the UV continuum
luminosities can be modified by the command line options
``--oii-factor`` or ``--oii-factor-range``, providing a new value or a
range of values for the luminosity factor, respectively. The
wavelength interval to integrate the UV continuum can also be changed
by ``--uv-range`` that provides the beginning of the UV range. *(and the end?)*

Phosphoros can add the emission lines either as a Dirac or a Gaussian
function. In the latter case, the FWHM is computed by
:math:`\lambda_{line}*\Delta v`, where the velocity dispersion
:math:`\Delta v` is controlled by the ``--velocity`` parameter. If the
parameter is absent, lines are added as dirac functions.
    
Some times it is useful to generate a file containing only the
emission lines, without the original SED template. This can be done by
using the parameter ``--no-sed``.
