|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_


**Command Line Transfer with iCommands**
----------------------------------------

iCommands is a collection of tools developed by the `iRODS <https://irods.org/>`_
project. iRODS is the technology that supports the CyVerse Data Store. Using
iCommands is the most flexible way to interact with the Data Store.

.. #### Comment: short description

**Some things to remember about iCommands**

- This is a *command line* tool, operated in a terminal.
- Poor support for *Windows OS*: Currently, we have not tested a Windows-only shell version
  of iCommands. We do suggest installing `Windows Subsystem for Linux v2 <https://docs.microsoft.com/en-us/windows/wsl/wsl2-install>`_ and following the Linux installation instructions 

----

**iCommands Installation for Linux**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. On a linux OS you can use the package managers from iRODS to install in the termainal:

**CentOS:**

.. code:: bash

  yum install https://files.renci.org/pub/irods/releases/4.1.10/centos7/irods-icommands-4.1.10-centos7-x86_64.rpm

**Debian/Ubuntu 18.04 or older:**

.. code:: bash

  wget https://files.renci.org/pub/irods/releases/4.1.10/ubuntu14/irods-icommands-4.1.10-ubuntu14-x86_64.deb
  apt-get install ./irods-icommands-4.1.10-ubuntu14-x86_64.deb

**Ubuntu 20.04**

In Ubuntu 20.04, the iRODS iCommands package depends on the ``multiarch-support_2.27`` and
``libssl1.0.0`` packages that are no longer available in the base repositories. These need to be
downloaded and installed manually as in the following example.

.. code:: bash

  wget \
    http://mirrors.kernel.org/ubuntu/pool/main/g/glibc/multiarch-support_2.27-3ubuntu1.4_amd64.deb \
    http://ftp.se.debian.org/debian/pool/main/o/openssl/libssl1.0.0_1.0.1t-1+deb8u8_amd64.deb \
    https://files.renci.org/pub/irods/releases/4.1.10/ubuntu14/irods-icommands-4.1.10-ubuntu14-x86_64.deb

  dpkg --install \
    multiarch-support_2.27-3ubuntu1.4_amd64.deb \
    libssl1.0.0_1.0.1t-1+deb8u8_amd64.deb \
    irods-icommands-4.1.10-ubuntu14-x86_64.deb
 
**iCommands Installation for MAC OS X**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Download the CyVerse-Specific
   `iCommands Installer 4.1.9 <https://cyverse.atlassian.net/wiki/download/attachments/241869823/cyverse-icommands-4.1.9.pkg?version=3&modificationDate=1472820029000&cacheVersion=1&api=v2>`_
   
2. Open the file by locating it in your Finder, right click on it and select 'Open'. When opening the package, you may get a security warning noting the file is from "an unidentified developer". Alternately, go to the OS 'System Preferences' and select the 'Security & Privacy' menu. At the bottom of the menu,  there should be and 'Open Anyway' button that will allow you to procede. 


3. Follow the prompts to begin the installation. You will need to know your administrator password to install new software. 
 
.. note:: 

    Newer Mac OS X now ships with ``zsh`` as its default shell rather than ``bash``. The installer will attempt to write some environmental variables to the ``.bashrc`` file which for ``zsh`` is called the `.zshrc`.
    
    By default, this installation will place iCommands in your system ``PATH`` so you should be ready to run iCommands immediately at the terminal. If this does not happen (i.e. you get an error when trying to run ``iinit``), you can add the `icommands` path by editing your ``.zshrc`` file: 

    .. code:: bash

      # add iCommands Path
      export PATH="/Applications/icommands/:$PATH"
      export IRODS_PLUGINS_HOME=/Applications/icommands/plugins/

    and then in terminal source the file ``source ~/.zshrc``. 

----

**iCommands First-time Configuration**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note::
    If using iCommands in an HPC environment, which has many systems with iCommands installed, run the ``module load irods`` command to get access to iRODS iCommands.

Once iCommands is installed and in the system `PATH` these instructions apply at a terminal in Mac OSX and Linux systems.

1. Open terminal 

2. Type `iinit` command to start the configuration
   process. When prompted, enter the values shown below as comments in the
   example code block.

.. code:: bash

     $ iinit
     One or more fields in your iRODS environment file (.irodsEnv) are
     missing; please enter them.
     Enter the host name (DNS) of the server to connect to: data.cyverse.org
     Enter the port number: 1247
     Enter your irods user name: #your_cyverse_username
     Enter your irods zone: iplant
     Those values will be added to your environment file (for use by
     other i-commands) if the login succeeds.

     Enter your current iRODS password: #your_cyverse_password

CyVerse Data Store configuration:

.. list-table::
    
 * - host name
   - port #
   - username
   - zone
   - password
 * - `data.cyverse.org`
   -  `1247`
   - CyVerse UserID
   - `iplant`
   - CyVerse Password

.. note::
    You can reconfigure iCommands for other iRODS data stores by changing your environment file
    
3. Verify that your iCommands installation works and is properly configured
   using the `ils` command to list the contents of your Data Store home
   directory.

   .. code:: bash

      $ ils
      /iplant/home/your_home_directory:
    file1
    file2
    file3
    C- /iplant/home/your_home_directory/analyses
    C- /iplant/home/your_home_directory/another_folder

----

**Anonymous access to the CyVerse Datastore**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can access public data in the CyVerse Datastore with icommands using:

- Username: anonymous

- Password: <leave blank>

*Upload Files/folders from local Computer to Data Store*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. warning::

   When uploading your data to the Data Store you should not upload files/folders
   with names containing spaces (e.g. experiment one.fastq) or name that contain
   special characters (e.g. ~ ` ! @ # $ % ^ & * ( ) + = { } [ ] | \ : ; " ' <
   > , ? /). The Apps on the Discovery Environment and most command line apps
   will typically not tolerate these characters. For long file/folder names the
   use of underscores (e.g. experiment_one.fastq) is the recommended practice.

.. tip::

    There are several optional arguments that the upload iCommand `iput` can
    take:

      .. code:: bash

        $ iput -r # For recursive transfer of directories and their contents

        $ iput -P # display the progress of the upload

        $ iput -f # force the upload and overwrite

        $ iput -T # Renew socket connection after 10 min (May help connections
                  # that are failing due to some connection/firewall settings)


    See the `full iCommands documentation <https://docs.irods.org/master/icommands/user/#iput>`__
    for more information.

1. Upload a directory using the `iput` command. Remember, the -r flag is to recursively upload a directory, so if you are uploading a single file, omit the -r flag.

   .. code:: bash

      $ iput -rPT /local_directory /iplant/home/cyverse_username/destination_folder
        # This command will output the progress as it uploads your local directory

----

**Download Files/folders from Data Store to local Computer**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. tip::

    There are several optional arguments that the upload iCommand `iget` can
    take:

      .. code:: bash

        $ iget -r # For recursive transfer of directories and their contents

        $ iget -P # display the progress of the upload

        $ iget -f # force the upload and overwrite

        $ iget -T # Renew socket connection after 10 min (May help connections
                  # that are failing due to some connection/firewall settings)


    See the `full iCommands documentation <https://docs.irods.org/master/icommands/user/#iget>`_
    for more information.

1. Download a file using the `iget` command. Remember, the -r flag is to recursively upload a directory, so if you are uploading a single file, omit the -r flag.

   .. code:: bash

      $ iget -PT /iplant/home/cyverse_username/target_file /local_destination
        # This command will output the progress as it downloads to your local machine

----

**NetCDF iCommands**
~~~~~~~~~~~~~~~~~~~~

For the Linux distributions there are three extra iCommands that support common NetCDF operations: 

``inc`` performs data operations on a list of NetCDF files, 

``incarch`` archives a open ended time series data, 

``incattr`` performs operation on attributes of NetCDF files. 

Each of these commands accepts the ``-h`` command line option. When a command is called with this option, it displays the command's help documentation.  Please see this help documentation for more information.

**Installation**

1. Install iRODS Runtime

Before the NetCDF iCommands can be installed, the current version of the iRODS run-time library needs to be installed. Please install the appropriate version (e.g. ``irods-runtime-X-X-XX``). The distribution specific packages can be found on  `RENCI's website <https://files.renci.org/pub/irods/releases/>`_.

2. Install NetCDF API

Once the run-time library is installed, the iRODS NetCDF API library needs to be installed. Please use the appropriate link to the download the installation package and install it. The package installer will likely warn that irods user and/or group don't exist, and that it will be using root instead. These warnings are harmless, since the package contents should be installed with root ownership.

* `CentOS7 <https://wiki.cyverse.org/wiki/download/attachments/28117338/irods-api-plugin-netcdf-1.0-centos7.rpm?version=1&modificationDate=1552065196000&api=v2>`_
* Ubuntu 14+ <https://wiki.cyverse.org/wiki/download/attachments/28117338/irods-icommands-netcdf-1.0-ubuntu14.deb?version=1&modificationDate=1549392566000&api=v2>`_

----

**Additional Frequently Used iCommands**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In addition to the commands above, there are several frequently used iCommands
- most of which you would expect following the Linux paradigm:

- **ipwd**: Print current directory
- **imkdir**: Create a directory
- **icd**: Change directory
- **irsync**: Sync local directory with iRODS directory

`iRODS iCommands Documentation <https://docs.irods.org/4.2.1/icommands/user/>`_

----

**Fix or improve this documentation:**

- On Github: `Repo link <https://github.com/CyVerse-learning-materials/data_store_guide>`_
- Send feedback: `Tutorials@CyVerse.org <Tutorials@CyVerse.org>`_

----

  |Home_Icon|_
  `Learning Center Home <http://learning.cyverse.org/>`_

.. |CyVerse logo| image:: ./img/cyverse_cmyk.png
    :width: 500
    :height: 100
.. _CyVerse logo: http://learning.cyverse.org/
.. |Home_Icon| image:: ./img/homeicon.png
    :width: 25
    :height: 25
.. _Home_Icon: http://learning.cyverse.org/
