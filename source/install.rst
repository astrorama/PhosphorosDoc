.. _phosphoros-install:

*************************
Download and Installation
*************************

Instructions on how to download and install the the Phosphoros software suite ar eprovided for

- :ref:`MacOSX_section`

- :ref:`Linux_section`

- :ref:`Source_Installation_section`

.. _MacOSX_section:

Mac OSX
=======

Phosphoros installation  with the MacPorts tool. MacPorts installation
instructions can be found on this
`link <https://www.macports.org/install.php>`_.  If you do not want to use
MacPorts, please try the source code installation instructions provided
below.

Phosphoros MacPorts installation:

#. Update the source location by adding the following line:  
      | "http://degauden.isdc.unige.ch/euclid/repo/macports/ports.tar.gz" 
      | into  the 
      | "/opt/local/etc/macports/sources.conf" 
      | configuration file
#. Synchronized the port DataBase with:
      | > sudo port sync 
#. Install the software: 
      | > sudo port install Phosphoros-0.5
#. Test your installation by launching the GUI with:  
      | > Phosphoros GUI

.. _Linux_section:

Linux
=====

We support CentOS7 and Fedora 22 with RPM packages.

CentOS7

   #. Download http://degauden.isdc.unige.ch/euclid/repo/centos/7/x86_64/EuclidRepo-0.1-1.noarch.rpm
   #. Execute: 
         | > sudo yum localinstall EuclidRepo-0.1-1.noarch
   #. Install Phosphoros:
         | > sudo yum install Phosphoros-devel

Fedora 22

   #. Download http://degauden.isdc.unige.ch/euclid/repo/fedora/22/x86_64/EuclidRepo-0.1-1.noarch.rpm
   #. Execute: 
         | > sudo yum localinstall EuclidRepo-0.1-1.noarch
   #. Install Phosphoros:
         | > sudo yum install Phosphoros-devel

For other Linux distributions, you have to install the software from the sources as described in the next section.

.. _Source_Installation_section:

Installation from sources
=========================

Instructions to install the Elements software from the source code are provided in the following page 

http://euclid.roe.ac.uk/projects/elements/wiki/InstallFromSource

Please be aware than the last 3 steps of this procedure (i.e., download, configure, build, install) have to be repeated for 

- Alexandria-2.5
- PhosphorosCore-0.5
- Phosphoros-0.5

(replacing "Elements-4.0" by the above names in the procedure)

Python packages
===============

To run some of the investigation tool, you will need the following Python packages

- NumPy
- Astropy
- Mathplotlib

