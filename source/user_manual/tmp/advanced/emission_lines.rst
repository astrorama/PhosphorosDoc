
Emission Lines
==============

The lack of emission lines on synthetic templates is known to have a negative
effect on the photometric redshift predictions. Phosphoros does not try to
internally correct for the effect, because this would mean to separate the SED
templates in two groups. Instead, it provides an external tool for adding
emission lines to existing SED templates. The user can afterwards use these SEDs
for performing his analysis.

.. warning::
    
    You should use the Phosphoros tool to add emission lines only to synthetic
    templates, which do not already contain the emission lines. If you use it on
    templates already containing the emission lines (like empirical templates),
    your will double their effect.

Emission lines addition method
------------------------------

Phosphoros adds emission lines based on precomputed tables of the emission line
fluxes, relative to the H\ |beta| line. The tables can contain multiple columns,
to provide different ratios for different metallicity values (for more detailed
description of the file format see :ref:`emission-line-tables`).

The computation of the H\ |beta| line flux is done by using the equation:
    
.. math::
    
    F(H_{\beta}) = 4.757*10^{-13}*N_{(Lym_cont)} erg/s

The number of photons of the lyman continuum for each SED template is computed
using a second table, which converts the metallicity value to the number of
ionized photons, normalized by the luminosity at 1500 |AA| (for more detailed
description of the file format see :ref:`metal-to-phot-table`). The value from
the table is then scaled for the SED that we want to add to the emission lines
at. This is done by multiplying with the value of the SED template at 1500 |AA|.

The emission lines can be added either as dirac functions or as Gaussians. In
the second case, the velocity dispersion (expressed in km/s) is used to compute
the FWHM of each line, using the equation:

.. math::
    
    FWHM = \lambda_{line} * velocity / 299792.458

`add_emission_lines` tool
-------------------------

At the moment, the Phosphoros tool for adding the emission lines is only
available via the |CLI| mode (:ref:`cli-execution-mode`). The Phosphoros action
to use is the :code:`add_emission_lines` (or the shortcut `AEL`), which can be
called like this::
    
    Phosphoros add_emission_lines ...
    or
    Phosphoros AEL ...

Selecting the templates
-----------------------

The first thing to do, is to select the SED templates which you want to add the
emission lines to. To simplify its usage, the script gets as input a directory
with SED templates and it adds emission lines to all of them (selection of
individual files is not possible).

The parameter to set this directory is the :code:`--sed-dir`. If the templates,
which you want to add to emission lines, are in the default Phosphoros SEDs
directory (see :ref:`aux-data`), it is enough to give the directory name. For
example::
    
    Phosphoros AEL --sed-dir CosmosSB

The script can also be used for SED templates which are in any directory of your
filesystem. In this case, you will have to use the absolute path of the
directory. This way, Phosphoros can be used as a generic tool for adding
emission lines to SED templates. It is though recomended, if you plan to use the
generated SEDs in Phosphoros, to used the default directory structure.

.. warning::
    
    The relative paths are relative to the Phosphoros SEDs directory and not to
    the current working directory! For directories under the current working
    directory you will have to give their absolute path.

Generated files
---------------

The :code:`add_emission_lines` action is non-destructive, meaning that it will
not modify the original SED template files, neither add anything to the given
directory. Instead, it will store the generated templates in a new directory,
named the same as the original with the postfix :code:`_el`. Note that this
directory must not exist (otherwise the script will complain).

The names of the generated files contain the information of the parameters used
for adding the emission lines and they follow the format:
    
    <ORIGINAL_NAME>_<METALLICITY>_<HYDROGEN_FACTOR>_<METALLIC_FACTOR>.sed

where:

- ORIGINAL_NAME : is the filename of the original SED template
- METALLICITY : The value of the metallicity used
- HYDROGEN_FACTOR : The factor used for the hydrogen lines
- METALLIC_FACTOR : The relative factor used for the metallic lines

Providing custom emission line tables
-------------------------------------

Phosphoros already contains default tables for the emission lines, which are
located in the :code:`<Phosphoros_install_dir>/auxdir/EmissionLines` directory.
These are two files, one containing the hydrogen emission lines (named
:code:`hydrogen_lines.txt`) and one containing the metallic emission lines
(named :code:`metallic_lines.txt`). These lines should be good enough for the
most cases.

To use customized tables instead, the command line options :code:`--hydrogen-lines`
and :code:`--metallic-lines` can be used. They both get as parameter the file
containing the related table. Note that relative paths are relative to the
currenct working directory.

Chosing the metallicity
-----------------------

By default, Phosphoros will add emission lines for three different metallicities
(0.0004, 0.004 and 0.01), which map to the third, fourth and fifth column of the
emission line tables. This can be modified with the command line option
:code:`--metallicities`, which gets as parameter the space separated values of
the metallicities. Because Phosphoros accesses the table columns by index, if
the first metallicity does not map to the third column, the option
:code:`--first-metal-index` must also be used, to define the (zero based) index
of the column with the first metallicity. For example, to generate only SEDs
with emission lines for metallicity 0.004, which maps to the fourth column, one
must give the following command::
    
    Phosphoros AEL --sed-dir CosmosSB --metallicities 0.004 --first-metal-index 3

.. warning::
    
    At the moment, Phosphoros does not support computation of metallicities
    which are not in consecutive columns in the emission line tables

Hydrogen linesfactors
---------------------

Experience has shown that just adding the emission lines with the method
described earlier, does improves the PHZ results, but it does still does not
give the optimal results. This is due to the rough approximation of the H\ |beta|
intensity and the fact that in reality the emission line intensities vary from
object to object.

The above problem can be fixed by 