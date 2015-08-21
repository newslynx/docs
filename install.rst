.. _installation:

Installation Guide
==================

The following installation guide is oriented towards people interested in running NewsLynx locally on a Mac OS X computer.
For all other applications, please refer to our `automation docs <https://github.com/newslynx/automation>`_. At this time, NewsLynx is incapable of running on Windows.

NewsLynx Core
------------

Download the git repository and install dependencies:

To install it manually simply download the repository from Github:

.. code-block:: bash

   $ git clone git://github.com/newslynx/newslynx.git && cd newslynx
   $ python setup.py install

Additional API Dependencies
+++++++++++++++++++++++++++

``newslynx`` also depends on `Postgres <http://www.postgresql.org/>`_ and `Redis <http://www.redis.io>`_. If you're on Mac OS X, the easiest way to run Postgres is with the `Postgres.app <http://www.http://postgresapp.com/.org/>`_. However, if you prefer the `Homebrew <http://www.brew.sh/>`_ distribution, make sure to install it with plpythonu.

.. code-block:: bash

   $ brew install postgresql --build-from-source --with-python

If you already have the Hombrew version installed, run this command:

.. code-block:: bash

   $ brew renstall postgresql --build-from-source --with-python


Finally, install ``redis`` via Homebrew

.. code-block:: bash

   $ brew install redis

For the next steps, refer to the  :ref:`config` docs.


NewsLynx App
------------

Download the git repository and install dependencies:

.. code-block:: bash

   $ git clone https://github.com/newslynx/newslynx-app && cd newslynx-app
   $ npm install


NewsLynx Desktop
------------

We also have a beta desktop version of NewsLynx App, which brings all of the same functionality of the web interface to a native environment, which can be easier for people to use. You can download the latest release for Mac OS X on the `Releases <https://github.com/newslynx/newslynx-electron/releases>`_ page of the `project repo <https://github.com/newslynx/newslynx-electron>`_.

Follow that project's `issue tracker <https://github.com/newslynx/newslynx-electron/issues>`_ for progess on the first 1.0 release. If you would like to try it in the mean time please do. If you would like a Windows or Linux version, let us know by filing an issue and we can bake one out.

