.. _config:

Configuration
=============

NewsLynx requires specific configurations to run. By default, these should all be stored in a file in your home directory named  ``~/.newslynx``.  In this directory there should at least be one file named ``config.yaml``. This file can also be JSON. If you would like to configure another location for this file, set the environment variable ``NEWSLYNX_CONFIG_FILE``.  Finally, we should note that all configurations described below can also be stored as environment variables, with the naming convention: ``NEWSLYNX_{SETTING_NAME}``, however you will still need to have a ``config.yaml`` file.

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
* ``api_dns``:
	- The URL of the final app. This is used when you authenticate with services to redirect you back to where you came from. This field isn't required if you aren't using the app at all.

API credentials
+++++++++++++++++++

This file is also where you configure your credentials for Google Analytics, Twitter, Facebook, and other services. These aren't credentials for the news organization(s), rather, your instance of NewsLynx needs an "app" with each of these services that each news organization gives its tokens **to**. For example, it's the app that you will see in your Google account under apps that have access to the news organization's account tokens. You can give that application the `NewsLynx logo <https://raw.githubusercontent.com/newslynx/newslynx-app/master/lib/public/images/gifs/merlynne-ears.png>`_ if you like, which will be presented to users when they authenticate with these different services from within the NewsLynx interface.

* ``twitter_api_key``:
	- See `Twitter's developer docs for <http://apps.twitter.com>`_ for details on how to create an application on Twitter and configure these credentials. The option you want is to create an app. Set the callback url to ``http://<app-url>/settings``. If you're running it on a port other than 80, put that in the url.
* ``twitter_api_secret``:
	- See `Twitter's developer docs for <http://apps.twitter.com>`_ for details on how to create an application with Twitter and configure these credentials.

* ``google_analytics_client_id``:
	- See `Google's developer docs for <https://console.developers.google.com/>`_ for details on how to create an application with Google Analytics and configure these credentials. You'll want to create an application and enable the Analytics API. Then click on "Credentials" on the left and create an oAuth 2.0 for a "web application." Set the callback URI to ``http://<api-url>:5000/api/v1/auths/google-analytics/callback``
* ``google_analytics_client_secret``:
	- See `Google's developer docs for <https://console.developers.google.com/>`_ for details on how to create an application  with Google Analytics and configure these credentials. You'll want to create an application and enable the Analytics API. Then click on "Credentials" on the left and create an oAuth 2.0 for a "web application." Set the callback URI to ``http://<api-url>:5000/api/v1/auths/google-analytics/callback``.

* ``facebook_app_id``:
	- See `Facebook's developer docs for <http://developers.facebook.com>`_ for details on how to create an application on facebook and configure these credentials.
* ``facebook_app_secret``:
	- See `Facebook's developer docs for <http://developers.facebook.com>`_ for details on how to create an application on facebook and configure these credentials.
* ``reddit_user_agent``:
	- A reddit-friendly User Agent.

* ``embedly_api_key``:
	- An API key for using `Embedly <http://embed.ly/>`_ for content extraction. If not provided, we fall back on internal methods.


Default Tags and Recipes
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

If you'd like to save these files elsewhere, you can modify the following configurations

* ``default_tags``:
	- A path to a ``yaml`` file with a list of default Tags.
	- default = ``~/.newslynx/defaults/tags.yaml``
* ``default_recipes``:
	- A path to a ``yaml`` file with a list of default Recipes.
	- default = ``~/.newslynx/defaults/recipes.yaml``


By default, NewsLynx Core installs the default Tags and Recipes needed to run the Application. If you'd like to install NewsLynx core without these defaults, make sure to use the ``--bare`` flat when you run ``newslynx init`` (more details on this below).

Additional Options
+++++++++++++++++++++++

In addition, there are numerous optional configurations you can tweak to modify the performance of NewsLynx. You can also read through them in  the `source code <https://github.com/newslynx/newslynx-core/blob/master/newslynx/defaults.py>`_.

Super user
~~~~~~~~~~
* ``super_user_org``:
	- The Super User's Organization. The name of the organization to create on initialization. Will default to ``admin``.
* ``super_user_org_timezone``:
	- The timezone for the above organization. Will default to ``UTC``.

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
	- default = ``1209600`` (14 days)
* ``url_cache_pool_size``
	- the number of URLs to extract conccurrently when ingesting Events 
	- default = ``5`` 

* ``extract_cache_prefix``
	- The key prefix of the Redis cache for Article extraction (the process of extracting metadata from URLs)
	- default = ``newslynx-extract-cache``
* ``extract_cache_ttl ``
	- The number of seconds before metadata extracted from a URL expires.
	- default = ``259200`` (3 days)

* ``thumbnail_cache_prefix``
	- The key prefix of the Redis cache for Article extraction (the process of extracting metadata from URLs)
	- default = ``newslynx-thumbnail-cache``
* ``thumbnail_cache_ttl``
	- The number of seconds before metadata extracted from a URL expires.
	- default = ``259200`` (3 days)
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
	- default = ``86400`` (1 day)
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
* ``network_user_agent``
	- The User Agent to use in the header of all outgoing network requests.
	- default = ``Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10; rv:33.0) Gecko/20100101 Firefox/33.0``
* ``network_timeout``
	- The timout range for all network requests.
	- default = ``[7, 27]``
* ``network_wait``
	- How long to wait in between network retiries.
	- default = ``0.8``
* ``network_backoff``
	- The factor with which to multiply ``network_wait`` on each subsequent retry.
	- default = ``2``
* ``network_max_retries``
	- The maximum number of retries before failing.
	- default = ``2``

Notifications
~~~~~~~~~~~~~~~~~~~~
* ``notify_methods``
	- A list of notification methods to utilize. Currently ``email`` and ``slack``.
	- default = ``[]``
* ``notify_email_recipients``
	- A list of emaill addresses to send notifications to.
	- default = ``[]``
* ``notify_email_subject_prefix``
	- The prefix to insert into the subject of all email notifications.
	- default = ``[ Merlynne ]``
* ``notify_slack_webhook``
	- A slack webhook url for posting notifications to.
	- see: https://api.slack.com/incoming-webhooks
* ``notify_slack_channel``
	- The slack channel to post to.
	- default = ``#general``
* ``notify_slack_username``
	- The slack username to post as.
	- default = ``Merlynne``
* ``notify_slack_emoji``
	- The slack emoji to post with.
	- default = ``:-1:``

Email 
~~~~~~~~~~~~~~~~~~~~
These configurations are only currenly required for installs where ``notify_methods`` includes ``email``.
In the future, there will be more email integrations and these configurations will stay the same.

* ``mail_username``
	- The username of the account to use for sending and recieving emails.
* ``mail_password``
	- The password of the account to use for sending and recieving emails.
* ``mail_server``
	- The domain of the account's server (e.g. mail.google.come)
* ``mail_smtp_port``
	- The server's smtp port for sending messages
* ``mail_imap_port``
	- The server's imap port for receiving messages

Intialization
++++++++++++++++++++++++

Once you have setup your configurations, follow the `installation docs <http://newslynx.readthedocs.org/en/latest/install.html>`_.
