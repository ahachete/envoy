.. _config_network_filters_postgresql_proxy:

PostgreSQL proxy
================

The PostgreSQL proxy filter decodes the wire protocol between a PostgreSQL client (downstream) and a PostgreSQL server
(upstream). The decoded information is currently used only to produce PostgreSQL level statistics like sesions,
statements or transactions executed, among others. This current version does not decode SQL queries. Future versions may
add more statistics and more advanced capabilities.

When the PostgreSQL filter detects that a session is encrypted (SSL), the messages are ignored and no decoding takes
place.


.. attention::

   The `postgresql_proxy` filter is experimental and is currently under active development.
   Capabilities will be expanded over time and the configuration structures are likely to change.


.. warning::

   The `postgreql_proxy` filter was tested only with
   `PostgreSQL frontend/backend protocol version 3.0`_, which was introduced in
   PostgreSQL 7.4. Earlier versions are thus not supported. Testing is limited
   anyway to not EOL-ed versions.

.. _PostgreSQL frontend/backend protocol version 3.0: https://www.postgresql.org/docs/current/protocol.html


Configuration
-------------

The PostgreSQL proxy filter should be chained with the TCP proxy as shown in the configuration
example below:

.. code-block:: yaml

    filter_chains:
    - filters:
      - name: envoy.filters.network.postgresql_proxy
        typed_config:
          "@type": type.googleapis.com/envoy.config.filter.network.postgresql_proxy.v2alpha.PostgreSQLProxy
          stat_prefix: postgresql
      - name: envoy.tcp_proxy
        typed_config:
          "@type": type.googleapis.com/envoy.config.filter.network.tcp_proxy.v2.TcpProxy
          stat_prefix: tcp
          cluster: postgresql_cluster

* :ref:`v2 API reference <envoy_api_field_listener.Filter.name>`

.. _config_network_filters_postgresql_proxy_stats:


Statistics
----------

Every configured PostgreSQL proxy filter has statistics rooted at postgresql.<stat_prefix> with the following statistics:

.. csv-table::
  :header: Name, Type, Description
  :widths: 2, 1, 3

  messages_backend, Counter, Total number of backend messages detected by the filter
  messages_backend_error, Counter, Number of times the server replied with error
  messages_backend_ok, Counter, Number of times the server replied OK
  messages_backend_unknown, Counter, Number of times the proxy successfully decoded a message but did not know what to do with it
  messages_backend_warning, Counter, Number of time the server replied with warning
  messages_frontend, Counter, Number of frontend messages detected by the filter
  sessions, Counter, Total number of successful logins
  sessions_ssl, Counter, Number of times the proxy detected encrypted (SSL) sessions
  sessions_nossl, Counter, Number of messages indicating unencrypted (no SSL) successful login
  statements, Counter, Total number of SQL statements
  statements_delete, Counter, Number of DELETE statements
  statements_insert, Counter, Number of INSERT statements
  statements_select, Counter, Number of SELECT statements
  statements_update, Counter, Number of UPDATE statements
  statements_other, Counter, "Number of statements other than DELETE, INSERT, SELECT or UPDATE"
  transactions, Counter, Total number of SQL transactions
  transactions_commit, Counter, Number of COMMIT transactions
  transactions_rollback, Counter, Number of ROLLBACK transactions
