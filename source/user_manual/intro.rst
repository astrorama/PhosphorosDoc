.. _user-manual-intro:

************
Introduction
************

Phosphoros is a Python script packaging a number of C++ executables
which implement a photometric redshift template fitting algorithm. In
short, a large grid of photometric models is first computed by
integrating redshifted and absorbed Spectral Energy Distribution (SED)
template through the filter transmission curves. In the second step,
the likelihood of each model is derived by measuring its distance
(e.g., through the :math:`\chi^2` value) to the observed photometric
values. The typical outputs of Phosphoros are Probability Density
Functions (PDFs) as a function of redshift, but PDFs in any of the
other considered parameters can also be produced.

The user manual contains the following sections:

- :ref:`Quickstart tutorial <quickstart>`
- :ref:`Methodology <methodology>`
- :ref:`Basic Steps <user-manual-basic-steps>`
- :ref:`Advanced Features <user-manual-advanced>`
- :ref:`File Format Reference <format-reference-section>`

