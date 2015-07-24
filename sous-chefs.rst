.. _sous-chefs:

Sous Chefs
============================

NewsLynx is powered by Sous Chefs. Sous Chefs are small ``python`` scripts that can do almost anything to modify NewsLynx's data via the API. We implemented NewsLynx this way for the following reasons:

1. Different Organizations value different Metrics and different Events. Locking all Organizations into a single framework ultimately inhibits them.  By making NewsLynx fully modular, we can account for many different potential use cases.
2. Social networks and analytic providers come in and out of fashion. The landscape of online media is constantly in flux. While we might assume that most every media organization has a Google Analytics account and a Facebook page, this may not be the case next year.  By not assuming the primacy of any particular data source, Sous Chefs allows NewsLynx to grow and adapt to shfiting conditions.
3. Certain Organizations may have internal CMS's or Analytics tools which they have created internally.  Sous Chefs enables these organizations to import these sources into NewsLynx.


Writing Sous Chefs
++++++++++++++++++++

All Sous Chefs must inherit from the core ``newslynx.sc.SousChef`` class.  You can see the source code for this class `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/sc/__init__.py>`_.  Every Sous Chef can define the following methods:

* ``setup``
    - Configures the Sous Chef before executing it. This method can be used for authenticating wtih APIs or setting custom properties of the Sous Chef.
* ``run``
    - Executes the Sous Chef.  If this method is designed to bring data into NewsLynx, it should return a list or generator of dictionaries.
*  ``load``
    - Loads data output from ``run`` into NewsLynx.  If this SousChef yields more than one record, it should utilize the :ref:`Bulk API <events>`
* ``teardown``
    - Do something after the Sous Chef finishes loading data.

You can see examples of our internal Sous Chefs `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/sc/__init__.py>`_.

Configuration
+++++++++++++++++++++++

All Sous Chefs are configured via a ``yaml`` or ``json`` file. These files follow a `JSON schema <http://jsonschema.org/>`_ which is defined `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/models/sous_chef.yaml>`_. Every Sous Chef consists of the following configurations:

* ``name``
    - The display name of the Sous Chef. 
    - **Required**
* ``slug``
    - A unique slug for the Sous Chef. This can double as it's ID in API calls.  Must only contain letters and dashes. In ``regex``, this means: ``'^[a-z][a-z\-]+[a-z]$'``.
    - **Required**
* ``description``
    - A description of what this Sous Chef does.
    - **Required**
* ``runs``
    - The ``python`` import path for the Sous Chef. For instance, if you built a ``python`` module named ``my_sous_chef``, this might take the value of ``my_sous_chef.SousChef``.  The class at the end of the import path must inherit from the core ``newslynx.sc.SousChef`` class.  You can see the source code for this class `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/sc/__init__.py>`_. You can see an example of one of these modules `here <https://github.com/newslynx/newslynx.sc>`_ or below. 
    - **Required**
* ``creates`` 
    - What type of collection does this Sous Chef create? 
    - **Required**
    - Can be one of:
        - ``events``
        - ``content``
        - ``tags``
        - ``metrics``
        - ``report``
        - ``external``
        - ``internal``
* ``option_order``
    - The order in which ``options`` are rendered. This parameter should take a list of option names. This parameter primarily exists to aid in the dynamic rendering of Sous Chef input forms.
    - *Optional*
* ``includes``
    - A list of relative paths to other Sous Chef config files to inherit for this Sous Chef. This prevents rewriting option definitions for Sous Chefs with similar behaviors. If this file contains a partial Sous Chef config, it's filename should begin with an underscore. You can see an example of such a file `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/sc/events/_event_options.yaml>`_ and it's include statement `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/sc/events/facebook_page_to_event.yaml>`_.
    - *Optional*
* ``requires_auths``
    - A list of :ref:`<endpoints-auths` ``names`` that must exist for an Organization before this Sous Chef can run.
    - *Optional*
* ``requires_settings``
    - A list of :ref:`<endpoints-settings` ``names`` that must exist for an Organization before this Sous Chef can run. 
    - *Optional*
* ``options``
    - The options this Sous Chef takes. 
    - *Optional* (if this Sous Chef's behavior should not be modified).
* ``metrics``
    - The Metrics this Sous Chef creates.
    - *Optional* (unless the Sous Chef creates :ref:`metrics`).

Options
++++++++++++++++++++

Sous Chefs can specify as many options as they require.  These options can be passed into the Sous Chef to change it's behavior.  At their core, :ref:`recipes` are simply a populated list of these options. Each option follows the schema sepecified below:

* ``input_type`` 
    - What type of input form should this option render?
    - This options enables ``newslynx-app`` to dynamically render forms to populate Sous Chef options.
    - This option can accept the following values
        * ``search``
            - Render a search form to aid in populating the option's value. In practice, this is used to add items to an option which can be searched for via the API.
        * ``radio``
            - Render a radio form.
        * ``select``
            - Render a drop-down form.
        * ``checkbox``
            - Render a checkbox form.
        * ``checkbox-single``
            - Render a single-checkbox. This would be used, for example, in the case that you want an non-required option to take a ``default`` value of ``true`` while enabling a user to override this default with a value of ``false``. 
        * ``number``
            - Render a numeric form.
        * ``datepicker``
            - Render a form which enables a user to select a date.
        * ``text``
            - Render an open-ended text form.
        * ``paragraph``
            - Render an open-ended text form with more room to fill in details. Primarily for use with ``description`` fields.
* ``input_options``
    - If the ``input_type`` is ``radio``, ``select``, ``checkbox``, or ``checkbox-single``, a list of possible options to populate the form.
    - This parameter enables ``newslynx-app`` to dynamically render dropdowns, checkbox, or radio options.
* ``value_types``
    - What value types does this option accept?
    - This parameter enables ``newslynx-core`` to exhaustively validate options before executing Sous Chefs.
    - This option can accept the following values:
        * ``datetime``
            - An ISO-8601 date.
        * ``crontab``
            - A cron string.
        * ``json``
            - A complex option, usually a dictionary, which only needs to be json-serializable.
        * ``regex``
            - Valid input to ``re.compile()``.
        * ``boolean``
            - Truish (true, t, yes, y, 1, on) or Falsish Values (false, f, no, n, off).  
        * ``numeric``
            - Valid input to ``float()`` or ``int()``.
        * ``string``
            - Valid input ot ``str()``
        * ``nulltype``
            - Any of (None, null, N/A, NaN)
        * ``url``
            - A valid URL  (determined by regular expresion.).
        * ``email``
            - A valid email address (determined by regular expresion.)
        * ``searchstring``
            - A custom type which we use to provide basic search capabilities to Sous Chefs (explained below.)
* ``accepts_list``
    - Does this option accept a list of values?
    - Defaults to ``false``.
* ``default``
    - What is the default value for this options?
    - Defaults to ``null``.
* ``required``
    - Is this option required for the Sous Chef to properly run? 
    - Defaults to ``false``.
* ``help``
    - Parameters to help users properly fill out options. 
    - ``help`` is a dictionary of the following values:
    - ``placeholder``
        - The placeholder/example text for this option.
    - ``link``
        - A link for more details about this option.
    - ``description``
        - A description of this option to display on form hover.

Default Options
++++++++++++++++++++

All Sous Chefs come with the following default options. These exist for aiding in the creation and scheduling of :ref:`recipes`.
For more information on how these options affect when a Sous Chef is executed, refer to the :ref:`recipes-scheduling` docs.

.. code-block:: yaml

    name:
        input_type: text
        value_types:
            - string
        required: true
        help:
            description: The name of the Recipe.

    slug:
        input_type: text
        value_types:
            - string
        help:
            placeholder: (optional)
            description: The recipe slug. Lowercase and separated with '-'.

    description: 
        input_type: paragraph
        value_types:
            - string
            - nulltype
        help: 
            placeholder: (optional)
            description: A description of what this recipe does.

    schedule_by:
        input_type: select 
        input_options:
            - minutes
            - time_of_day
            - crontab
            - unscheduled
        value_types: 
            - string
            - nulltype
        default: null 
        help:
            placeholder: minutes
            description: The method for scheduling the recipe.

    crontab:
        input_type: text
        value_types:
            - crontab
            - nulltype
        default: null
        help:
            placeholder: "*/30 * * * *"
            description: A crontab string to use for scheduling this recipe.
            link: "https://en.wikipedia.org/wiki/Cron"

    minutes:
        input_type: number
        value_types: 
            - numeric
            - nulltype
        default: null
        help:
            placeholder: 60
            description: The frequency with which this Recipe should run (in minutes).

    time_of_day:
        input_type: select
        input_options:
            - '12:00 AM'
            - '12:30 AM'
            ...
            - '11:30 PM'
        value_types:
            - string 
            - nulltype
        default: null
        help:
            placeholder: '4:30 PM'
            description: The time of day at which this Recipe should run daily.

    timeout:
        input_type: number
        value_types:
            - numeric
        default: 240
        help:
            placeholder: 240
            description: |
                The number of seconds after which this Recipe will time out.


Search
+++++++++++++++++++

As mentioned above, Sous Chefs can accept options with a value type of ``searchstring``. A search string is a built-in type that has the following capabilities (described through examples):

* ``term``
    - match on a term
* ``~term``
    - fuzzy match on a term using jaro-winkler distance
* ``/.*term.*/``
    - apply a regex
* ``"term1 term2"``
    - match on a phrase
* ``~"term1 term2"``
    - fuzzy match on a phrase
* You can chain two search strings together with the following operators:
    - ``AND`` => must match both searchstrings
    - ``&`` => must match both searchstrings
    - ``OR`` => can match either term
    - ``|`` => can match either term
* There is not yet support of parenthetical grouping of chained terms.

Configuring Metrics
++++++++++++++++++++

Sous Chefs which create metrics must also specify the schema of the metrics they create. This schema is specified in the :ref:`metrics` docs.

Examples 
++++++++++++++

The best way to understand how Sous Chef's work is to look at the Source Code for the built-in modules `here <https://github.com/newslynx/newslynx-core/blob/master/newslynx/sc/__init__.py>`_.  You can see an example of a custom Sous Chef module `here <https://github.com/newslynx/newslynx.sc>`_. If you're interested in writing your own Sous Chefs, please refer to the :ref:`writing-sous-chefs` docs.




