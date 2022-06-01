.. _methodology:

******************************
Methodology
******************************

This section aims to describe the main operations developed by the
Phosphoros algorithm and to provide the methodology background
required to better understand all the Phosphoros steps. A more
detailed explanation about the formalism, assumptions and input data
used in Phosphoros can be found in the Phosphoros paper :cite:`PP22`.
This section is complementary to the previous :ref:`Basic Steps
<user-manual-basic-steps>` and :ref:`Advanced Features
<user-manual-advanced>` sections, which are focused on how to execute
Phosphoros in the |GUI| and in the |CLI|.


.. _templates:

Templates
=========

Phosphoros requires a library of restframe spectral energy
distribution (SED) templates as input. Templates can be empirical
(typically from spectroscopic observations of nearby galaxies),
synthetic (from stellar population models), or a mix of both. The
library can also comprise SEDs of galaxies with a potential
contribution of active galactic nuclei (AGN) as well as star
templates. It is left up to users to create a less or more complex
template library.

The Phosphoros data repository includes a set of commonly used
templates for galaxies, AGN, AGN-host galaxies and stars. A detailed
description of the Phosphoros library is given in the :ref:`data-repository`
chapter. It contains, for example, the COSMOS template library (Ilbert et
al. 2009 :cite:`Ilb09`), shown in :numref:`msed`.

..
  Phosphoros provides the COSMOS template library (see Ilbert et
  al. 2009 :cite:`Ilb09`; and :numref:`msed`): it includes seven
  templates for elliptical galaxies (from E1 to E7) and twelve for
  spiral galaxies (from S0 to Sdm) from the Polletta et al. (2007
  :cite:`Pol07`) library; twelve for young blue star-forming galaxies
  (from SB0 to SB11), generated with the Bruzual\&Charlot (2003,
  :cite:`BC03`) stellar population synthesis models (starburst ages
  from 0.03 to 3 Gyr), for a total of 31 templates.

.. figure:: /_static/first_step/Ilbert09_SED.png
    :name: msed 
    :align: center 
    :width: 350px
    :height: 350px

    The 31 SED templates from the COSMOS library (figure from Ilbert
    et al. 2009): it includes seven templates for elliptical galaxies
    (from E1 to E7; red curves); twelve for spiral galaxies (from S0
    to Sdm; green curves); twelve for young blue star-forming galaxies
    (from SB0 to SB11; cyan curves). The flux scale is arbritary.

..  Red (green) [cyan] lines are for elliptical (spiral) [starburst] galaxies.

.. note::

   The only requirement for input spectra is to follow the
   standardized format described in :ref:`File Format: Input
   files<format-catalogs>`.


..
   Emission Lines
   and then adopts flux ratios in relation to the
   [:math:`H\alpha` ] line to derive the fluxes of the other emission
   lines (see Table below).

.. Kennicutt (1998) :cite:`Ken98` showed that the UV continuum and the
   luminosity of the [OII] emission line are both a diagnostic of the
   star formation rate (SFR) in galaxies. Using their relations with the
   SFR, the UV continuum is translated to the [OII] line flux through the
   relation (Moustakas et al. 2006 :cite:`Mou06`):

.. math
    
    f_{[OII]} = 0.745\times10^{13}\,f_{UV}\,.

.. By default, Phosphoros estimates the UV continuum flux for a given
   template by integrating its SED between 1500\ |AAm| and 2800\ |AAm|.

.. 
   +--------------------+------------------------------+-------------+
   | Emission Line      | |lambda| [ |AAm| ]           | Line/[OII]  |
   +====================+==============================+=============+
   | :math:`H\alpha`    | 6562.80                      | 1.0000      |
   +--------------------+------------------------------+-------------+
   | :math:`H\beta`     | 4861.32                      | 0.2960      |
   +--------------------+------------------------------+-------------+
   | :math:`H\gamma`    | 4340.46                      | 0.1350      |
   +--------------------+------------------------------+-------------+
   | :math:`H\delta`    | 4101.73                      | 0.0770      |
   +--------------------+------------------------------+-------------+
   | OII                | 3726.10                      | 0.3165      |
   +--------------------+------------------------------+-------------+
   | OII                | 3728.80                      | 0.3165      |
   +--------------------+------------------------------+-------------+
   | OIII               | 4958.91                      | 0.0490      |
   +--------------------+------------------------------+-------------+
   | OIII               | 5006.84                      | 0.1410      |
   +--------------------+------------------------------+-------------+

    
.. _intrinsic-interstellar-dust:

Intrinsic interstellar dust absorption
=========================================

SED templates of extragalactic sources have to be modified in order to
take into account the effects of extinction due to intrinsic
interstellar dust. After extinction, the source flux :math:`f_{ext}`
at the restframe wavelength |lambda| is attenuated by

.. math::

   f_{ext}(\lambda)=f_{int}(\lambda)\times 10^{-0.4k(\lambda)E_{B-V}}\,,

where :math:`f_{int}` is the intrinsic flux and :math:`k(\lambda)` is
the attenuation curve (or **reddening curve**; see :numref:`mredd`)
that defines the dependence of absorption with wavelength. The
**color excess** :math:`E_{B-V}` controls the overall amount of
absorption. Both the reddening curve and the color excess are free
parameters in the grid of models (see :ref:`Basic Steps: Generating
the model grid <user-manual-basic-steps>`).

.. Color excess :math:`E_{B-V}` values are specified by the user.

Commonly adopted reddening curves are provided as auxiliary data in
Phosphoros (see the :ref:`data-repository` chapter): e.g., the
Calzetti et al. (2000 :cite:`Cal00`) dust law for starburst galaxies;
the Fitzpatrick (1986 :cite:`Fit86`) law for the Large Magellanic
Cloud; the Prevot et al. (1984 :cite:`Pre84`) law for the Small
Magellanic Cloud. Users can add and adopt different attenuation
prescriptions.


.. figure:: /_static/first_step/Cao18_extintion.png
    :name: mredd
    :width: 500px
    :height: 350px
    :align: center

    Examples of reddening curves (figure from Cao et al. 2018
    :cite:`Cao18`).

	    
..
  tip::

   Possible parameters for the intrinsic absorption: for
   **starburst** galaxies the Calzetti et al. law and :math:`E_{B-V}`
   ranging from 0 and 1; for **elliptical** galaxies no intrinsic
   absorption, :math:`E_{B-V}=0`; for **spiral** galaxies the Prevot
   et al. law and :math:`E_{B-V}` between 0 and 1. *to be confirmed*

.. _igm-absorption:

Intergalactic medium absorption
======================================

The SED of sources at cosmological distances are also attenuated by
absorption due to the intergalactic medium (IGM) between observer and
source. This absorption is mainly due to the neutral hydrogen
contained in discrete clouds of primordial gas located along the line
of sight at various redshifts. It affects the source flux at
wavelengths shortward of :math:`{\rm Ly}\alpha` (i.e.,
1216\ |AAm| at the emitter restframe).

Common prescriptions from literature provide an estimate of the mean
effective IGM optical depth, :math:`\tau_{eff}`, along the line of
sight of a source. They are in fact based on estimates of the average
density and chemical properties of absorbers in the Universe. The IGM
impact on the source flux at restframe wavelength |lambda| is evaluated
as:

.. math:: 

   f_{ext,IGM}(\lambda)=f_{ext}(\lambda)\times
   e^{-\tau_{eff}(\lambda,z)}\,.

The effective optical depth :math:`\tau_{eff}` depends on the
restframe wavelength, modifying consequently the shape of the SED. It
depends also on the source redshift since the absorbers’ column
density increases with distance. The IGM attenuation is computed for
each redshift of the grid of models.

Three different prescriptions are currently implemented
in Phosphoros in order to compute the effective optical depth (see
:numref:`migm`).

#. Madau 1995 :cite:`Mad95`: the most commonly adopted prescription in
   template-fitting codes for photometric redshifts. It assumes a
   Poisson distribution of absorption systems. The recipe used in
   Phosphoros extends the Madau prescription taking into account the
   Lyman series up to :math:`n=18` (using the coefficients from NASA's
   HEASARC [#f1meth]_) and metal lines. It also assumes
   :math:`\exp(-\tau_{eff})=0` at :math:`\lambda < 912`\ |AAm|.

#. Inoue et al. 2014 :cite:`Ino14`: an update of the Madau model based
   on more recent observations of the intergalactic absorbers
   distribution. The implemented prescription follows the analytic
   models provided in their section 4, that approximates the Lyman
   series (up to :math:`n=40`) and Lyman continuum absorption.

#. Meiksin 2006 :cite:`Mei06`: he estimates the IGM absorption based
   on numerical simulations. In particular, Phosphoros considers the
   Meiksin prescription of the Lyman series absorption (up to
   :math:`n=31`) and of the photoelectric absorption from optically
   thin and Lyman Limit systems.

.. note::

   Because in the Inoue et al. and Meiksin formalism the value of
   :math:`\exp(-\tau_{eff})` rises to infinity towards
   :math:`\lambda=0`, the minimum value of :math:`\exp(-\tau_{eff})`
   is adopted at all wavelengths shorter than the wavelength
   corresponding to that minimum.

.. figure:: /_static/first_step/IGM.png
    :name: migm
    :width: 400px
    :height: 300px
    :align: center

    The :math:`\exp(-\tau_{eff})` curves at :math:`z=3.5` for the
    three IGM absorption prescriptions implemented in Phosphoros.

The user can choose one of these prescriptions, but not modify them or
add a new one. In Phosphoros there is also the option to not apply any
IGM absorption correction. This can reduce the time to compute the
grid of models when sources are expected to be at low-to-intermediate
redshifts and the IGM absorption is not relevant.

.. note::

   Photometric redshift estimates for high redshift sources
   significantly improve when an IGM absorption correction is
   applied. Phosphoros paper :cite:`PP22` shows that photometric
   redshifts at :math:`z>2` are biased by a factor :math:`\Delta
   z\sim0.1(1+z)`, if this correction is not taken into account. The
   three different prescriptions provide similar results.


.. note::

   The current version of Phosphoros does not take into account the
   variability of the IGM absorption with the line of sight, which
   could be more or less impacted by a higher or lower number of
   absorbers.


.. _redshifting-templates:

Redshifting of the restframe templates
============================================

Restframe SED templates (including intrinsic and IGM absorption) are
redshifted following the grid of redshifts specified by users. In
particular, the wavelength is transformed from the original, restframe
wavelength :math:`\lambda` to one at the desired redshift,
i.e. :math:`(1+z)\lambda`. Modeled SEDs are consequently obtained as

.. math:: 

   f_m(\lambda)=\frac{f_{rest}(\lambda/(1+z))}{(1+z)^2}\,.

where the factor :math:`1/(1+z)^2` takes into account the effects of
redshifting on the source flux.

   
.. _filter-curves:

Applying filter trasmission curves
======================================

As a result of the above steps, a library of redshifted and attenuated
SEDs is produced. In order to be compared with photometric flux
measurements, modeled SEDs have to be integrated through the filter
trasmission curves of the bands surveyed by the input catalog.

For photon-counting systems, such as CCDs, the observed flux through a
filter :math:`i` is computed by:

.. math::
   :label: fi1

   f_m^i =
   \frac{\int\frac{\lambda}{c}f_m(\lambda)
   T_i(\lambda)d\lambda}{\int
   T_i(\lambda)\frac{d\lambda}{\lambda}}\,,   

where :math:`T_i` is the filter trasmission curve and
:math:`f_m(\lambda)` is the observer-frame modeled SED.

For energy-measuring systems, such as bolometers, the observed flux
through a filter :math:`i` is instead:

.. math::
   :label: fi2

   f_m^i =
   \frac{\int f_m(\lambda)
   T_i(\lambda)d\lambda}{\int
   T_i(\lambda)\frac{c\,d\lambda}{\lambda^2}}\,.

Phosphoros is able to handle both cases, i.e. photon-counting (by
default) and energy-measuring filters (see :ref:`File format: Input
files<auxiliary_format>`).
   
Phosphoros ``Data Pack`` repository supplies a set of filter
transmission curves for the main observatories in nearIR/optical/UV
bands, collected from the Filter Profile Service of the Spanish
Virtual Observatory [#f3meth]_. For instance, :numref:`mfilter` shows
the filter trasmission curves used in the *Euclid* Data
Challenge 3. Users can select the transmission curves to be used or
add new ones.

.. figure:: /_static/first_step/filter_curves_DC3.png
    :name: mfilter
    :width: 600px
    :height: 350px
    :align: center

    Filter trasmission curves at different bands from the *Euclid*
    Data Challenge 3.

.. _galactic-absorption:

Galactic Absorption
=============================

The observed flux of a source is also attenuated by Milky Way dust
absorption. To account for Galactic absorption, Eq. :eq:`fi1` of the
previous sub-section should be modified as:

.. math::
   :label: ga1 

   f^i_{m,ga} = \frac{1}{\int
   T_i(\lambda)\frac{d\lambda}{\lambda}}
   \int\,\frac{\lambda}{c} f_m(\lambda)
   10^{-0.4A_{\lambda}}T_i(\lambda)d\lambda\,,

where :math:`A_{\lambda}` is the extinction due to Milky Way
absorption at wavelength :math:`\lambda`. This is usually expressed as
:math:`A_{\lambda}=E^{\scriptscriptstyle
MW}_{B-V}k_{\scriptscriptstyle MW}(\lambda)`, where
:math:`k_{\scriptscriptstyle MW} (\lambda)` is the Milky Way
absorption law, normalized to the value of the color excess
:math:`E^{\scriptscriptstyle MW}_{B-V}`. Galactic absorption, when
associated with a filter, depends therefore on the source SED.

The effect of Galactic absorption is taken into account in Phosphoros
after computing the grid of modeled photometry, using the following
expression:

.. math::
   :label: ga3

   f^i_{m,ga}=f^i_{m}\times 10^{-0.4A_{{\scriptscriptstyle SED},i}}\,,

where :math:`A_{{\scriptscriptstyle SED},i}` is the total extinction
for the filter :math:`i` defined as the logarithmic of the ratio
between the *observed* flux with and without Galactic absorption

.. math::
   :label: ga2 

   A_{{\scriptscriptstyle SED},i}=
   -2.5\log_{10}\bigg(\frac{\int_i \lambda f_m(\lambda)
   10^{-0.4A_{\lambda}} T_i(\lambda)d\lambda}
   {\int_i \lambda f_m(\lambda)T_i(\lambda)d\lambda}\bigg) \,.

In the context of template-fitting codes, computing *reddened* SEDs by
Eq. :eq:`ga2` would be too time-demanding in large catalogues.  In
order to include the SED dependence in the Galactic absorption
correction, Phosphoros follows the prescription provided by Galametz
et al. 2017 :cite:`Gal17` in their Appendix A. They show that the
total extinction :math:`A_{{\scriptscriptstyle SED},i}` for a given
filter can be robustly approximated as a linear function of the color
excess :math:`E^{\scriptscriptstyle MW}_{B-V}` when
:math:`E^{\scriptscriptstyle MW}_{B-V}\le0.3` (i.e., for the typically
values in the sky areas far from the Galactic Plane):

.. math::
   :label: ga4

   A_{{\scriptscriptstyle SED},i}(E^{\scriptscriptstyle MW}_{B-V})
   \simeq a_{{\scriptscriptstyle SED},i}\times
   E^{\scriptscriptstyle MW}_{B-V}\,.

The reddened flux can be again computed from Eq. :eq:`ga3`, with
:math:`A_{{\scriptscriptstyle SED},i}` depending on the source SED
through the parameter :math:`a_{{\scriptscriptstyle
SED},i}`. Practically, Phosphoros will generate a grid of coefficients
:math:`a_{{\scriptscriptstyle SED},i}` for each different pair of
{SED, filter} by computing the exact value of
:math:`A_{{\scriptscriptstyle SED},i}` for
:math:`E^{\scriptscriptstyle MW}_{B-V}=0.3` from Eq. :eq:`ga2`, and
setting :math:`a_{{\scriptscriptstyle SED},i}=A_{{\scriptscriptstyle
SED},i}(0.3)/0.3`.


.. note::

   The SED dependence of Galactic absorption is commonly neglected,
   and Galactic total extinction is approximated by
   :math:`A_i=E^{\scriptscriptstyle MW}_{B-V}k_{pivot}`, where
   :math:`k_{pivot}` is the value of the Galactic absorption law at an
   adopted pivot wavelength :math:`\lambda_{pivot}` of the filter
   [#f2meth]_.
   However, as discussed by Galametz et al. 2017,
   neglecting the SED dependence can significantly affect photometric
   redshifts estimates. Using a mock flux catalog of sources, they
   show that photometric redshifts can be biased by a factor
   :math:`\Delta z\gtrsim2-3\times10^{-3}(1+z)` when the
   :math:`k_{pivot}` approximation is applied. Although small, this is
   relevant for *Euclid* that requires unbiased photometric redshifts
   at the level of :math:`<2\times10^{-3}(1+z)` :cite:`Lau11`.
   We have verified that the Galactic absorption correction used in
   Phosphoros does not introduce any significant bias in photometric
   redshift estimates.


The Galactic absorption correction requires the knowledge of the Milky
Way absorption law, :math:`k_{\scriptscriptstyle MW}(\lambda)`, and of
the value of the color excess along the line of sight of each
source. Phosphoros adopts the absorption law from Fitzpatrick 1999
:cite:`Fit99`, which is calibrated using colour excesses from main
sequence B5 stars, :math:`E^{\scriptscriptstyle B5}_{B-V}`.

Phosphoros allows two options to provide color excess values:

* the user can input the :math:`E^{\scriptscriptstyle MW}_{B-V}` value
  associated at each source as one of the columns of the photometric
  catalog;

* Phosphoros can fetch :math:`E^{\scriptscriptstyle MW}_{B-V}`
  directly from the reddening map provided by *Planck*
  :cite:`Planck14`.

.. warning::

   The absorption law :math:`k_{\scriptscriptstyle MW}(\lambda)` used
   in Phosphoros is calibrated by main sequence B5 stars. If Galactic
   color excess :math:`E^{\scriptscriptstyle MW}_{B-V}` is derived
   from different sources (e.g., *Planck* data use reddening
   measurements of quasars), :math:`E^{\scriptscriptstyle MW}_{B-V}`
   values have to be scaled by the band-pass correction (see Galametz
   et al. 2017). This is a small effect and it is taken into account
   by Phosphoros for *Planck* data: in this case, the band-pass
   correction is :math:`E^{\scriptscriptstyle
   B5}_{B-V}=E^{\scriptscriptstyle Planck}_{B-V}\times1.018`. On the
   contrary, color excess from the Schlegel et al. :cite:`Sch98`
   Galactic reddening map does not require any band-pass correction.

.. *(tbc?)*

.. in Schlegel+98 paper, they say that they use elliptical galaxies, not B5 stars!!


.. note::
   The Galactic absorption correction is an optional functionality in
   Phosphoros that can be switched off by users.


.. _template-fitting:

Template fitting method
==============================

As first step, Phosphoros builds a grid of modeled photometry: this
consists of one photometric value for each selected filter, spanning
over all possible model parameters. The parameters are: redshift
:math:`z`, restframe SED template, color excess :math:`E_{B-V}` and
reddening curve :math:`k(\lambda)` (the last two paramteres are
related to intrinsic dust absorption).

The next step is to compute, for each source of the input catalog, the
likelihood :math:`\mathcal{L}` that observed photometry are described
by a model :math:`m`. This is done via a standard :math:`\chi^2`
method:

.. math::
   :label: tfm1 

   \ln(\mathcal{L}) = -\frac{\chi^2}{2} =
   -\frac{1}{2}\sum_i\bigg(\frac{f_{obs}^i-\alpha
   f_m^i}{\sigma_i}\bigg)^2\,.

The sum is over the number of selected photometric bands.
:math:`f_{obs}^i` and :math:`f_m^i` are the observed and modeled flux
for the filter :math:`i`, while :math:`\sigma_i` is the error
associated with the observed flux. The :math:`\chi^2` reflects the
discrepancies between the observed fluxes and a given model. The
smallest :math:`\chi^2` among the grid of models can therefore
determine the best-fit model and consequently the photometric
redshift of a source.

The normalization factor (or **scale factor**) :math:`\alpha` present
in the above equation is an additional free parameter of the model. In
order to reduce the number of free parameters and to be faster, by
default Phosphoros fixes :math:`\alpha` to the value that minimize
the :math:`\chi^2`. This value can be derived analytically by:

.. math::
   :label: tfm1a

   \alpha = \sum_i \frac{f_{obs}^if_m^i}{\sigma_i^2} \bigg/ \sum_i
   \frac{(f_m^i)^2}{\sigma_i^2}\,.

However, in Phosphoros users have the option to sample different
values of :math:`\alpha` and to derive the redshift PDF after the
scale factor marginalization.
   
Input catalogs may contain **missing data**, i.e. sources not imaged
in one or more filters. Phosphoros simply ignores those filters in the
previous formulas.

..
   note

   The user has to specify what value is used in the input catalog to
   identify missing data. It must be a number (e.g., -99, 0,
   etc.). Symbolic values as *NaN*, *NULL* or *INF* are not accepted
   by Phosphoros.

Multi-band catalogue can also include **upper limits** of source
fluxes. This occurs when sources are not detected in one or more
images due to their low fluxes. Upper limits are taken into account by
Phosphoros in the :math:`\chi^2` calculation following the Sawicki's
:cite:`Saw12` recipe (see their Appendix and the Phosphoros paper
:cite:`PP22`) [#f4meth]_.

..
   note

   Flux upper limits have to be associated with negative errors in
   order to be identified by Phosphoros.

.. _bayesian-priors:

Bayesian inference and Priors
====================================

In the maximum-likelihood method, the best-fit model corresponds to
the model that minimizes the :math:`\chi^2`. However, in many cases
there are additional information, not taken into account in the
likelihood, that could potentially help to have a more accurate model
selection. For instance, it may be known from previous experience that
one of the possible redshift/galaxy type combinations is much more likely
than any other, given the galaxy magnitude.

**Bayesian inference** allows us to include additional information on
model parameters, known a priori (**priors**). In this framework, the
best model is estimated by finding the posterior probability
distribution :math:`p(m|\mathbf{F}, \mathcal{P})`, i.e. the
probability of a galaxy to be described by the model :math:`m` given
the observed photometry :math:`\mathbf{F}` and the prior information
:math:`\mathcal{P}`. Applying the Bayes' theorem,

.. math::

   p(m|\mathbf{F}, \mathcal{P}) \propto
   \mathcal{L}(\mathbf{F}|m)\,\mathcal{P}(m)\,,

where :math:`\mathcal{L}(\mathbf{F}|m)` is the likelihood previously
defined in Eq. :eq:`tfm1` and :math:`\mathcal{P}(m)` is the prior
probability distribution for a model :math:`m`.

For simplicity, in the following discussion, we will neglect the model
parameters :math:`E_{B-V}` and reddening curve. Moreover, because
priors are usually known with respect to the galaxy
spectral/morphological type (e.g., elliptical, spiral, starburst
galaxies), we will talk about galaxy types :math:`T` instead of SED
templates. Hereafter, a model is just reduced to
:math:`m=\{z,\,T\}`. The discussion can be easily extended to the rest
of model parameters.

The main output of Phosphoros is the **redshfit probability density
function**, :math:`PDF(z)`. In absence of priors, this is simply
:math:`PDF(z)\equiv\mathcal{L}(\mathbf{F}|z)`; with priors, the
:math:`PDF(z)` is the posterior distribution for :math:`z`,
:math:`PDF(z)\equiv p(z|\mathbf{F},\mathcal{P})`. This is obtained by
projecting the posterior distribution to the :math:`z` axis. In the
reduced parameter space, it is:

.. math::

   PDF(z)\equiv p(z|\mathbf{F},\mathcal{P})=\sum_{T}
   p(z,T|\mathbf{F},\mathcal{P})
   \propto\sum_{T}\mathcal{L}(\mathbf{F}|z,T)\,\mathcal{P}(z|T)
   \,\mathcal{P}(T)\,,

where the priors :math:`\mathcal{P}(T)` and :math:`\mathcal{P}(z|T)`
correspond, respectively, to the fraction of :math:`T`--type galaxies
and their redshift distribution.

.. note::

   Phosphoros can compute 1D probability density functions for all the
   model parameters, in the similar way as for the redshift PDF.

.. note::

   As discussed above, the likelihood is typically computed by fixing
   the normalization factor :math:`\alpha` with the value that
   minimizes the :math:`\chi^2` for a given model. However, in a fully
   Bayesian approach, the template normalization :math:`\alpha` should
   be considered as an additional model parameter. In this case, the
   redshift :math:`PDF` is derived by marginalizing over the
   :math:`\alpha` parameter too:

   .. math::

      p(z|\mathbf{F},\mathcal{P})=\sum_{T}\int d\alpha\,
      p(z,T,\alpha|\mathbf{F},\mathcal{P}) 
      \propto\sum_{T}\int d\alpha\,
      \mathcal{L}(\mathbf{F}|z,T,\alpha)\,\mathcal{P}(z,T)\,.

   where we have assumed a flat prior for :math:`\alpha`, and that the
   galaxy redshift and type do not depend on :math:`\alpha`.    

.. note::

   In the above discussion SED templates are grouped in different
   types, whose priors correspond to their fractions with respect to
   the total family of galaxies. Phosphoros deals with the issue of
   the weight of each template within a galaxy type too. It assigns in
   fact a weight to each SED template based on the volume of the color
   space uniquely covered by the template (see the Phosphoros paper
   :cite:`PP22` and :ref:`Advanced features: SED weights<sed-weight>`
   for more details).
   
Phosphoros provides some default prior functionalities that can be
applied to the likelihood of models. They consist in priors on the
source luminosity, redshift distribution and volume, and they are the
topic of the next sub-sections. In addition, Phosphoros allows users
to introduce their own pre-computed priors on one or multiple model
parameters (see :ref:`Advanced features: Ganeric
Prios<generic-priors>` ).

.. _redshift-dist:

Redshift distribution
-------------------------------------

Prior information are often given in terms of redshift
distribution for galaxies with apparent magnitude :math:`m_0`, i.e.
:math:`p(z|m_0)` (see, e.g., Benitez et al. 2000 :cite:`Ben00`). The
prior can include information such as the existence of upper or lower
limits on galaxy redshifts, or discriminate values of redshifts
that are considered less or more probable with respect to other ones.

Because galaxies belonging to different morphological/spectral types
may have different distributions in redshift, the prior definition is
usually *expanded* into the probability :math:`\mathcal{P}(z,T|m_0)`,
i.e. the probability of the galaxy redshift being *z* and the galaxy
type being *T* given an apparent magnitude :math:`m_0`. It follows
that

.. math::

   \mathcal{P}(z,T|m_0)=\mathcal{P}(T|m_0)\,\mathcal{P}(z|T,m_0)\,,

where :math:`\mathcal{P}(T|m_0)` is the galaxy type fraction as a
function of magnitude, and :math:`\mathcal{P}(z|T,m_0)` is the prior
information on the redshift distribution for galaxies of the given
type and magnitude. The redshift :math:`PDF` is then expressed in
terms of prior distributions as:

.. math::

   PDF(z) \propto\sum_T \mathcal{L}(\mathbf{F}|z,T)\,p(z,T|m_0)
   =\sum_T \mathcal{L}(\mathbf{F}|z,T)\,\mathcal{P}(T|m_0)
   \,\mathcal{P}(z|T,m_0)\,.

See the :ref:`redshift-prior` section for a detailed explanation of
redshift priors in Phosphoros and of their use.

   
Volume correction
---------------------------
      
Phosphoros implements the so called *volume correction*. This
prior information takes into account the fact that a survey covers
larger volumes of the Universe at higher redshift than at lower
redshift, and consequently gives higher probability to find a galaxy
at higher redshift. The prior distribution depends only on redshift
and is given by:

.. math::
   
   \mathcal{P}(z)\propto \frac{dV_c}{dz} = 4\pi D_c^2\frac{dD_c}{dz}\,,

where :math:`D_c~(V_c)` is the comoving distance (volume) at redshift
:math:`z`.
      
See the :ref:`volume-prior` section for its use in Phosphoros.


Luminosity functions
---------------------------------

Another example of prior information implemented by Phosphoros is
given by the galaxy luminosity function, :math:`\phi(L_b,z)`. Luminosity
functions can be seen in fact as a probability function, i.e. the
probability for a source to have a particular luminosity at a given
redshift. Here, the luminosity :math:`L_b` refers to the intrinsic
luminosity (or, equivalently, the magnitude) integrated over a
specific observational band :math:`b`.

For each model of the parameter space we can compute the luminosity in
the :math:`b` band, :math:`L_{b,m}`, and consequently, through the
luminosity function, the prior probability for that model. If luminosity
functions are known over the full redshift range and for all galaxy
types, the redshift :math:`PDF` with luminosity priors becomes:

.. math::
   :label: tfm2 

   PDF(z)\equiv p(z|\mathbf{F},\mathcal{P})=\sum_{T}
   p(z,T|\mathbf{F},\mathcal{P})\propto\sum_{T}
   \mathcal{L}(\mathbf{F}|z,T)\,\phi_{z,T}(L_{b,m})\,,

where :math:`\phi_{z,T}` is the luminosity function of :math:`T`--type
galaxies at redshift :math:`z`. In the above equation, we have assumed
:math:`\phi_{z,T}(L_{b,m})=\mathcal{P}(z,T)` and a uniform prior for
:math:`\mathcal{P}(T)`.

We refer users to the :ref:`luminosity-prior` section for a detailed
explanation on luminosity priors in Phosphoros and their use.

.. note

   In Phosphoros, volume correction is automatically added to luminosity priors.

.. note

   The band at which luminosity functions are known/provided has not
   to be necessarily one of the observational bands of the input
   catalog.


.. rubric :: Footnotes

.. [#f1meth] see https://heasarc.gsfc.nasa.gov/xanadu/xspec/models/zigm.html 

.. [#f3meth] see https://http://svo2.cab.inta-csic.es/svo/theory/fps3/

.. [#f2meth] A typical way to define the filter pivot wavelength is
   :math:`\lambda_{pivot}=\sqrt{\int\lambda T_i d\lambda/\int T_i
   d\lambda/\lambda}`, where :math:`T_i` is the trasmission curve of
   filter :math:`i`.

.. [#f4meth] Equation A10 of Sawicki et al. is modified in Phosphoros
   in order to avoid negative values of :math:`\chi^2`, replacing the
   factor :math:`\sqrt{\pi/2}\sigma_j` in the second term of the
   equation by 0.5.

	     
.. bibliography:: references.bib
