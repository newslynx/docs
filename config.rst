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

* ``super_user``:
	- The name of the Super User for this NewsLynx install. / org settings.
* ``super_user_email``:
	- The Super User's email (what you use to login with).
* ``super_user_password``:
	- The Super User's login password. This password will enable login to all other user profiles.
* ``super_user_apikey``:
	- The Super User's ``apikey``.  Principally here for development ease. Enables the regeneration of the datbase without resetting credentials.
* ``super_user_org``:
	- OPTIONAL: The Super User's Organization. The name of the organization to create on initialization. Will default to ``admin``.
* ``super_user_org_timezone``:
	- OPTIONAL: The timezone for the above organization. Will default to ``UTC``.

API credentials
+++++++++++++++++++

This file is also where you configure your credentials for Google Analytics, Twitter, Facebook, and other services:

* ``twitter_api_key``:
	- See `Twitter's developer docs for <http://dev.twitter.com>`_ for details on how to create an application on Twitter and configure these credentials.
* ``twitter_api_secret``:
	- See `Twitter's developer docs for <http://dev.twitter.com>`_ for details on how to create an application with Google Analytics and configure these credentials.

* ``google_analytics_client_id``:
	- See `Google's developer docs for <https://developers.google.com/analytics/>`_ for details on how to create an application with Google Analytics and configure these credentials.
* ``google_analytics_client_secret``:
	- See `Google's developer docs for <https://developers.google.com/analytics/>`_ for details on how to create an application  with Google Analytics and configure these credentials.

* ``facebook_app_id``:
	- See `Facebook's developer docs for <http://developers.facebook.com>`_ for details on how to create an application on facebook and configure these credentials.
* ``facebook_app_secret``:
	- See `Facebook's developer docs for <http://developers.facebook.com>`_ for details on how to create an application on facebook and configure these credentials.
* ``reddit_user_agent``:
	- A reddit-friendly User Agent.

* ``embedly_api_key``:
	- An API key for using `Embedly <http://embed.ly/>`_ for content extraction. If not provided, we fall back on internal methods.

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

Additional options
+++++++++++++++++++++++

In addition, there are numerous optional configurations you can tweak to modify the performance of NewsLynx. These are as follows:

Postgres
~~~~~~~~~~
* ``sqlalchemy_database_uri``
	- A valid `SQLAlchemy Database URI <http://docs.sqlalchemy.org/en/rel_1_0/core/engines.html#database-urls>`_.
	- **NOTE** This configuration is required when installing ``newslynx-core`` locally. 
	- default = ``postgresql://localhost:5432/newslynx``
* ``sqlalchemy_pool_size``
	- the maximum number of concurrent database connecitons
	- default = ``1000``
* ``sqlalchemy_pool_max_overflow``
	- the maximum number of concurrent database connections over sqlalchemy_pool_size before an error is thrown.
	- default = ``100``
* ``sqlalchemy_pool_timeout``
	- the number of seconds to wait on a database transaction before throwing an error.
	- default = ``60``
* ``sqlalchemy_echo``
	- whether or not to log all sql queries. Recommended only for debugging purposes.
	- default = ``false``

Redis 
~~~~~~
* ``redis_url``
	- the URL of the redis connection
	- default = ``redis://localhost:6379/0``

Caching
~~~~~~~~~~~
* ``url_cache_prefix``
	- The key prefix of the Redis cache for URL extraction (the process of reconciling raw URLs to their canonical form)
	- default = ``newslynx-url-cache``
* ``url_cache_ttl``
	- The number of seconds before an extracted URL expires.
	- default = ``1209600`` _14 days_
* ``url_cache_pool_size``
	- the number of URLs to extract conccurrently when ingesting Events 
	- default = ``5`` 

* ``extract_cache_prefix``
	- The key prefix of the Redis cache for Article extraction (the process of extracting metadata from URLs)
	- default = ``newslynx-extract-cache``
* ``extract_cache_ttl ``
	- The number of seconds before metadata extracted from a URL expires.
	- default = ``259200`` _3 days_

* ``thumbnail_cache_prefix``
	- The key prefix of the Redis cache for Article extraction (the process of extracting metadata from URLs)
	- default = ``newslynx-thumbnail-cache``
* ``thumbnail_cache_ttl``
	- The number of seconds before metadata extracted from a URL expires.
	- default = ``259200`` _3 days_
* ``thumbnail_size``
	- The size of thumbnails to generate. (These are stored on Events and Articles when an Image URL is present.)
	- default = ``[150, 150]``
* ``thumbnail_default_format``
	- The default format to render Thumbnails as. When we can identify the proper original format, we will render it as that format.
	- default = ``png`` 

* ``comparison_cache_prefix``
	- The key prefix of the Redis cache for Comparison metrics
	- default = ``newslynx-comparison-cache``
* ``comparison_cache_ttl``
	- The number of seconds before metadata extracted from a URL expires.
	- default = ``86400`` _1 day_
* ``comparison_percentiles``
	- The percentiles to return in the Comparison API.
	- default = ``[2.5, 5.0, 10.0, 20.0, 30.0, 40.0, 60.0, 70.0, 80.0, 90.0, 95.0, 97.5]``

Recipe Queue
~~~~~~~~~~~~
* ``merlynne_kwargs_prefix``
	- The key prefix for recipe configuraion we pass into Sous Chefs.
	- default = ``newslynx-merlynne-kwargs``
* ``merlynne_kwargs_ttl``
	- The number of seconds we'll keep these configuration in redis before they expire.
	- default = ``60``
* ``merlynne_results_ttl``
	- The number of seconds we'll keep the outputs of SousChefs in Redis before they expire.
	- default = ``60`` 

Recipe Scheduler
~~~~~~~~~~~~~~~~~
* ``scheduler_refresh_interval``
	- The frequency in seconds with which we'll check for updates to recipe schedules.
	- default = ``45``

* ``scheduler_reset_pause_range``
	- The range in seconds within which we'll reset Recipes when their schedule / configurations have changed.
	- default = ``[20, 200]``

Network
~~~~~~~~~~~~~~~~~~~~
* ``browser_user_agent``
	- The User Agent to use in the header of all outgoing network requests.
	- default = ``Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10; rv:33.0) Gecko/20100101 Firefox/33.0``
* ``browser_timeout``
	- The timout range for all network requests.
	- default = ``[7, 27]``
* ``browser_wait``
	- How long to wait in between network retiries.
	- default = ``0.8``
* ``browser_backoff``
	- The factor with which to multiply ``browser_wait`` on each subsequent retry.
	- default = ``2``
* ``browser_max_retries``
	- The maximum number of retries before failing.
	- default = ``2``


Intialization
++++++++++++++++++++++++

Once you have setup your configurations, you can initialize NewsLynx by running the following command:

.. code-block:: bash

	$ newslynx init 

This command will perform the following tasks:

1. Initialize the Postgres database specified with ``sqlalchemy_database_uri``.
2. Initialize the Super User and Organizaiton.
3. Install all default SousChefs in the current environment.
4. Add all Sous Chefs for this organization.
5. Add all default tags for this organization.
6. Install all default recipes for this organization.
7. Install all metrics associated with all default Sous Chefs for this organization.

If you want to initialize NewsLynx with only the Super User and Organization, use the following command

.. code-block:: bash

	$ newslynx init --bare


Starting the API.
++++++++++++++++++++++++

Once you've configured NewsLynx, you can start a debug server with the following command:

.. code-block:: bash
	
	$ newslynx debug 

If you'd like to start a multi-theaded production server (some Sous Chefs may not work without this), run this command inside the root directory of ``newslynx-core``:


.. code-block:: bash
	
	$ bin/run 

To start the task queue, run this command inside the root directory of ``newslynx-core``:

.. code-block:: bash
	
	$ bin/start_workers

To stop the task queue, run this command inside the root directory of ``newslynx-core``:

.. code-block:: bash
	
	$ bin/stop_workers

To start the Recipe scheduler, run this command:

.. code-block:: bash
	
	$ newslynx cron 

For next step, refer to our :ref:`getting-started` docs.


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
