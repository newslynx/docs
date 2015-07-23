.. _config:

Configuration
=============

NewsLynx Core
--------------

NewsLynx requires specific configurations to run.  By default, these should all be stored in a file in your home directory named  ``~/.newslynx``.  In this directory there should at least be one file named ``config.yaml``. This file can also be JSON. If you would like to configure another location for this file, set the environment variable ``NEWSLYNX_CONFIG_FILE``.  Finally, we should note that all configurations described below can also be stored as environment variables, with the naming convention: ``NEWSLYNX_{SETTING_NAME}``, however you will still need to have a ``config.yaml`` file.

Required Settings
+++++++++++++++++++

``config.yaml`` contains all the information we need to run NewsLynx.  As you'll see below, the App also utilizes this file for it's configurations.

In this file, we require, at, minimum, the following fields:

* ``sqlalchemy_database_uri``:  This tells us where to create NewsLynx's Postgres database. You can read more about how these work in the `SQL Alchemy Docs <http://docs.sqlalchemy.org/en/rel_1_0/core/engines.html>`_.
* ``super_user``: The name of the Super User for this NewsLynx install. / org settings.
* ``super_user_email``: The Super User's email (what you use to login with).
* ``super_user_password``: The Super User's login password. This password will enable login to all other user profiles.
* ``super_user_apikey``: The Super User's ``apikey``.  Principally here for development ease. Enables the regeneration of the datbase without resetting credentials.
* ``super_user_org``: OPTIONAL: The Super User's Organization. The name of the organization to create on initialization. Will default to ``admin``.
* ``super_user_org_timezone``: OPTIONAL: The timezone for the above organization. Will default to ``UTC``.

API credentials
+++++++++++++++++++

This file is also where you configure your credentials for Google Analytics, Twitter, Facebook, and other services:

* ``twitter_api_key``: See `Twitter's developer docs for <http://dev.twitter.com>`_ for details on how to create an application on Twitter and configure these credentials.
* ``twitter_api_secret``: See `Twitter's developer docs for <http://dev.twitter.com>`_ for details on how to create an application with Google Analytics and configure these credentials.

* ``google_analytics_client_id``: See `Google's developer docs for <https://developers.google.com/analytics/>`_ for details on how to create an application with Google Analytics and configure these credentials.
* ``google_analytics_client_secret``: See `Google's developer docs for <https://developers.google.com/analytics/>`_ for details on how to create an application  with Google Analytics and configure these credentials.

* ``facebook_app_id``: See `Facebook's developer docs for <http://developers.facebook.com>`_ for details on how to create an application on facebook and configure these credentials.
* ``facebook_app_secret``: See `Facebook's developer docs for <http://developers.facebook.com>`_ for details on how to create an application on facebook and configure these credentials.
* ``reddit_user_agent``: A reddit-friendly User Agent.

* ``embedly_api_key``: An API key for using `Embedly <http://embed.ly/>`_ for content extraction. If not provided, we fall back on internal methods.

There are many, many more configurations you can set. You can see the defaults for all of these in the `source code <https://github.com/newslynx/newslynx-core/blob/master/newslynx/defaults.py>`_.

Organizational defaults
++++++++++++++++++++++++

In addition to these settings, you can also configure a list of default Tags and Recipes to create for every new Organization. This is useful for creating applications on top of the API which onboard new Organizations and Users without too much hassle.

These files live, by default, in ``~/.newslynx/defaults``.  Each file should have the name of the items it contains and consist of a list of objects:

For example, ``~/.newslynx/defaults/tags.yaml`` specifies a list of tags to apply to every new organization.

.. code-block:: yaml 

	- name: Politics
	  slug: politics
	  color: '#a6cee3'
	  type: subject

	- name: Reprint / pickup
	  slug: reprint-pickup
	  category: citation
	  level: media
	  color: '#6699cc'
	  type: impact 

If you would like to use the defaults for the Application, make sure to 
use the ``--app-defaults`` flag when you run ``newslynx init`` (more details on this below).


Intialization
++++++++++++++++++++++++

Once you have setup you configurations, you can initialize NewsLynx by running the following command:

.. code-block:: bash

	$ newslynx init 

This command will perform the following tasks:

1. Initialize the Postgres database specified with ``sqlalchemy_database_uri``.
2. Initialize the Super User and Organizaiton.
3. Add all built-in SousChefs to this Organization.
4. Install all default Tags (described above)
5. Install all default Sous Chefs (described above)

If you want to initialize NewsLynx with the default Tags and Recipes the App expects, add the ``--app-defaults`` flag.

.. code-block:: bash

	$ newslynx init --app-defaults


Running NewsLynx App
---------------------

To start the server, in the ``newslynx-app`` folder, run the following:

.. code-block:: bash

   $ npm start

This compiles your CSS and JS and runs the server with `Forever <https://github.com/foreverjs/forever>`_.

When you see the following, it's done and you can visit http://localhost:3000.

**Note**: If you are running this in production, you want to run it in behind https and tell the app you are doing so one of two ways:

1. Run it with the environment variable ``NEWSLYNX_ENV=https``
2. Set ``newslynx_app_https: true`` in your ``~/.newslynx/config.yaml`` file

This will make sure your cookies are set securely.

.. code-block:: bash

  #####################################
  # HTTP listening on 0.0.0.0:3000... #
  #####################################

Other App start up commands 
---------------------------

Alternate commands are in `package.json <https://github.com/newslynx/newslynx-app/blob/master/package.json>`_ under `"scripts" <https://github.com/newslynx/newslynx-app/blob/master/package.json#L5>`_. These are for **developing locally.**

If you want to modify files and have the CSS and JS re-compiled automatically and the server restarted if necessary, do:

.. code-block:: bash

   $ npm run dev

If you just want to watch the CSS and JS and re-compile when on change, do:

.. code-block:: bash

   $ npm run watch-files

If you just want to watch the Express server and restart when its files change (templates, server js files), do:

.. code-block:: bash

   $ npm run watch-server

These last two commands are best run in tandem in two separate shell windows. `npm run dev` does them both in one window for convenience.

The final command listed is ``npm test``, which will run a simple test to make sure the server can launch.
