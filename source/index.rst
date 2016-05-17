.. _phosphoros-main:

############################
Phosphoros Project Main Page
############################


.. toctree::
   :hidden:

   install
   user_manual/index
   developer_manual/index
   changes
   data

This is the Phosphoros |version| main page, last updated |today|

Phosphoros is a Python script packaging a number of C++ executables which implement a
photometric redshift template fitting algorithm. In short, a large grid of photometric
models is first computed by integrating red-shifted and absorbed Spectral Energy
Distributions (SEDs) through the filter transmission curves. In the second step,
the likelihood of each model is derived by measuring its distance (though a Chi2 value for example)
to the observed photometric values. Probability Density Functions (PDFs) providing probabilities
as a function of redshift is the typical output, but PDFs in any of the other considered
parameters can also be produced.

*************************
:ref:`phosphoros-install`
*************************

Instructions of how to download and install the Phosphoros software

******************
:ref:`user-manual`
******************

User manual, containing a :ref:`quickstart tutorial <quickstart>` and the full
Phosphoros user manual

*********************
:ref:`algo-reference`
*********************

Extended description of the implemented algorithm

***********************
:ref:`developer-manual`
***********************

Developer manual, containing design explanation, module tutorials and API references

**************
:ref:`changes`
**************

Changelog between the different Phosphoros versions

***********
:ref:`data`
***********

Download SED templates, filter transmissions and catalogs
