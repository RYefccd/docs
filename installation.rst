.. _installation:

PipelineDB Installation
===========================

Install PostgreSQL
---------------------------

Since PipelineDB runs as an extension to PostreSQL, begin by `installing PostgreSQL`_.

.. _`installing PostgreSQL`: https://www.postgresql.org/download/

Once you have PostgreSQL installed on your system, you just need to install the PipelineDB binaries and then create the PipelineDB extension within your PostgreSQL database.

apt
------------


yum
---------------

-------------------------

You may also download binary packages from our `release <https://github.com/pipelinedb/pipelinedb/releases>`_ archives and install them directly.

RPM Packages
--------------------

To install the PipelineDB RPM package, run:

.. code-block:: sh

	sudo rpm -ivh pipelinedb-postgresql-<pg version>_<pipelindb version>.rpm

Debian Packages
---------------------

To install the PipelineDB Debian package, run:

.. code-block:: sh

	sudo dpkg -i pipelinedb-postgresql-<pg version>_<pipelindb version>.deb

OS X
----

Just double-click the :code:`pipelinedb-<version>.pkg` file to launch the OS X Installer. For older versions of OS X, you might need to install a few packages that PipelineDB depends on:

.. code-block:: sh

  brew install json-c freexl

Initializing PipelineDB
------------------------

Once PipelineDB is installed, you can initialize a database directory. This is where PipelineDB will store all the files and data associated with a database. To initialize a data directory, run:

.. code-block:: sh

	pipeline-init -D <data directory>

where :code:`<data directory>` is a nonexistent directory. Once this directory has been successfully initialized, you can run a PipelineDB server.

Running PipelineDB
---------------------

To run the PipelineDB server in the background, use the :code:`pipeline-ctl` driver and point it to your newly initialized data directory:

.. code-block:: sh

	pipeline-ctl -D <data directory> -l pipelinedb.log start

The :code:`-l` option specifies the path of a logfile to log to. The :code:`pipeline-ctl` driver can also be used to stop running servers:

.. code-block:: sh

	pipeline-ctl -D <data directory> stop

Run :code:`pipeline-ctl --help` to see other available functionality. Finally, the PipelineDB server can also be run in the foreground directly:

.. code-block:: sh

	pipelinedb -D <data directory>

To connect to a running server using the default database "pipeline", the :code:`pipeline` command can be used:

.. code-block:: sh

	pipeline pipeline

`PostgreSQL's`_ standard client, :code:`psql`, can also be used to connect to PipelineDB. Note that PipelineDB's default port is :code:`5432`:

.. _`PostgreSQL's`:  http://www.postgresql.org/download/

.. code-block:: sh

	psql -p 5432 -h localhost pipeline

You can check out the :ref:`quickstart` section to start streaming data into PipelineDB right now.

Configuration
---------------------

PipelineDB's configuration is generally synonymous with `PostgreSQL's configuration`_, so that is a good place to look for details about what everything in :code:`pipelinedb.conf` does.

.. _`PostgreSQL's configuration`: http://www.postgresql.org/docs/current/static/runtime-config.html

By default, PipelineDB is not configured to allow incoming connections from remote hosts. To enable incoming connections, first set the following line in :code:`pipelinedb.conf`:

.. code-block:: sh

    listen_addresses = '*'

And in :code:`pg_hba.conf`, add a line such as the following to allow incoming connections:

.. code-block:: sh

    host    all             all             <ip address>/<subnet>            md5


For example, to allow incoming connections from any host:

.. code-block:: sh

    host    all             all             0.0.0.0/0            md5

-------------

Now you're ready to put PipelineDB to work! Check out the :ref:`continuous-views` or :ref:`quickstart` sections to get started.

Docker
---------------------

A PipelineDB Docker image is also available (thanks to Josh Berkus). It can be run with:

.. code-block:: sh

  docker run -v /dev/shm:/dev/shm pipelinedb/pipelinedb

This image exposes port :code:`5432` for interaction with PipelineDB; credentials are user :code:`pipeline`, password :code:`pipeline`.

The database gets installed to :code:`/mnt/pipelinedb`, so if you want to put that on real storage, or modify the configuration files, then simply mount that as a volume before starting the image for the first time.

.. note:: The configuration which installs with the image is appropriate for testing on your laptop. If you deploy this to production, you will want to edit pipelinedb.conf and substantially increase resource limits for most things.
