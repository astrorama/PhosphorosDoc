Computing the Redshift
======================

GUI
---

.. figure:: /_static/first_step/InputOutputSubpanel4.png
    :align: center

We focus here on the sub-panel four (red frame).
This is the last step for computing the photometric redshift. It provides you the
best fit catalog with the best fitted models for the catalog containing your sources. |br|

To do so, you need to define the followings:

1. **Input Catalog File**
 
 Provide a photometry catalog (FITS or ASCII format) containing your sources 
 (See format details **here**).
 
2. **Generate Output Catalog with Format**

 Select the output format of the Phosphoros best fitted catalog you wish. It is
 either ASCII or FITS format (See format details **here**).
 
3. **Generate Output PDF Files**
 
 Click the checkbox for generating the 1D PDF(z) files, one per source 
 (See format details **here**).

4. **Generate Likelihood Files**
 
 Click the checkbox for generating the likelihood FITS files, one per source 
 (See format details **here**).
 
5. **Generate Posterior Files**

 Click the checkbox for generating the posterios FITS files, one per source 
 (See format details **here**).
 
6. **Output Folder**
 
 The Phosphoros final results are stored there.The output folder is provided 
 by default and is located to::
 
 $PHOSPHOROS_ROOT/Results/Catalog/"Catalog Type" 
 
 But you can choose another location by clicking on the ``Browse`` button. The
 ``Catalog Type`` is the one defined at the top left of the image.

Note you do not need to go through all the points above. Select just the ones you
need. As shown in the above image the ``Run`` button is inactive for our example. It
means that something is not setup yet and the computing can not be done. In such
case, just put your mouse pointer on the ``Run`` button and some help will appear
explaining you what is missing

The bottom right buttons (the blue rectangle on the image):

- **Run**

 You click on this button to start the computation of the redshifts.
 All your results are written into the ``Output Folder`` you have defined.
 (see details about output files produced **here**)
 
- **Get Config File**

 Use this button to export your settings into a file.The file is stored into::
 
 $PHOSPHOROS_ROOT/config/"filename.txt" 
 
 (where ``filename.txt`` is the filename of your choice)

CLI
---

Show the command. Mention the default configuration file name. Show the same steps
as for the GUI.