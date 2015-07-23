.. _config:

Configuration
=============

NewsLynx Core
------------

``newslynx`` requires specific configurations to run.  By default, these should all be stored in a file in your home directory named  ``~/.newslynx``.  In this directory there should at least be one file named ``config.yaml``. 




Running NewsLynx App
------------

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
