.. _getting-started:

Getting Started
================

Running Newslynx Core
---------------------

Once you've `installed and initialized NewsLynx Core <http://newslynx.readthedocs.org/en/latest/install.html>`_, you can start a debug server with the following command:

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


Running NewsLynx App
---------------------

To start the server, in the ``newslynx-app`` folder, run the following:

.. code-block:: bash

   $ npm start

This compiles your CSS and JS and runs the server with `Forever <https://github.com/foreverjs/forever>`_.

When you see the following, it's done and you can visit http://localhost:3000.

**Note**: If you are running this in production, you want to run it in behind https and tell the app you are doing so one of two ways:

1. Run it with the environment variable ``NEWSLYNX_ENV=https``
2. Set ``https: true`` in your ``~/.newslynx/config.yaml`` file

This will make sure your cookies are set securely.

.. code-block:: bash

  #####################################
  # HTTP listening on 0.0.0.0:3000... #
  #####################################

Other App start up commands 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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
