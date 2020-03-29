.. _arch_overview_postgresql:

PostgreSQL
==========

Envoy supports a network level PostgreSQL sniffing filter to add network observability. By using the
PostgreSQL proxy, Envoy is able to decode `PostgreSQL frontend/backend protocol`_ and gather
statistics from the decoded information.

The main goal of the PostgreSQL filter is to capture runtime statistics without impacting or
generating any load on the PostgreSQL upstream server, it is transparent to it. The filter currently
offers the following features:

* Decrypt non SSL traffic, ignore SSL traffic.
* Decode session information.
* Capture transaction information, including commits and rollbacks.
* Basic decoding of the incoming SQL, exposing counters for different types of
  transactions (INSERTs, DELETEs, UPDATEs, etc).
* Count backend messages, distinguising OK messages, errors and warnings.

The PostgreSQL filter solves a notable problem for PostgreSQL deployments:
gathering this information either imposes additional load to the server; or
requires from pull-based querying metadata from the server, sometimes requiring
external components or extensions. This filter provides valuable observability
information, without impacting the performance of the upstream PostgreSQL
server or requiring the installation of any software.

PostreSQL proxy filter :ref:`configuration reference <config_network_filters_postgresql_proxy>`.

.. _PostgreSQL frontend/backend protocol: https://www.postgresql.org/docs/current/protocol.html
