Proxysql-chart
===========

ProxySQL 2.X Helm chart for Kubernetes

## Introduction

This chart bootstraps a proxysql deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

  - Kubernetes 1.10+
  - Helm 3.+

## Installation
To install the chart with the release name `proxysql`:

```console
$ helm install proxysql proxysql-chart
```

## Uninstalling the Chart

To uninstall/delete the `proxysql` deployment:

```console
$ helm delete proxysql
```
## Testing the Chart

First, install the chart on your cluster to create a release. You may have to wait for all pods to become active; if you test immediately after this install, it is likely to show a transitive failure, and you will want to re-test.

```console
$ helm install proxysql proxysql-chart
$ helm test proxysql
```

## Configuration

The following table lists the configurable parameters of the Proxysql-chart chart and their default values.

| Parameter                | Description             | Default        |
| ------------------------ | ----------------------- | -------------- |
| `replicaCount` | Desired number of pods | `1` |
| `image.repository` | Repository to use for the proxysql | `"severalnines/proxysql"` |
| `image.pullPolicy` | Container image pull policy | `"IfNotPresent"` |
| `image.tag` | Container image tag | `"2.0.13"` |
| `imagePullSecrets` | Name of Secret resource containing private registry credentials | `[]` |
| `nameOverride` |  | `""` |
| `fullnameOverride` |  | `""` |
| `serviceAccount.create` |  | `true` |
| `serviceAccount.annotations` |  | `{}` |
| `serviceAccount.name` |  | `""` |
| `podAnnotations` | Annotations to be added to pods | `{}` |
| `podSecurityContext` | Security context policies to add to the pod | `{}` |
| `securityContext` | Security context policies to add  | `{}` |
| `service.type` | Type of service to create | `"ClusterIP"` |
| `service.port` |  Sets service proxysql port | `6037` |
| `ingress.enabled` |  | `false` |
| `ingress.annotations` |  | `{}` |
| `ingress.hosts` |  | `[{"host": "chart-example.local", "paths": []}]` |
| `ingress.tls` |  | `[]` |
| `resources` |  | `{}` |
| `autoscaling.enabled` |  If true, creates Horizontal Pod Autoscaler | `false` |
| `autoscaling.minReplicas` | If autoscaling enabled, this field sets minimum replica count | `1` |
| `autoscaling.maxReplicas` | If autoscaling enabled, this field sets maximum replica count | `100` |
| `autoscaling.targetCPUUtilizationPercentage` |  Target CPU utilization percentage to scale | `80` |
| `nodeSelector` | node labels for pod assignment | `{}` |
| `tolerations` |  | `[]` |
| `affinity` |  | `{}` |
| `mysql.readers` | Same as below, but for reader | `[{"endpoint": "db1_reader_endpoint", "port": 3306, "max_connections": 100}]` |
| `mysql.writer.endpoint` | DB endpoint that this cluster can reach | `"db_writer_endpoint"` |
| `mysql.writer.port` | DB Writer port connection  | `3306` |
| `mysql.writer.max_connections` | DB Writer max connection he can handle | `100` |
| `mysql.username` | Username used for connection of the writer/reader | `"username"` |
| `mysql.password` | Password used for connection of the writer/reader | `"password"` |
| `proxysql.check_type` | For aws aurora: innodb_read_only | `"read_only"` |
| `proxysql.threads` | The number of background threads that ProxySQL uses in order to process MySQL traffic | `4` |
| `proxysql.max_connections` | The maximum number of client connections that the proxy can handle. After this number is reached, new connections will be rejected with the #HY000 error, and the error message Too many connections. | `2048` |
| `proxysql.default_query_delay` | Simple throttling mechanism for queries to the backends. Setting this variable to a non-zero value (in miliseconds) will delay the execution of all queries, globally. There is a more fine-grained throttling mechanism in the admin table mysql_query_rules, where for each rule there can be one delay that is applied to all queries matching the rule. That extra delay is added on top of the default, global one. | `0` |
| `proxysql.default_query_timeout` | Mechanism for specifying the maximal duration of queries to the backend MySQL servers until ProxySQL should return an error to the MySQL client. Whenever ProxySQL detects that a query has timed out, it will spawn a separate thread that runs a KILL query against the specific MySQL backend in order to stop the query from running in the backend. Because the query is killed, an error will be returned to the MySQL client. | `36000000` |
| `proxysql.have_compress` | Currently unused. | `true` |
| `proxysql.poll_timeout` | The minimal timeout used by the proxy in order to detect incoming/outgoing traffic via the poll() system call. If the proxy determines that it should stick to a higher timeout because of its internal computations, it will use that one, but it will never use a value less than this one. | `2000` |
| `proxysql.interfaces` | Semicolon-separated list of hostname:port entries for interfaces for incoming MySQL traffic. Note that this also supports UNIX domain sockets for the cases where the connection is done from an application on the same machine. | `"0.0.0.0:6033;/tmp/proxysql.sock"` |
| `proxysql.default_schema` | The default schema to be used for incoming MySQL client connections which do not specify a schema name. This is required because ProxySQL doesn’t allow connection without a schema. | `"information_schema"` |
| `proxysql.stacksize` | The stack size to be used with the background threads that the proxy uses to handle MySQL traffic and connect to the backends. Note that changing this value has no effect at runtime, if you need to change it you have to restart the proxy. | `1048576` |
| `proxysql.server_version` | The server version with which the proxy will respond to the clients. Note that regardless of the versions of the backend servers, the proxy will respond with this. | `"5.1.30"` |
| `proxysql.connect_timeout_server` | The timeout for a single attempt at connecting to a backend server from the proxy. If this fails, according to the other parameters, the attempt will be retried until too many errors per second are generated (and the server is automatically shunned) or until the final cut-off is reached and an error is returned to the client (see mysql-connect_timeout_server_max). | `10000` |
| `proxysql.monitor_history` | The duration for which the events for the checks made by the Monitor module are kept. Such events include connecting to backend servers (to check for connectivity issues), querying them with a simple query (in order to check that they are running correctly) or checking their replication lag. These logs are kept in the following admin tables: | `60000` |
| `proxysql.monitor_connect_interval` | The interval at which the Monitor module of the proxy will try to connect to all the MySQL servers in order to check whether they are available or not. | `200000` |
| `proxysql.monitor_ping_interval` | The interval at which the Monitor module should ping the backend servers by using the mysql_ping API. | `200000` |
| `proxysql.ping_interval_server_msec` | The interval at which the proxy should ping backend connections in order to maintain them alive, even though there is no outgoing traffic. The purpose here is to keep some connections alive in order to reduce the latency of new queries towards a less frequently used destination backend server. | `10000` |
| `proxysql.ping_timeout_server` | The proxy internally pings the connections it has opened in order to keep them alive. This eliminates the cost of opening a new connection towards a hostgroup when a query needs to be routed, at the cost of additional memory footprint inside the proxy and some extra traffic. This is the timeout allowed for those pings to succeed. | `200` |
| `proxysql.commands_stats` | Enable per-command MySQL query statistics. A command is a type of SQL query that is being executed. Some examples are: SELECT, INSERT or ALTER TABLE. | `true` |
| `proxysql.sessions_sort` | This variable controls whether sessions should be processed in the order of waiting time, in order to have a more balanced distribution of traffic among sessions.  | `true` |
| `proxysql.monitor_username` | Specifies the username that the Monitor module will use to connect to the backends. | `"username"` |
| `proxysql.monitor_password` | Specifies the password that the Monitor module will use to connect to the backends. | `"password"` |


  