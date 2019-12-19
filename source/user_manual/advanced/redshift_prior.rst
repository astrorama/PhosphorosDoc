.. _redshift-prior:
    
Redshift Distribution Priors
=======================================

Redshift distribution describe the probability that a galaxy of
apparent magnitude :math:`m_0` is at redshift *z* and belongs to a
type :math:`T`, :math:`p(z,T|m_0)`. It follows that

.. math::

   p(z,T|m_0)=p(T|m_0)p(z|T,m_0)\,,

where :math:`p(T|m_0)` is the galaxy type fraction as a function of
magnitude and :math:`p(z|T,m_0)` is the redshift distribution for
galaxies of magnitude :math:`m_0` (see also the :ref:`bayesian-priors`
section).

Phosphoros implements these priors following the procedure
developed by Benitez 2000 :cite:`Ben00` and implemented in the *Le
Phare* code :cite:`Ilb06`.

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

In total, there are 13 free parameters
:math:`\{\alpha_t,\,z_{0t},\,k_{mt},\,f_t,\,k_t\}`, while the
:math:`C_t` values are determined by imposing the redshift
distribution :math:`p(z|T,m_0)` to be normalized to 1 *(or are they provided by users? tbc)*. The values of
free paramters used in Phosphoros are reported in the Table below
(they are updated with respect to the ones provided by Benitez 2000
and Ilbert et al. 2006). Users can change them in the CLI mode.


+---------------+---+------------------+----------------+----------------+-------------+-------------+
| Spectral Type | t | :math:`\alpha_t` | :math:`z_{0t}` | :math:`k_{mt}` | :math:`f_t` | :math:`k_t` |
+===============+===+==================+================+================+=============+=============+
| E/S0          | 1 | 2.46             | 0.431          | 0.091          | 0.30        | 0.4         |
+---------------+---+------------------+----------------+----------------+-------------+-------------+
| Sbc, Scd      | 2 | 1.81             | 0.390          | 0.100          | 0.35        | 0.3         |
+---------------+---+------------------+----------------+----------------+-------------+-------------+
| Irr           | 3 | 2.00             | 0.300          | 0.150          |             |             |
+---------------+---+------------------+----------------+----------------+-------------+-------------+
   
The spectral type fractions at :math:`m_0=20` are therefore
30% E/SO, 35% spirals, and 25% irregulars.


Redshift Priors in the GUI 
------------------------------------

Redshift distribution priors are enabled in the GUI checking ``N(z)
Prior`` in the ``3. Prior`` sub-panel of the ``Compute Redshifts``
window. Clicking on the ``Configure N(z) Prior`` a popup window opens
where users have to select the *B* and *I* filters from the
database. These filters are needed to compute the *B-I* color of
modeled templates and so their type.

.. figure:: /_static/advanced_steps/z_distr_prior.png
    :align: center
    :width: 800px
    :height: 400px
..    :scale: 50 %


.. note::

   The above free parameters cannot be modified in the GUI.

.. warning::

   The *I* filter must be part of the filters selected to compute
   photometry. This is not the case for the *B* filter.


Redshift Priors in the CLI 
------------------------------------

Redshift distribution priors are enabled in the CLI setting the action
parameter ``--Nz-prior=YES`` (the default is ``NO``) of the
``compute_redshift`` action.

Qualified names (below the ``AuxiliaryData/Filters`` directory) for
the *B* and *I* filters are required through the options::

  Nz-prior_B_Filter=<name>
  Nz-prior_I_Filter=<name>

The *I* filter is used to compute the apparent magnitude of galaxies
and must be part of the selected photometric filters.

The value of the above free parameters can be changed by users with
the option::

  Nz-prior_<p>_T<i>=<value>

where ``p = z0`` (:math:`z_{0t}`), ``Km`` (:math:`k_{mt}`), ``alpha``
(:math:`\alpha_t`), ``K`` (:math:`K_{t}`) and ``f`` (:math:`f_t`),
while ``i`` refers to the galaxy type (:math:`t=1,2,3`, apart from
``f`` and ``K`` where :math:`t=1,2`). For example, the option::

  Nz-prior_z0_T2=0.5

modifies the spiral galaxies parameter :math:`z_{02}` to 0.5.


.. creare un solo file con references e metter i link come sotto!!!

.. bibliography:: references_advanced.bib
..		  
  bibliography:: /Users/tucci/Zastro/Zeuclid/Zphosphoros/PhosphUserManual_new/source/references.bib

..		  