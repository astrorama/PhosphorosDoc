
***************************
First Steps with Phosphoros
***************************

What is template fitting
========================

Small very basic description so the reader understands what the software tries
to do, but it should be a bit longer that what goes to the introduction chapter.

Phosphoros Workflow
===================

It starts with a paragraph explaining the three kind of steps: model grid generation,
optional steps and redshift computation.

Introduces the concept of the parameter space. Explains that the models are the
computed photometries.

This is at theoretical level. Diagrams should be used, files or directories not.

Directory organization
======================

Explain the logic behind the organization of the Phosphoros directories. This
should include the catalog-type concept. Here we should not explain every single
one of the directories, but focus more on the concept and mention the most used
ones. We should also mention the PHOSPHOROS_ROOT environment variable.

Executing Phosphoros
====================

This should be very similar with what we already have, but it must be updated
with the new config directory. It must not have a full list of the CLI commands.

Setting up the input data
=========================

First explain what the input data are. At this level we should limit it to the
catalogs, filters, SEDs and reddening curves. We should not describe the formats
of the files, but have links to the format reference section.

Import using the GUI
--------------------

Import manually
---------------

Mapping the catalog columns
===========================

Explain again the concept of the catalog type.

GUI how-to
----------

CLI how-to
----------

filter_mapping.txt explanation

Defining the model parameter space
==================================

Explain the concept of the parameter space. Specify the axes. Not mention yet
the sparse parameter space.

GUI how-to
----------

Show an example with multiple ranges and values

CLI how-to
----------

Explain the related configuration options, which map to the same example shown
at the GUI

Generating the models
=====================

GUI
---

Show the steps and explain where the file is created. Here we should explain the
color codes of the Compute Redshifts panel.

CLI
---

Show the command. Mention the default configuration file name. Explain where the
files are created (and the reasoning behind the default naming).

Computing the Redshift
======================

GUI
---

Show the steps and explain the different outputs (and link to the format descriptions).
Explain where the data are created. Have a link back to the directory organization
strucutre.

CLI
---

Show the command. Mention the default configuration file name. Show the same steps
as for the GUI.

Examining the results
=====================

Explain that there are some tools for that and that are available only at CLI.

Explain how to use the plot_specz_comparison for seeing the specz-phz plot and
the PDFs. Also mention the SAMP functionality.

