.. _sed-interpo:

SED interpolation
===============================

..
  As discussed in the previous section, the SED axis consists in a
  discrete set of SEDs that should map the continuous
  multi-dimensional space of colors. However, input SED libraries
  typically sample non-uniformly the colors space.

Phosphoros provides a tool to create new SEDs through a linear
interpolation of two SED templates present in the input library. This
tool can help users to fill regions of the colors space that are
undersampled.

Users have to select two reference SEDs for the interpolation and to
choose the number of new SEDs to be generated. The interpolation is
performed along the wavelength axis and the new SEDs will be uniformly
distributed between the two reference SEDs. If the reference SEDs have
a different sampling in wavelengths, the finest SED sampling is used
to resample the other SED. If the two SEDs cover different wavelength
range, interpolation is computed only in the common range and new SEDs
are set to zero outside.

SED interpolation with GUI 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In ``Configuration-->AuxiliaryData-->SEDs`` sub-panel, users can click
on the ``Create Interpolated SEDs`` button, and a pop-up window opens
(see :numref:`sedint`). After selecting two SEDs of the input
library, users must provide the number of new SEDs to be
created. Clicking on the ``Add SED``, a new input SED can be added and
used for interpolation.

The directory where interpolated SEDs are stored is specified in the
``Save in a new Folder with name`` tab. The directory is expected to
be new, otherwise, it will be cleaned before storing the new SEDs.
Input SEDs used for the interpolation can be also included in the
directory.
   
The name used to store a new SED is explanatory of how interpolation
is computed: in the example of :numref:`sedint`, 2 new SEDs are
generated from ``Ell1_A_0.sed`` and ``Ell2_A_0.sed``, and a new one
from ``Ell2_A_0.sed`` and ``Ell3_A_0.sed``; their names will be::
  
  > 1:3_Ell1_A_0_+_2:3_Ell2_A_0.sed
  > 2:3_Ell1_A_0_+_1:3_Ell2_A_0.sed 
  > 1:2_Ell2_A_0_+_1:2_Ell3_A_0.sed 

  
.. figure:: /_static/basic_steps/SED_interpo.png
    :name: sedint
    :align: center
    :scale: 50 %
	   
    Panel to create interpolated SEDs
  
SED interpolation with CLI
^^^^^^^^^^^^^^^^^^^^^^^^^^

SED interpolation can be performed in the CLI using the
``InterpolateSED`` (or ``IS``) action. 

An example of configuration file is::

  sed-dir=$PHOSPHOROS_ROOT/AuxiliaryData/SEDs/CosmosEll
  seds=Ell1_A_0.sed,Ell2_A_0.sed,Ell3_A_0.sed
  numbers=2,1
  out-dir=<dir-name>
  copy-sed=False

With the above configuration file, Phosphoros creates three new SEDs:
two by interpolating the ``Ell1_A_0.sed`` and ``Ell2_A_0.sed`` SEDs,
and one by interpolating the ``Ell2_A_0.sed`` and ``Ell3_A_0.sed``
SEDs. Output files are stored in the directory ``<dir-name>`` below
``AuxiliaryData/SEDs/CosmosEll`` (the location of the output directory
is relative to the directory provided by ``--sed-dir``), and named as
shown in the previous sub-section.

.. note::

   If physical parameters are found in SED file headers, common
   physical parameters are interpolated too. Set
   ``--interpolate-pp=False`` to avoid it.


