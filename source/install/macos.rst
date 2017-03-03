.. _macos-install:

********************
Mac OSX installation
********************

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
      | > sudo port install Phosphoros-0.6
#. Test your installation by launching the GUI with:  
      | > Phosphoros GUI
