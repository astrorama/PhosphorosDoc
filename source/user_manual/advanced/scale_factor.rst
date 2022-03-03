.. _scale-factor:

Scale Factor
=====================================

Template fitting methods usually fix the normalization of SED
templates (or **scale factor**) to the value that minimizes the
:math:`\chi^2` (the best-likelihood value) in order to reduce the
number of free parameters and to be faster (see the
:ref:`Metodology<template-fitting>` chapter for more details). By
default, Phosphoros does the same. Nevertheless, Phosphoros gives the
possibility to treat the scale factor as a free parameter of the
model, in a fully Bayesian approach.

..
  The redshift PDFs are then derived after marginalization of the scale factor too.

We remind that the best-likelihood value of the scale factor and its
uncertainty (obtained through the observed flux errors propagation)
can be analytically derived by the following formula:

.. math::
   :label: sf1

   \alpha_{best} = \sum_i \frac{f_{obs}^if_m^i}{\sigma_i^2} \bigg/ \sum_i
   \frac{(f_m^i)^2}{\sigma_i^2}~~~~~~~~~~~\sigma_{\alpha} =
   1/\sqrt{\sum_i\frac{(f_m^i)^2}{\sigma_i^2}}\,,

where :math:`f_{obs}^i` and :math:`f_m^i` are the observed and modeled
flux in the :math:`i` filter. Modeled fluxes are computed by
Phosphoros after normalizing SED templates to the solar luminosity at
10pc distance in a filter chosen by users.

In order to optimize the calculations, Phosphoros uses a grid of
values for the scale factor distributed around its best-likelihood
value. The number of sampled points and the sampling range (defined in
terms of the scale factor uncertainty) are chosen by users.

Scale Factor in the GUI
------------------------------------------------

In order to sample the scale factor and to marginalize it in the
redshift PDF computation, three steps are required:

- Select the solar SED in the ``Configuration-->Auxiliary
  Data-->SEDs`` sub-panel by clicking on the ``Reference Solar SED:
  solar_spectrum`` button.

- Select the reference filter to use for the SED template
  normalization in the ``Compute Redshift-->1. Luminosity Filter and
  Extrinsic Absorption`` by clicking on the ``Select Filter`` button
  (the default filter is on the left of the button).

- Select the ``Sample and marginalize the model scaling`` option in
  the ``Compute Redshift-->5. Algorithm`` sub-panel. The number of
  points in the sample and the sampling range, defined as
  :math:`\alpha_{best}\pm n\times\sigma_{\alpha}`, can be modified by
  users (see :numref:`scalfac`).

.. figure:: /_static/advanced_steps/scale_factor.png
    :name: scalfac
    :align: center 
    :scale: 70%
	     
    SED normalization sampling with the GUI

  
Scale Factor in the CLI
------------------------------------------------

The SED normalization sampling and marginalization is performed by the
Phosphoros action ``compute_redshift`` (or ``CR``) if the following
option in the configuration file is ``YES`` (by default, ``NO``)::

  scale-factor-marginalization-enabled=YES

Users can choose the sampling number points and range by::

  scale-factor-marginalization-range-size=<n_sigma> (default=5)
  scale-factor-marginalization-sample-number=<n_points> (default=101)

where the sample range is :math:`\alpha_{best}\pm{\rm <n\_sigma>}\times\sigma_{\alpha}`.

Finally, the filter for the SED normalization and the solar SED
template can be specified through::

  normalization-filter=DECAM/r
  normalization-solar-sed=solar_spectrum

where the filter and the solar SED are expected to be found in the
``$PHOSPHOROS_ROOT/AuxiliaryData/Filters`` and
``$PHOSPHOROS_ROOT/AuxiliaryData/SEDs`` directories, respectively.
