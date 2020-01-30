.. _axis-collapse:

Axis Collapse options
=====================

Once the likelihood and the posterior distributions of models are
computed, Phosphoros can derive the one-dimensional probability
density function (PDF) of model parameters (see the :ref:`Template
Fitting Method <template-fitting>` section). The common example is the
redshift PDF. This is done by projecting, e.g., the likelihood
distribution to the axis of the model parameter for which the PDF is
required.

Three possible techniques are implemented in Phosphoros:

* (``BAYESIAN``) **Marginalization**. Likelihood distributions are
  integrated or summed (for categorial variables, i.e. the SED
  templates and the reddening curves) over all the possible values of
  the *nuisance* parameters. This is the default option.

* (``MAX``) **Maximum likelihood** method. The PDF of a chosen
  parameter is determined from the maximum probability corresponding
  to each value of that parameter.

* (``SUM``) **Summing** method. Likelihood distributions are added up
  over all the *nuisance* parameters. This method differs from
  marginalization when the grid of models for numerical variables is
  not uniformly sampled. Each point of the grid is assumed with the
  same weight.

**In the GUI mode**

In the sub-panel ``3. Algorithm Options`` of the ``Compute Redshifts``
window, users can select the axis collapse options for the likelihood
and the posterior distribution.

.. Click on the ``Normalize 1D PDF`` tab in order to compute normalized PDFs.

**In the CLI mode**

By default Phosphoros uses the ``BAYESIAN`` method for collapsing axes
when 1D PDFs are produced. The other methods can be chosen using the
command options ``--axes-collapse-type`` for posterior
distributions and ``--likelihood-axes-collapse-type`` for
likelihood distributions::

  > Phosphoros CR --axes-collapse-type <arg> --likelihood-axes-collapse-type <arg> ....

where ``<arg>`` is one of ``SUM``, ``MAX``, ``BAYESIAN``.

..
   in CLI la normalizzazione si toglie con --output-pdf-normalized arg (=YES)
