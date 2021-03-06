.. _installation:

Development Installation Guide
===============================

The following installation guide is oriented towards people interested in running NewsLynx locally on a Mac OS X computer.
For deploying the software, please refer to our `automation docs <https://github.com/newslynx/automation>`_.

NewsLynx Core
---------------

**NOTE** - For most applications, we recommend following our `automation guide <https://github.com/newslynx/automation>`_.  If you'd like to setup a development environment, following the instructions below for MacOS X. At this time, NewsLynx Core is not supported on Windows.

Install the dependencies
~~~~~~~~~~~~~~~~~~~~~~~~

``newslynx`` depends on `Postgres <http://www.postgresql.org/>`_ and `Redis <http://www.redis.io>`_. If you're on Mac OS X, the easiest way to run Postgres is with the `Postgres.app <http://www.http://postgresapp.com/.org/>`_. However, if you prefer the `Homebrew <http://www.brew.sh/>`_ distribution, make sure to install it with plpython.

.. code-block:: bash

   $ brew install postgresql --build-from-source --with-python

If you already have the Hombrew version installed, run this command:

.. code-block:: bash

   $ brew reinstall postgresql --build-from-source --with-python

Finally, install ``redis`` via Homebrew

.. code-block:: bash

   $ brew install redis

If ``redis`` does not automatically start, open another tab and run

.. code-block:: bash

   $ redis-server

Installation
~~~~~~~~~~~~~~~~~~~~~~~~

**OPTIONAL** - First set your `configurations <http://newslynx.readthedocs.org/en/latest/config.html>`_. If you don't do this, we will fallback on the app's `default configuration file <https://github.com/newslynx/newslynx-core/blob/master/newslynx/app/config.yaml>`_.

Install ``newslynx``, initialize the database, super user, and install default sous chefs, tags, and recipes.

.. code-block:: bash

	$ git clone https://github.com/newslynx/newslynx-core.git
	$ cd newslynx-core
	$ make app_install

**EXPERT MODE**  - Don't install the app's default sous chefs, tags, or recipes.

.. code-block:: bash

	$ git clone https://github.com/newslynx/newslynx-core.git
	$ cd newslynx-core
	$ make bare_install 

For the next steps, refer to the `getting started docs <http://newslynx.readthedocs.org/en/latest/getting-started.html>`_.

NewsLynx App
------------

**NOTE** - For most applications, we recommend following our `automation guide <https://github.com/newslynx/automation>`_.  If you'd like to setup a development environment, following the instructions below for MacOS X.

Download the git repository and install dependencies:

.. code-block:: bash

   $ git clone https://github.com/newslynx/newslynx-app && cd newslynx-app
   $ npm install


NewsLynx Desktop
-----------------

We also have a beta desktop version of NewsLynx App, which brings all of the same functionality of the web interface to a native environment, which can be easier for people to use. You can download the latest release for Mac OS X on the `Releases <https://github.com/newslynx/newslynx-electron/releases>`_ page of the `project repo <https://github.com/newslynx/newslynx-electron>`_.

Follow that project's `issue tracker <https://github.com/newslynx/newslynx-electron/issues>`_ for progress on the first 1.0 release. If you would like to try it in the mean time please do. If you would like a Windows or Linux version, let us know by filing an issue and we can bake one out.

