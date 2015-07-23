.. _sous-chefs:

Sous Chefs
============================

NewsLynx is powered by Sous Chefs. Sous Chefs are small python scripts that can do almost anything to modify NewsLynx's data via the API. We implemented NewsLynx this way for the following reasons:

1. Different Organizations value different Metrics and different Events. Locking all Organizations into a single framework ultimately inhibits them.  By making NewsLynx fully modfular, we can account for many different potential use cases.
2. Social networks and analytic providers come in and out of fashion. The landscape of online media is constantly in flux. While we might assume that most every media organization has a Google Analytics account and a Facebook page, this may not be the case next year.  By not assuming the primacy of any particular data source, Sous Chefs allows NewsLynx to grow and adapt to shiting conditions.
3. Certain Organizations may have internal CMS's or Analytics tools which they have created internally.  Sous Chefs enables these organizations to import these sources into NewsLynx.


Writing Sous Chefs
====================

All Sous Chefs must inherit from the core ``newslynx.sc.SousChef`` class.  You can see the source code for this class `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/sc/__init__.py>`_.  Every Sous Chef can define the following methods:

* ``setup``: Configures the Sous Chef before executing it. This method can be used for connecting to APIs or other external source, as well as setting custom properties of the class' ``self``.
* ``run``: Executes the Sous Chef.  If this method is designed to bring data into NewsLynx, it should return a list or generator of dictionaries.
*  ``load``: Loads data output from ``run`` into NewsLynx.  If this SousChef yields more than one record, it should utilize the :ref:`Bulk API <events>`
* ``teardown``: Do something after the Sous Chef finishes loading data.

You can see examples of our internal Sous Chefs `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/sc/__init__.py>`_.

Configuring Sous Chefs
========================

All Sous Chefs are configured via a ``yaml`` or ``json`` file. These files follow a `JSON schema <http://jsonschema.org/>`_ which is defined `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/models/sous_chef.yaml>`_. Every Sous Chef consists of the following configurations:

* ``name``: The display name of the Sous Chef.
* ``slug``: A unique slug for the Sous Chef. This can double as it's ID.  Must only contain letters and ``-``. In ``regex``, this means: ``'^[a-z][a-z\-]+[a-z]$'``.
* ``description``: A description of what this SousChef does.
* ``runs``: The ``python`` import path for the SousChef.










.. _sous-chefs-creates:

Creates
~~~~~~~~~


.. _sous-chefs-runners:

Runners
~~~~~~~~