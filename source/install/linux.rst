.. _linux-install:

******************
Linux installation
******************

We support CentOS7 and Fedora 23 installations with RPM packages.

CentOS7

   #. Download http://degauden.isdc.unige.ch/euclid/repo/centos/7/x86_64/EuclidRepo-0.1-1.noarch.rpm
   #. Execute: 
         | > sudo yum localinstall EuclidRepo-0.1-1.noarch.rpm
   #. Install Phosphoros:
         | > sudo yum install Phosphoros-devel

Fedora 23

   #. Download http://degauden.isdc.unige.ch/euclid/repo/fedora/23/x86_64/EuclidRepo-0.1-1.noarch.rpm
   #. Execute: 
         | > sudo dnf install EuclidRepo-0.1-1.noarch.rpm
         | > sudo dnf install gcc-c++
         | > sudo dnf install EuclidEnv

   #. Reboot your computer

   #. Install Phosphoros with:
        | > sudo dnf install Phosphoros-devel

   #. Create an alias by adding the following line in your .bashrc file:
        | alias Phosphoros="ERun Phosphoros |version| Phosphoros"

   #. Test your installation by launching the GUI with:
        | > Phosphoros GUI

Please note that you will have to update the alias every time you update to a new version of
Phosphoros.

For other Linux distributions, you have to install the software from the sources as described in the next section.

.. When installing via the RPMs, you will have to run Phosphoros using the `ERun`
   command. For your convenience, you can create an alias to the Phosphoros command by adding the following line in your .bashrc file::
    
    alias Phosphoros="ERun Phosphoros 0.5 Phosphoros"

    where you have to replace the version with the one you just installed. Note that
    you will have to update the alias every time you update to a new version of Phosphoros.