.. _metrics:

Metrics
============================

In our `white paper <http://towcenter.org/wp-content/uploads/2015/06/Tow_Center_NewsLynx_Full_Report.pdf>`_ (PDF, released June 2015), which had originally been focused on impact measurement, we were surprised to learn how important quantitative metrics were. More importantly, we came to understand how newsrooms were getting their numbers from a nearly infinite range of sources. Many newsrooms like `NPR <https://github.com/nprapps/carebot>`_ have even developed metrics that best reflect their unique understanding of their audience. A principal challenge in building NewsLynx was determining how best to collect, store, analayze, and present metrics in a way that allowed Newsrooms to focus on the metrics that mattered most to them.

The system we devised adheres to the following standards:

.. _metrics-principle-1:
1. While NewsLynx is an analytics platform, it is not a service for tracking events on sites.  NewsLynx exists in between analytics providers and their audiences as a tool which enables the curation and customization of metrics.

.. _metrics-principle-2:
2. For the reasons above, **we do not store timeseries data more granular than hour**. While this is configurable by setting ``metrics_min_date_unit`` and ``metrics_min_date_value`` in your ``config.yaml``, it's not encouraged.

.. _metrics-principle-3:
3. Metrics should be stored in the their rawest state but should be automatically summarized and compared.  The process from ingestion => summary => comparison should be both invisible to the user and effortlessly undone.

.. _metrics-principle-4:
4. While we should make no assumptions about the data source a user prefers, we should not have to constantly compromise the above standards to accomodate one source.  With that in mind, we should have a schema for Metrics which, while rigid, is flexible enough to handle most use cases.  In the cases which this schema does not prove adequate, we should consider whether this particular use case is general enough before modifying the schema.

.. _metrics-schema:

Creating Metrics
============================

As described in the :ref:`Sous Chef docs <sous_chefs>`, Metrics are exclusively defined by the Sous Chefs which create them. All Sous Chefs that create metrics must have the following section in their configuration:

.. code-block:: yaml
    ...
    metrics:
        metric_name:
            **options
        anoter_metric_name:
            **options
        ...

Options 
++++++++++++++++

All metrics have access to the following options:

* ``display_name``
    - How should this metric be displated?

* ``description``
    - An informative explanation of what this metric represents.

* ``type`` 
    - Helps in determining how metric should be interpreted / summarized / presented.
    - Can be one of:
        - ``count``
            - A number of something, 
            - e.g. pageviews, time on page, attention minutes, etc.
            - ``count`` metrics are also fill-ins for Binary or Boolean fields. Simply store these values as ``0`` or ``1``. If you'd like to summarize them as Booleans, set the ``agg`` to ``max``.
            - Metrics with this type will be assumed to have an ``agg`` of ``sum`` unless overridden.
        - ``cumulative``
            - A count that increases over time.
            - This is a special type of Metric in that we should like to capture its differences over time periods.
              However, we also want to avoid alteration of the original source of the data.  As a result, a ``cumulative`` metric is stored as cumulative sum, but when queried, is transformed into a ``count``.  
            - e.g. twitter shares, facebook comments, followers, etc.
            - Metrics with this type will be assumed to have an ``agg`` of ``sum`` unless overridden.
        - ``median``
            - The median of a list of metrics.
            - e.g. median time on page.
            - Metrics with this type will be assumed to have an ``agg`` of ``median`` unless overridden.
        - ``average``
            - The average of a list of metrics.
            - e.g. average time on page.
            - Metrics with this type will be assumed to have an ``agg`` of ``average`` unless overridden.
        - ``percentile``
            - Usually a number beween 0 - 100.
            - e.g. percent internal traffic
            - Metrics with this type will be assumed to have an ``agg`` of ``avg`` unless overridden.
        - ``min_rank``
            - A number which should be interpreted as "a lower number is good."
            - e.g. position on homepage.
            - Metrics with this type will be assumed to have an ``agg`` of ``min`` unless overridden.
        - ``max_rank``
            - A number which should be interpreted as "a higher number is good."
            - e.g. position on homepage.
            - Metrics with this type will be assumed to have an ``agg`` of ``max`` unless overridden.

* ``agg``
    - The function to use when aggregating this metric.
    - In practice, these map directly onto ``postgres`` functions. 
    - Can be one of (for now):
        - ``sum``
        - ``avg`` 
        - ``median`` 
        - ``max`` 
        - ``min``

* ``content_levels``
    - This field lets us know that the metric is related to content items and should be stored at the specified level. For more on what this means see :ref:`metrics-how-does-this-work`
    - Can be one of:
        - ``timeseries``
            - Accessible via the :ref:`Content Timeseries API <endpoints-content-metrics-get-timeseries>`.
        - ``summary``
            - Accessible via the :ref:`Content Search API endpoints-content-items-search` and when retrieving :ref:`individual Content Items <endpoints-content-items-get>`.
        - ``comparison``
            - Accessible via :ref:`endpoints-content-metrics-get-comparisons`

* ``org_levels``
    - This field lets us know that the metric is linked to an organization as a whole. For more on what this means see :ref:`metrics-how-does-this-work`

    - Can be one of:
        - ``timeseries``
            - Accessible via :ref:`endpoints-org-metrics-get-timeseries`
        - ``summary``
            - Accessible via :ref:`endpoints-org-metrics-get-summary`



For instance, the Sous Chef ``google-analytics-to-content-timeseries`` lists this metric configuration:

.. code-block:: yaml

    metrics:
        ga_pageviews:
            display_name: Pageviews 
            description: |
                The number of times this page was opened, 
                as reported by Google Analytics.  
            type: count 
            content_levels:
                - timeseries
                - summary
                - comparison
            org_levels:
                - timeseries
                - summary

        ga_total_time_on_page:
            display_name: Total Time on Page
            description: |
                The total time visitors spent on this page, 
                as reported by Google Analytics.
            type: count
            content_levels:
                - timeseries
                - summary
                - comparison
            org_levels:
                - timeseries
                - summary

        ga_avg_time_on_page:
            display_name: Average Time on Page 
            type: computed
            content_levels:
                - timeseries 
                - summary 
                - comparison 
            org_levels:
                - timeseries
                - summary 
            formula: '{ga_total_time_on_page} / NULLIF({ga_pageviews}, 0)'
            agg: avg




