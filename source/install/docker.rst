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
this you can follow the instructions at the `docker documentation
<https://docs.docker.com/install/>`_, which
provides instructions and downloads for all operating systems (you should get
the community edition, which is free).


.. warning:: If you have any of the following OS, you should follow the
             instructions bellow instead:

Mac OS Sierra 10.12 or earlier
------------------------------

The Mac version of the docker requires a recent kernel, so it can run only under
High Sierra 10.13 and later. If you have any earlier Mac OS X version you can
still use docker by installing the `docker toolbox
<https://docs.docker.com/toolbox/toolbox_install_mac/>`_.

Fedora
------

The instructions at the docker store will guide you to setup the official docker
repository, from where you can retrieve the latest docker version. The Fedora
repositories though already contain a docker package (which is available also
for older OS versions). If you do not want to setup third party repositories you
can use the already available package using the following commands:
::

    # install the docker
    sudo dnf install docker     # For Fedora 29 or newer

    # start the docker daemon
    sudo systemctl start docker

    # set docker daemon to start at startup
    sudo systemctl enable docker

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


.. _docker_extra_mac:

Extra docker instructions for Mac OS users
==========================================

X Window System
---------------

Phosphoros container is going to redirect the X windows to your machine. To be
able to handle this redirection you need to install an X Window System, if you
don't already have one. You can do this by installing `XQuartz <https://www.xquartz.org/>`_.

Docker toolbox: Error when starting up
--------------------------------------

If you get an error like this:
::

    Machine does not have a host-only adapter


Make sure you grant permission to VirtualBox under "System Preferences > Security
and Privacy > General", restart VirtualBox:

::

    sudo "/Library/Application Support/VirtualBox/LaunchDaemons/VirtualBoxStartup.sh" restart

And try again.

.. warning:: You may have to remove the created virtual machine (named default),
  and let Docker toolbox to recreate it again.


Directory permissions
---------------------

Docker for Mac by default limits the directories accessible to the containers to
a small list (`/Users`, `/Volumes`, `/tmp` and `/private`). This list can be
extended, so you can access any directory you want from the Phosphoros docker
(to see more details about how to mount the directories refert the
:ref:`docker_mount_extra_dirs` section). To extend the list of accessible
directories just do the following:

#. Click the small docker icon at your menu bar (top right)
#. Select `Preferences...` from the menu
#. Select the tab `File Sharing`
#. Click the `+` button and add your directory
#. Click the `Apply & Restart` button

For more information about the File Sharing and the rest of the Mac preferences
see the official documentation `here <https://docs.docker.com/docker-for-mac/#preferences>`_.


Installing DockerPhosphoros
===========================

The DockerPhosphoros is the tool which manages the Phosphoros docker container.
To install it you will need pip installed on your system, which is normally
available on Linux systems, and on MacOSX if you have Conda.
::

    python3 -m pip install --user DockPhos

.. warning:: If you use Mac and you copy the files outside your home directory
             (for example in a directory under /Applications) you must make this
             directory accessible to docker, as described at the
             :ref:`docker_extra_mac` section

.. warning:: Python 2 EOL is January 1st 2020. The script is still compatible
             with it, but we strongly recommend to use Python 3.



Using DockerPhosphoros
======================

Starting the container
----------------------

Using DockerPhosphoros is straight forward. First you have to start the
Phosphoros docker container (which will continue running in the background):
::

    DockPhos start

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

.. tip:: When using Docker toolbox, you may want to override the temporary
         directory used (--temp_dir), as the default one may not work correctly.

Using a different Phosphoros root directory
-------------------------------------------

If you do not want to use the default Phosphoros root directory you can either
set the environment variable ``PHOSPHOROS_ROOT`` or your can pass the ``-d``
option to the start command:
::

    DockPhos start -d /your/phosphoros/root/dir

If the Phosphoros container was already running it will be restarted and the new
directory will be mounted. Again, the directory must already exist, otherwise
you will get an error message.

Connecting to the container
---------------------------

To connect to the container you can use the following command:
::

    DockPhos connect

After running this command your terminal will be moved in the Phosphoros
container. From there you can use any of the Phosphoros commands, for example:
::

    Phosphoros GUI

.. tip:: You can run the ``DockPhos connect`` command in multiple terminals
         and all of them will connect to the same container

Note that when you are inside the container you are the user phosphoros and that
the Phosphoros root directory is mounted under ``/home/phosphoros/Phosphoros``.

.. warning:: The filesystem of the container is **NOT** the same with yours!
             Your local files will not be accessible from inside the container.
             The only exception is the Phosphoros root directory, which can be
             used to move files in and out of the container.

When you want to leave the container and return back to your machine you can
just type ``exit``. This will not stop the Phosphoros container. You can
re-connect using the ``DockPhos connect`` command.

Choosing the Phosphoros version
-------------------------------

By default, when run the start command the latest stable version of Phosphoros
is used. If you want to use a different version you can
use the -v option when you start the container:
::

    DockPhos start -v <VERSION>

If you want to get a list of all the available versions you can run the
command:
::

    DockPhos versions

Stopping the container
----------------------

After you finish you work and you exit the container using the ``exit`` command
you can stop stop the Phosphoros container to release your resources by running:
::

    DockPhos stop


Advanced options
================

.. _docker_mount_extra_dirs:

Mounting extra directories
--------------------------

Sometimes you might need to have access to files which are not under the
Phosphoros root directory. To mount extra directories, so they are visible from
inside the container you can use the ``-m`` option when you start the container:
::

    DockPhos start -m /directory/to/mount

All directories mounted this way are visible in the container under the
``/mount`` directory and they contain the full absolute path of the mounted
directory. For example, the above command will make the directory available
inside the container user:
::

    /mount/directory/to/mount

If the directory path is too long, you can use an alias name, by prefixing the
directory to mount with ``:`` and the alias name:
::

    DockPhos start -m /directory/to/mount:mydir

The above command will make the directory available inside the container under:
::

    /mount/mydir

If you want to mount more than one directories you can pass multiple directories
to the ``-m`` option, separated by space:
::

    DockPhos start -m /first/dir/to/mount:first /second/dir/to/mount:second

Deleting the Phosphoros docker images
-------------------------------------

When you run the ``DockPhos start`` command, docker will download from the
internet the Phosphoros docker images. The location where these files are stored
depends on the OS and they are managed by the docker itself. If you want to
delete all these images to get back your disk space you can run the command:
..

    DockPhos cleanup

.. tip:: The next time you start the docker container the images will be
         re-downloaded automatically

Minimizing the disk space usage
-------------------------------

The Phosphoros docker images can be quite big. This is to support extra
functionality above the core Phosphoros. If you have disk space limitations and
you are not interested on the post-processing functionality you can use the
``-l`` option when you start the container, which will download a smaller image:
::

    DockPhos start -l <LABEL>

The currently available labels are the following:

- **topcat** (576 MB): Contains all the Phosphoros tools as well as topcat for
  examining the results (see :ref:`connecting-with-topcat` for more details).
  This is the default and recommended option.

- **full** (484 MB): Contains all the Phosphoros tools except the topcat (topcat
  and JRE are removed).

- **light** (408 MB): Contains the Phosphoros GUI and all the core functionality.
  numpy, astropy and matplotlib are removed, so most of the post processing
  Phosphoros tools will not work.

- **cli** (340 MB): The smallest available image. Only the core functionality of
  the Phosphoros can be used from the command line.

The size between parenthesis are the compressed size.

To get a full up to date list of the available labels you can use the command:
::

    DockPhos labels


Retrieving the status of the Phosphoros container
-------------------------------------------------

To check if the Phosphoros docker container is already running and to get
information about it you can run the command:
::

    DockPhos status

If the container is running, this command will return its docker ID, the local
port used to connect to the container and all the directories mounted.

Fixing the container port
-------------------------

When docker starts the Phosphoros container it assigns a port to it randomly.
If you want to fix the port number used (for example to setup a firewall) you
can use the ``-p`` option:
::

    DockPhos start -p <PORT_NO>
