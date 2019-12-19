.. _galactic-absorption-CLI:

Galactic absorption correction in the CLI
==================================================

The Milky Way absorption correction is implemented in the CLI through
two or three steps, depending on whether Galactic color excess is
already included in the input catalog or has to be read from a
reddenning map (see the :ref:`galactic-absorption` section for a
description of the method employed for the correction).

First, a grid of correction coefficients is generated. These are the
linear coefficients :math:`a_{{\scriptscriptstyle SED},i}` of
Eq. :eq:`ga4` in the :ref:`galactic-absorption` section, used by
Phosphoros to compute the total extinction for a given filter. They
depend on the source SED and on the filter transmission curve.

The grid is computed using the
``compute_galactic_correction_coeff_grid`` action (or simply
``CGCCG``), which calls the﻿
PhosphorosComputeGalacticAbsorptionCoefficientGrid C++
executable. They are generated after building the grid of models.

For this action, Phosphoros reads by default the following
configuration file ::

  > $PHOSPHOROS_ROOT/config/PhosphorosComputeGalacticAbsorptionCoefficientGrid.conf

or, if not found, the system one in the installation directory. A
different configuration file can be selected through the
``--config-file`` option.

An example of the configuration file is::

  phosphoros-root=/home/phosphoros/Phosphoros 
  catalog-type=Challenge2
  model-grid-file=Grid_Chalenge2_Parameter_Space_MADAU.dat
  igm-absorption-type=MADAU
  milky-way-reddening-curve-name=F99/F99_3.1
  output-galactic-correction-coefficient-grid=Grid_Challenge2_Parameter_Space_MADAU_MW_Param.dat

The list of action parameters includes the catalog type, the filename
of the grid of models, the type of IGM absorption, and the qualified
filename of the Milky Way reddening curve (it is searched below the
``AuxiliaryData/Filters/`` directory).

The Phosphoros CLI gives the possibility to use a generic reddening
maps (in HEALPIX [#fga_adv]_ format) by the ``add_galactic_dust_data``
(or ``AGDD``) action. This action read source coordinates from the
input catalog and extract the corresponding color excess values from a
reddening map. The :math:`E_{(B-V)}` values are then added to the
input catalog as a new column.

The main action parameters are input/output data and the source
coordinates (in degrees)::

  planck-dust-map=<path>/<name of reddening map>
  input-catalog=<input catalog name>
  output-catalog=<output catalog name>
  ra=<Right Ascension column in the input catalog>
  dec=<Declination column in the input catalog>
  galatic-ebv-col=<column name to be added>

The ``planck-dust-map`` and ``galatic-ebv-col`` parameters are
optional and, if not provided, the *Planck* reddening map will be
considered and the column will be named ``GAL_EBV``.

Finally, modeled photometry are corrected by Milky Way absorption. The
correction is enabled within the ``compute_redshift`` action by
setting the ``dust-column-density-column-name`` action parameter::

  dust-column-density-column-name=<E(B-V) column name>

Other action parameters are::

  dust-map-sed-bpc=<bpc value>
  galactic-correction-coefficient-grid-file=<coefficient grid name>

The former defines the band-pass correction that is required when
Galactic color excess values are derived from sources different from
B5 stars. The default value is 1.018 that corresponds to the band pass
correction for the *Planck* reddening map. If the Schlegel et
al. :cite:`sch98` reddening map is used the band-pass correction
equals to 1. *(tbc)*

The filename of the correction coefficients grid, computed in the
first step, must be specified through the
``galactic-correction-coefficient-grid-file`` parameter (the path is
required only if it is not located in the default directory).


.. rubric :: Footnotes

.. [#fga_adv] see https://healpix.sourceforge.io/


.. bibligraphy:: reference_advanced.bib
