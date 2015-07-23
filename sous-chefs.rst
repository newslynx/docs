.. _sous-chefs:

Sous Chefs
============================

NewsLynx is powered by Sous Chefs. Sous Chefs are small python scripts that can do almost anything to modify NewsLynx's data via the API. We implemented NewsLynx this way for the following reasons:

1. Different Organizations value different Metrics and different Events. Locking all Organizations into a single framework ultimately inhibits them.  By making NewsLynx fully modfular, we can account for many different potential use cases.
2. Social networks and analytic providers come in and out of fashion. The landscape of online media is constantly in flux. While we might assume that most every media organization has a Google Analytics account and a Facebook page, this may not be the case next year.  By not assuming the primacy of any particular data source, Sous Chefs allows NewsLynx to grow and adapt to shiting conditions.
3. Certain Organizations may have internal CMS's or Analytics tools which they have created internally.  Sous Chefs enables these organizations to import these sources into NewsLynx.


Writing Sous Chefs
++++++++++++++++++++

All Sous Chefs must inherit from the core ``newslynx.sc.SousChef`` class.  You can see the source code for this class `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/sc/__init__.py>`_.  Every Sous Chef can define the following methods:

* ``setup``: Configures the Sous Chef before executing it. This method can be used for connecting to APIs or other external source, as well as setting custom properties of the class' ``self``.
* ``run``: Executes the Sous Chef.  If this method is designed to bring data into NewsLynx, it should return a list or generator of dictionaries.
*  ``load``: Loads data output from ``run`` into NewsLynx.  If this SousChef yields more than one record, it should utilize the :ref:`Bulk API <events>`
* ``teardown``: Do something after the Sous Chef finishes loading data.

You can see examples of our internal Sous Chefs `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/sc/__init__.py>`_.

Configuring Sous Chefs
+++++++++++++++++++++++

All Sous Chefs are configured via a ``yaml`` or ``json`` file. These files follow a `JSON schema <http://jsonschema.org/>`_ which is defined `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/models/sous_chef.yaml>`_. Every Sous Chef consists of the following configurations:

* ``name``: The display name of the Sous Chef.
* ``slug``: A unique slug for the Sous Chef. This can double as it's ID.  Must only contain letters and ``-``. In ``regex``, this means: ``'^[a-z][a-z\-]+[a-z]$'``.
* ``description``: A description of what this SousChef does.
* ``runs``: The ``python`` import   path for the SousChef. For instance, if you built a ``python`` module named `my_sous_chef`, this might take the value of ``my_sous_chef.SousChef``.  The class at the end of the import path must inherit from the core ``newslynx.sc.SousChef`` class.  You can see the source code for this class `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/sc/__init__.py>`_. You can see an example of one of these modules `here <https://github.com/newslynx/newslynx.sc>`_.
* ``creates``: What type of collection does this Sous Chef create? Can be one of:
    - ``events``
    - ``content``
    - ``tags``
    - ``metrics``
    - ``report``
    - ``external``
    - ``internal``
* ``option_order``: OPTIONAL: The order in which ``options`` (explained below) are rendered. This parameter should take a list of ``option`` names. This parameter primarily exists to the aid in the dynamic rendering of Sous Chef options (explained below).
* ``options``: The Options this Sous Chef takes (explained below).
* ``metrics``: OPTIONAL: The metrics this Sous Chef creates (expalined below).

Sous Chef Options
++++++++++++++++++++

Sous Chefs can specify as many options as they require.  These options can be passed into the Sous Chef to change it's behavior.  At their core, :ref:`recipes` are simply a populated list of these options. Each options follows the schema sepecified below:


* ``input_type``: What type of input form should this option render?
    - This options enables ``newslynx-app`` to dynamically render forms to populate SousChef options.
    - This option can accept the following values:
        * ``search``: Render a search form to aid in populating the option's value. In practice, this is used to add items to an option which can be searched for via the API.
        * ``radio``: Render a radio form .
        * ``select``: Render a drop-down form.
        * ``checkbox``: Render a checkbox form.
        * ``checkbox-single``: Render a single-checkbox. This would be used, for example, in the case that you want an non-required option to take a ``default`` value of ``true`` while enabling a user to override this default with a value of ``false``. 
        * ``number``: Render a numeric form.
        * ``datepicker``: Render a form which enables a user to select a date.
        * ``text``: Render an open-ended text form.
        * ``paragraph``: Render an open-ended text form with more room to fill in details. Primarily for use with ``description`` fields.

* ``input_options``: If the ``input_type`` is ``radio``, ``select``, ``checkbox``, or ``checkbox-single``, a list of possible options to populate the form.
    - This parameter enables ``newslynx-app`` to dynamically render dropdowns, checkbox, or radio options.

* ``value_types``: What value types does this option accept?
    - This parameter enables ``newslynx-core`` to exhaustively validate options before executing Sous Chefs.
    - This option can accept the following values:
        * ``datetime``
        * ``crontab``
        * ``json``
        * ``regex``
        * ``boolean``
        * ``numeric``
        * ``string``
        * ``nulltype``
        * ``url``
        * ``email``
        * ``searchstring``

* ``accepts_list``: Does this option accept a list of values?  Defaults to ``false``.
* ``default``: What is the default value for this options? Defaults to ``null``.
* ``required``: Is this option required for the Sous Chef to properly run? Defaults to ``false``.
* ``help``: Parameters to help Users properly fill out options. ``help`` is a dictionary of the following values:
    
    - ``placeholder``: The placeholder/example text for this option.
    - ``link``: A link for more details about this option.
    - ``description``: A description of this option to display on form hover.

Sous Chef Metrics
++++++++++++++++++++

Sous Chefs which create metrics must also specify the schema of the metrics they create. This schema is specified in the :ref:`<metrics` docs.


Examples 
++++++++++++++

The best way to understand how Sous Chef's work is to look at the Source Code for the built-in modules `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/sc/__init__.py>`_.  You can see an example of a custom Sous Chef module `here <https://github.com/newslynx/newslynx.sc>`_.

