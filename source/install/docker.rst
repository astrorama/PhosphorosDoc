.. _docker-installation:

************************************
Installation using docker containers
************************************

This section contains instructions of how to use the Phosphoros software via
docker containers. This method allows to run Phosphoros under any operating
system and is the recommended way of installation.

.. contents:: Table of Contents
    :local:


Installing docker
=================

Docker store (all OS)
---------------------

If you do not have docker already installed, you will have to install it. To do
this you can follow the instructions at the `docker store
<https://store.docker.com/search?offering=community&q=&type=edition>`_, which
provides instructions and downloads for all operating systems (you should get
the community edition, which is free).


.. warning:: If you have any of the following OS, you should follow the
             instructions bellow instead:

Mac OS Yosemite 10.10.2 or earlier
----------------------------------

The Mac version of the docker requires a recent kernel, so it can run only under
Yosemite 10.10.3 and later. If you have any earlier Mac OS X version you can
still use docker by installing the `docker toolbox
<https://www.docker.com/products/docker-toolbox>`_.

Fedora
------

The instructions at the docker store will guide you to setup the official docker
repository, from where you can retrieve the latest docker version. The Fedora
repositories though already contain a docker package (which is available also
for older OS versions). If you do not want to setup third party repositories you
can use the already available package using the following commands:
::

    # install the docker
    sudo dnf install docker     # For Fedora 24 or newer
    sudo dnf install docker-io  # For Fedora 22 or 24
    sudo yum install docker-io  # For Fedora 21 or older

    # start the docker daemon
    sudo systemctl start docker

    # set docker daemon to start at startup
    sudo systemctl enable docker

.. note:: If you decide to install docker using the instructions at the docker
          store, note that there is a typo in the repository url. You should use
          ``https://download.docker.com/linux/fedora/docker-ce.repo`` instead.

.. warning:: The docker store version supports only Fedora 24 or newer. If you
             have an older version you must follow the above instructions.

Ubuntu
------

The instructions at the docker store will guide you to setup the official docker
repository, from where you can retrieve the latest docker version. The Ubuntu
repositories though already contain a docker package (which is available also
for older OS versions). If you do not want to setup third party repositories you
can use the already available package using the following commands:
::

    # install the docker
    sudo apt-get install docker.io

    # start the docker daemon
    sudo systemctl start docker  # For Ubuntu 16.04 or newer
    sudo service docker start    # For older versions

    # set docker daemon to start at startup
    sudo systemctl enable docker # For Ubuntu 16.04 or newer
    update-rc.d docker defaults  # For older versions

.. warning:: The docker store version supports only Ubuntu Trusty or newer. If
             you have an older version you must follow the above instructions.


Extra docker instructions for Linux users
=========================================

For security reasons, the Linux systems require root privileges for contacting
the docker daemon. The Phosphoros container though should **NOT** be executed as
root (or using sudo). To fix this problem you should add your user in a group
called `docker`, using the following commands:
::

    # Create a group named docker
    sudo groupadd docker

    # Add your user to the group
    sudo gpasswd -a ${USER} docker

After these commands you need to restart your system, so the changes are applied
to both the system and the docker daemon. After that, your user will be able to
use the Phosphoros container.


Installing DockerPhosphoros
===========================

The DockerPhosphoros is the tool which manages the Phosphoros docker container.
To install it you have to perform the following steps:

- Download the `zip file <http://www.isdc.unige.ch/euclid/phosphoros/data/other/DockerPhosphoros.zip>`_
  which contains the DockerPhosphoros directory with two files
- Unzip it in any directory you want
- Add the DockerPhosphoros directory in your PATH environment variable

Note that the last step is optional and that you can run the script using its
path (like ``./DockPhos.py``).

.. tip:: You can copy the files anywhere you like as long as they are in the
         same directory

The only dependency of the DockerPhosphoros tool is that you must have python
installed in your system. The tool is compatible with both Python2 and Python3
versions.


Using DockerPhosphoros
======================

Starting the container
----------------------

Using DockerPhosphoros is straight forward. First you have to start the
Phosphoros docker container (which will continue running in the background):
::

    DockPhos.py start

Note that when you start the container, your Phosphoros root directory will be
mounted to the container (see :ref:`directory-organization` for more information
of what this directory is). If this directory does not exist (by default is the
directory ``Phosphoros`` under your home directory) the container will not start
and you will get an error message. To fix this you just have to create the
directory.

.. tip:: The first time you start the Phosphoros docker container, it will be
         downloaded from the internet. This may take a while, so be patient. The
         next time you start the container everything will be available locally
         and it will start much faster.

Using a different Phosphoros root directory
-------------------------------------------

If you do not want to use the default Phosphoros root directory you can either
set the environment variable ``PHOSPHOROS_ROOT`` or your can pass the ``-d``
option to the start command:
::

    DockPhos.py start -d /your/phosphoros/root/dir

If the Phosphoros container was already running it will be restarted and the new
directory will be mounted. Again, the directory must already exist, otherwise
you will get an error message.

Connecting to the container
---------------------------

To connect to the container you can use the following command:
::

    DockPhos.py connect

After running this command your terminal will be moved in the Phosphoros
container. From there you can use any of the Phosphoros commands, for example:
::

    Phosphoros GUI

.. tip:: You can run the ``DockPhos.py connect`` command in multiple terminals
         and all of them will connect to the same container

Note that when you are inside the container you are the user phosphoros and that
the Phosphoros root directory is mounted under ``/home/phosphoros/Phosphoros``.

.. warning:: The filesystem of the container is **NOT** the same with yours!
             Your local files will not be accessible from inside the container.
             The only exception is the Phosphoros root directory, which can be
             used to move files in and out of the container.

When you want to leave the container and return back to your machine you can
just type ``exit``. This will not stop the Phosphoros container. You can
re-connect using the ``DockPhos.py connect`` command.

Choosing the Phosphoros version
-------------------------------

By default, when run the start command the latest stable version of Phosphoros
is used (currently |version|). If you want to use a different version you can
use the -v option when you start the container:
::

    DockPhos.py start -v <VERSION>

If you want to get a list of all the available versions you can run the
command:
::

    DockPhos.py versions

Stopping the container
----------------------

After you finish you work and you exit the container using the ``exit`` command
you can stop stop the Phosphoros container to release your resources by running:
::

    DockPhos.py stop


Advanced options
================

Mounting extra directories
--------------------------

Sometimes you might need to have access to files which are not under the
Phosphoros root directory. To mount extra directories, so they are visible from
inside the container you can use the ``-m`` option when you start the container:
::

    DockPhos.py start -m /directory/to/mount

All directories mounted this way are visible in the container under the
``/mount`` directory and they contain the full absolute path of the mounted
directory. For example, the above command will make the directory available
inside the container user:
::

    /mount/directory/to/mount

If the directory path is too long, you can use an alias name, by prefixing the
directory to mount with ``:`` and the alias name:
::

    DockPhos.py start -m /directory/to/mount:mydir

The above command will make the directory available inside the container under:
::

    /mount/mydir

If you want to mount more than one directories you can pass multiple directories
to the ``-m`` option, separated by space:
::

    DockPhos.py start -m /first/dir/to/mount:first /second/dir/to/mount:second

Deleting the Phosphoros docker images
-------------------------------------

When you run the ``DockPhos.py start`` command, docker will download from the
internet the Phosphoros docker images. The location where these files are stored
depends on the OS and they are managed by the docker itself. If you want to
delete all these images to get back your disk space you can run the command:
..

    DockPhos.py cleanup

.. tip:: The next time you start the docker container the images will be
         re-downloaded automatically

Minimizing the disk space usage
-------------------------------

The Phosphoros docker images can be quite big. This is to support extra
functionality above the core Phosphoros. If you have disk space limitations and
you are not interested on the post-processing functionality you can use the
``-l`` option when you start the container, which will download a smaller image:
::

    DockPhos.py start -l <LABEL>

The currently available labels are the following:

- **topcat** (1.07 GB): Contains all the Phosphoros tools as well as topcat for
  examining the results (see :ref:`connecting-with-topcat` for more details).
  This is the default and recommended option.

- **full** (870 MB): Contains all the Phosphoros tools except the topcat (topcat
  and JRE are removed).

- **light** (491 MB): Contains the Phosphoros GUI and all the core functionality.
  numpy, astropy and matplotlib are removed, so most of the post processing
  Phosphoros tools will not work.

- **cli** (386 MB): The smallest available image. Only the core functionality of
  the Phosphoros can be used from the command line.

To get a full up to date list of the available labels you can use the command:
::

    DockPhos.py labels

.. tip:: The Phosphoros docker images are built as a stack, so if you run
         multiple labels, the total size occupied will by only the size of the
         biggest one.

Retrieving the status of the Phosphoros container
-------------------------------------------------

To check if the Phosphoros docker container is already running and to get
information about it you can run the command:
::

    DockPhos.py status

If the container is running, this command will return its docker ID, the local
port used to connect to the container and all the directories mounted.

Fixing the container port
-------------------------

When docker starts the Phosphoros container it assigns a port to it randomly.
If you want to fix the port number used (for example to setup a firewall) you
can use the ``-p`` option:
::

    DockPhos.py start -p <PORT_NO>