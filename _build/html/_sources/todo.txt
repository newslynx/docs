
.. _endpoints-org-metrics-timeseries:
**Org Metrics Timeseries**
+++++++++++++++++++++++++++++
Query timeseries metrics for an organization.

.. _endpoints-orgs-list-timeseries:
**GET** ``/orgs/timeseries``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Query the timeseries for all organizations.

**NOTE**:
  - Only accessible to Super Users.

.. _endpoints-orgs-get-timeseries:
**GET** ``/orgs/:org_id/timeseries``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Query the timeseries store for an organization.

.. _endpoints-orgs-get-timeseries:
**PUT** ``/orgs/:org_id/timeseries``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Refresh an org's timeseries metrics by rolling up it's :ref:`Content Timeseries Metrics `endpoints-content-timeseries>`.


.. _endpoints-org-metrics-summary:
**Org Metrics Summary**
+++++++++++++++++++++++++++++

**GET** ``/orgs/summary``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
List the summary metrics for all organizations. :ref:`Org Timeseries Metrics `endpoints-orgs-get-timeseries>`.

**NOTE**:
  - Only accessible to Super Users.

**GET** ``/orgs/:org_id/summary``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Get an org's summary metrics by rolling up it's :ref:`Org Timeseries Metrics `endpoints-orgs-get-timeseries>`.

**PUT** ``/orgs/:org_id/summary``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Refresh an org's summary metrics by rolling up it's :ref:`Org Timeseries Metrics `endpoints-orgs-get-timeseries>`.

.. _endpoints-org-metrics-summary:
**Org Comparisons**
+++++++++++++++++++++++++++++

.. _endpoints-content-get-comparisons:
**GET** ``/orgs/comparisons``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Get all comparison metrics

.. _endpoints-content-update-comparisons:
**PUT** ``/orgs/comparisons``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Update all comparison metrics

.. _endpoints-content-get-comparisons:
**GET** ``/orgs/comparisons/:type``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Get just this comparison ``type``.

.. _endpoints-content-update-comparisons:
**PUT** ``/orgs/comparisons/:type``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Update just this comparison ``type`` for all orgs.

.. _endpoints-content-item-get-comparisons:
**GET** ``/orgs/:org_id/comparisons``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Compare this Org by all types.

.. _endpoints-content-item-update-comparisons:
**PUT** ``/orgs/:org_id/comparisons``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Update all comparison metrics for this Org.

.. _endpoints-content-item-get-comparisons:
**GET** ``/orgs/:org_id/comparisons/:type``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Update all comparison metrics for this Org.

.. _endpoints-content-item-update-comparisons:
**PUT** ``/orgs/:org_id/comparisons/:type``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Update comparison metrics for this Org and this ``type``.