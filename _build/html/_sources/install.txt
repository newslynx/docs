.. _installation:

Installation Guide
==================

The following installation guide is oriented towards people interested in running NewsLynx locally on a Mac OS X computer.
For all other applications, please refer to our `automation docs <http://www.github.org/newslynx/authomation>`_. At this time, NewsLynx is incapable of running on Windows.

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

**NOTE**: Newslynx is incapable of running on Windows.

NewsLynx App
------------

Download the git repository and install dependencies:

.. code-block:: bash

   $ git clone https://github.com/newslynx/newslynx-app && cd newslynx-app
   $ npm install

