.. _data:

***************
Data Repository
***************

Quickstart data
===============

This are the data needed by the :ref:`quickstart tutorial <quickstart>`. For
instructions of how to use these data refer the tutorial itself.

All quickstart data: `tar.gz file <http://www.isdc.unige.ch/phosphoros/data/other/quickstart.tar.gz>`_

.. Example Configuration Files
.. ===========================

.. :download:`BuildTemplates.conf </_static/BuildTemplates.conf>` |BR|
.. :download:`DeriveZeroPoints.conf </_static/DeriveZeroPoints.conf>` |BR|
.. :download:`FitTemplates.conf </_static/FitTemplates.conf>` |BR|

Photometric Redshift Challenge Data
=========================

First challenge (2014-2015)
------------------------

Filter tansmissions: `tar.gz file <http://www.isdc.unige.ch/phosphoros/data/MER_Challenge/Filters.tar.gz>`_

SED templates: `tar.gz file <http://www.isdc.unige.ch/phosphoros/data/MER_Challenge/SEDs.tar.gz>`_

Reddening curves: `Calzetti <http://www.isdc.unige.ch/phosphoros/data/MER_Challenge/calzetti.dat>`_

Catalogs:

- `training <http://www.isdc.unige.ch/phosphoros/data/MER_Challenge/training-cat.txt>`_ :
  A small catalog containing spec-Z which can be used for training the zero point corrections
- `test <http://www.isdc.unige.ch/phosphoros/data/MER_Challenge/test-cat.txt>`_ :
  A small catalog containing spec-Z which can be used for testing the Phosphoros performance
- `full catalogs <http://euclid.roe.ac.uk/projects/sgw/wiki/Data_Challenge>`_ :
  The full MER Callenge catalogs, as provided by WP4-3-09-5101

Second challenge (2015-2016)
------------------------

Filter tansmissions: `tar.gz file <http://www.isdc.unige.ch/phosphoros/data/Challenge_2/Filters.tar.gz>`_

SED templates: `tar.gz file <http://www.isdc.unige.ch/phosphoros/data/Challenge_2/SEDs.tar.gz>`_

Reddening curves: `tar.gz file <http://www.isdc.unige.ch/phosphoros/data/Challenge_2/ReddeningCurves.tar.gz>`_

Catalogs:

1. `training catalog <http://www.isdc.unige.ch/phosphoros/data/Challenge_2/Challenge2TrainingCatalog.fits.gz>`_ :
  A catalog containing 12960 sources with spec-Z obtained by filtering
  the original file **euclid_cosmos_DC2_2fwhm_S2_v2_DESnoise_calib.fits**
  with the following criteria: 
  
  - z_spec_S15 < 6.0 
  - STAR == 0  
  - AGN == 0 
  - reliable_S15 == 1 
  - MAGERR_VIS < 0.1  
  - FWHM_IMAGE_DETECT > 2.58

2. `small training catalog <http://www.isdc.unige.ch/phosphoros/data/Challenge_2/Challenge2TrainingSmallCatalog.fits.gz>`_ :
  A smaller version of the previous catalog containing the first 3000
  sources 

3. `Photometric correction catalog <http://www.isdc.unige.ch/phosphoros/data/Challenge_2/Challenge2ZeroPointsCatalog.fits.gz>`_ :
  A catalog for computing the photometric zero point corrections with
  2876 sources obtained by using the same selection as for above
  catalog 1, but with two more restrictive value for:

  - z_spec_S15 < 2 && 
  - MAGERR_VIS < 0.01
