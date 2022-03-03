.. _redshift-prior:
    
Redshift Distribution Priors
=======================================

Phosphoros implements the :math:`\mathcal{P}(z,T|m_0)` prior, i.e.
the probability that a galaxy of apparent magnitude :math:`m_0` is at
redshift *z* and belongs to the type :math:`T` (see :ref:`Methodology:
Redshift Priors<redshift-dist>`). It follows that

.. math::

   \mathcal{P}(z,T|m_0)=p(T|m_0)p(z|T,m_0)\,,

where :math:`p(T|m_0)` is the galaxy type fraction as a function of
magnitude and :math:`p(z|T,m_0)` is the redshift distribution for
galaxies of magnitude :math:`m_0`.

Phosphoros follows the procedure developed in Benitez 2000
:cite:`Ben00` and implemented in the *Le Phare* code :cite:`Ilb06`.
Three different galaxy types are considered according to the
:math:`B-I` color: **early types** (E/S0) if :math:`B-I>1.285`,
**irregulars** if :math:`B-I<0.945` and **spirals** (Sbc, Scd)
otherwise. The color is computed for each restframe SED template in
the grid of models, without intrinsic reddening.

The apparent magnitude :math:`m_0` of modeled SED templates is then
computed with respect to the :math:`I` filter, for each model of the
grid.

For :math:`m_0(I)<20`, we set the prior equals to 1 for :math:`z\le1`
and 0 otherwise.

For :math:`m_0(I)>20`, we assume that:

- the spectral type prior can be parametrized as:

  .. math::

      p(T|m_0)=f_t\,e^{-k_t(m_0-20)}\,,

  with :math:`t=1` for early types and :math:`t=2` for spirals. The
  fraction of irregulars (:math:`t=3`) is automatically determined by
  the other two fractions, :math:`p(3|m_0)=1-p(1|m_0)-p(2|m_0)`.

- The redshift distribution prior is:

  .. math::

     p(z|T,m_0)=C_t z^{\alpha_t}
     \exp\bigg\{-\bigg[\frac{z}{z_{mt}(m_0)}\bigg]^{\alpha_t}\bigg\}\,,

  giving an exponential cutoff in the galaxy distribution at high
  redshifts. The *median redshift*, :math:`z_{mt}`, is chosen to have a
  linear dependence on magnitude:

.. math::

   z_{mt}(m_0)=z_{0t}+k_{mt}(m_0-20).
   
.. warning::

   The exponential cutoff at high redshifts in the redshift
   distribution prior is maybe too strong and it will be updated with a
   power-law cutoff in the future Phosphoros versions.
   
In total, there are 16 free parameters
:math:`\{\alpha_t,\,z_{0t},\,k_{mt},\,f_t,\,k_t,\,C_t\}`. The values
of free parameters used in Phosphoros are reported in
:numref:`tzprior` (they are updated with respect to the ones provided
by Benitez 2000 and Ilbert et al. 2006). Users can change them in the
CLI mode.

.. table:: Parameters of the redshift distribution prior
   :name: tzprior

   +---------------+---+------------------+----------------+----------------+-------------+-------------+-------------+
   | Spectral Type | t | :math:`\alpha_t` | :math:`z_{0t}` | :math:`k_{mt}` | :math:`f_t` | :math:`k_t` | :math:`C_t` |
   +===============+===+==================+================+================+=============+=============+=============+
   | E/S0          | 1 | 2.46             | 0.431          | 0.091          | 0.30        | 0.4         | 0.8869      |
   +---------------+---+------------------+----------------+----------------+-------------+-------------+-------------+
   | Sbc, Scd      | 2 | 1.81             | 0.390          | 0.100          | 0.35        | 0.3         | 0.8891      |
   +---------------+---+------------------+----------------+----------------+-------------+-------------+-------------+
   | Irr           | 3 | 2.00             | 0.300          | 0.150          |             |             | 0.8874      |
   +---------------+---+------------------+----------------+----------------+-------------+-------------+-------------+
   
The spectral type fractions at :math:`m_0=20` are therefore
30% E/SO, 35% spirals, and 25% irregulars.


Redshift Priors in the GUI 
------------------------------------

Redshift distribution priors are enabled in the GUI by checking ``N(z)
Prior`` in the ``3. Prior`` sub-panel of the ``Compute Redshifts``
window (see :numref:`zprior`). Clicking on the ``Configure N(z)
Prior`` a popup window opens where users have to select the *B* and
*I* filters from the database. These filters are needed to compute the
*B-I* color of modeled templates and so their type.

.. figure:: /_static/advanced_steps/z_distr_prior_v12.png
    :name: zprior 
    :align: center
    :width: 800px
    :height: 400px

    Introducing redshift priors with the GUI

.. note::

   The values of the parameters of the redshift distribution priors
   (:numref:`tzprior`) cannot be modified in the GUI.

.. warning::

   The *I* filter must be part of the filters selected to compute
   photometry. This is not the case for the *B* filter.


Redshift Priors in the CLI 
------------------------------------

Redshift distribution priors are enabled in the CLI by setting the action
parameter ``--Nz-prior=YES`` (the default is ``NO``) of the
``compute_redshift`` action.

Qualified names (below the ``AuxiliaryData/Filters`` directory) for
the *B* and *I* filters are required through the options::

  Nz-prior_B_Filter=<name>
  Nz-prior_I_Filter=<name>

The *I* filter is used to compute the apparent magnitude of galaxies
and must be part of the selected photometric filters.

The values of the parameters of the redshift distribution priors
(:numref:`tzprior`) can be changed by users with the option::

  Nz-prior_<p>_T<i>=<value>

where ``<p>`` can be ``z0`` (i.e. :math:`z_{0t}` in the above
equation), ``Km`` (:math:`k_{mt}`), ``alpha`` (:math:`\alpha_t`),
``K`` (:math:`K_{t}`), ``f`` (:math:`f_t`) and ``cst`` (:math:`C_t`),
while ``i`` refers to the galaxy type (:math:`t=1,2,3`, apart from
``f`` and ``K`` where :math:`t=1,2`). For example, the option::

  Nz-prior_z0_T2=0.5

modifies the spiral galaxies parameter :math:`z_{02}` to 0.5.

An effectiveness value different from 1 can be set with the command
option ``--Nz-prior-effectiveness`` (see :ref:`effectiveness`).

.. creare un solo file con references e metter i link come sotto!!!

.. bibliography:: references_advanced.bib
..		  
  bibliography:: /Users/tucci/Zastro/Zeuclid/Zphosphoros/PhosphUserManual_new/source/references.bib

..		  
