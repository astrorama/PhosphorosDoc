.. _user-manual-intro:

************
Introduction
************


Phosphoros is a Python script packaging a number of C++ executables which implement a
photometric redshift template fitting algorithm. In short, a large grid of photometric
models is first computed by integrating red-shifted and absorbed Spectral Energy
Distributions (SEDs) through the filter transmission curves. In the second step,
the likelihood of each model is derived by measuring its distance (though a Chi2 value for example)
to the observed photometric values. Probability Density Functions (PDFs) providing probabilities
as a function of redshift is the typical output, but PDFs in any of the other considered
parameters can also be produced.

This user manual contains the following sections:

- :ref:`Quickstart tutorial <quickstart>`
- :ref:`First Steps <user-manual-basic-steps>`
- :ref:`Advanced Features <user-manual-advanced>`
- :ref:`File Format Reference <format-reference-section>`

