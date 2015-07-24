.. _metrics:

Metrics
============================

In our `white paper <http://towcenter.org/wp-content/uploads/2015/06/Tow_Center_NewsLynx_Full_Report.pdf>`_ (PDF, released June 2015), which had originally been focused on impact measurement, we were surprised to learn how important quantitative metrics were. More importantly, we came to understand how newsrooms were getting their numbers from a nearly infinite range of sources. Many newsrooms like `NPR <https://github.com/nprapps/carebot>`_ have even developed metrics that best reflect their unique understanding of their audience. A principal challenge in building NewsLynx was determining how best to collect, store, analayze, and present metrics in a way that allowed Newsrooms to focus on the metrics that mattered most to them.

The system we devised adheres to the following standards:

1. While NewsLynx is an analytics platform, it is not a service for tracking events on sites.  NewsLynx exists in between analytics providers and their audiences as a tool which enables the curation and customization of metrics.
2. For the reasons above, **we do not store timeseries data more granular than hour**. While this is configurable by setting ``metrics_min_date_unit`` and ``metrics_min_date_value`` in your ``config.yaml``, its change is not encouraged.
3. Metrics should be stored in the their rawest state but should be automatically summarized and compared.  The process from ingestion => summary => comparison should be both invisible to the user and effortlessly undone.
4. While we should make no assumptions about the data source a user prefers, we should not have to constantly compromise the above standards to accomodate a particular data source.  With that in mind, we should have a schema for Metrics which, while rigid, is flexible enough to hadle 90% of use cases.  In the cases which this schema does not proove adequate, we should consider whether this particular use case is general enough before modifying the schema.

.. _metrics-schema:
Schema
============================

As described in the :ref:`<sous_chefs>` docs, Metrics are exclusively defined by Sous Chefs. All Sous Chefs that create metrics must have the following section in their configuration:

.. code-block:: yaml

    metrics:
        ga_pageviews:
            display_name: Pageviews
            type: count
            content_levels:
                - timeseries
                - summary
                - comparison
            org_levels:
                - timeseries
                - summary



