.. _volume-prior:

Volume Prior
================

The volume prior takes into account the fact that the volume sampled
by a survey increases with redshfit. It increases therefore the
likelihood of sources being at higher redshifts and disfavours
low-redshift solutions where the volume sampled is very
small. Phosphoros assumes a volume prior proportional to the
redshift-dependent differential comoving volume,
i.e. :math:`p(z)\propto dV_C(z)/dz` (see :ref:`Methodology: Priors
<bayesian-priors>`, or the Phosphoros paper for more details).

**In the GUI mode**

Volume prior is simply included in the redshifts computation by
selecting ``Volume Prior`` in the ``Compute Redshifts --> 3. Prior``
sub-panel.

**In the CLI mode**

Volume prior is enabled by setting ``--volume-prior=YES`` in the
``compute redshifts`` executable (by default it is ``NO``). An
effectiveness value different from 1 can be set with the command
option ``--volume-prior-effectiveness`` (see :ref:`effectiveness`).
