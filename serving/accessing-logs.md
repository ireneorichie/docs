# Accessing log files

Depending on the monitoring plugin, you can view and visualize your log files
using either Elasticsearch and Kibana, or Stackdriver Logging.

## Table of contents

* [Before you begin](#before-you-begin)
* [Accessing logs with Elasticsearch and Kibana](#accessing-log-files-with-elasticsearch-and-kibana)
* [Accessing logs with Stackdriver Logging](#accessing-log-files-with-stackdriver-logging)

## Before you begin

You must have one of the monitoring plugins installed and configure in your
Knative cluster, for details see
[Installing the Monitoring, Logging, and Tracing Plugin](./installing-logging-metrics-traces.md).

## Accessing log files with Elasticsearch and Kibana

If you have the monitoring plugin for Elasticsearch installed, you can use
Kibana to view and visualize your log files.

If you are unsure if you have the right plugin, you can
[view which monitoring plugin is installed in your cluster](./installing-logging-metrics-traces.md#viewing-which-monitoring-plugin-is-installed).

Before you can use Kibana, you must first configure Kibana by creating index
patterns for the indices that exist in Elasticsearch.

The typical work flow for viewing log files with Elasticsearch and Kibana is:

1. [Start Kibana](#starting-kibana)
1. [Configure index patterns](#configuring-index-patterns)
1. [View your log files](#viewing-log-files)

<!-- mdemirhan@ TODO: create a video walkthrough of the Kibana UI -->
---------------------

### 1. Starting Kibana

To use Kibana, you must start a proxy server:

1. Start a local proxy for the Kibana UI by running the following command:

   ```shell
   kubectl proxy
   ```

   Kibana uses port `8001`. For security, Kibana is only accessible within the
   cluster.

1. Access Kibana through your browser by navigating to
   http://localhost:8001/api/v1/namespaces/knative-monitoring/services/kibana-logging/proxy/app/kibana.

   Note: It might take a minute for the proxy server to start.

### 2. Configuring index patterns

Before you can use Kibana, you need to configure and specify index patterns that
identify the indices in Elasticsearch. After index patterns are initially
configured, you only need perform this step when you want to add or modify
patterns.

For Knative, you should create index patterns for the following indices:

  * `Logstash` - Index for application logs.
  * `Zipkin` - Index for request traces.

To create index patterns:

1. After [starting and opening Kibana](#starting-kibana), open the
   'Configure an index pattern' page:

   * If you have not specified any index patterns, the
     [Configure an index pattern](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/management/kibana/indices)
     page opens by default.

   * To manually navigate to the [Configure an index pattern](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/management/kibana/indices)
     page, you click **Management** > **Index Patterns**.

1. Individually configure an index pattern by entering the corresponding
   Elasticsearch index in the `Index pattern`. You should append the `*`
   wildcard to match all variants, for example: `logstash-*` or `zipkin*`.

1. In the `Time Filter field name`, indicate the value of the global time
   that you want to use to filter the data.

   Examples:
   * For `logstash-*`, use the `@timestamp` time filter.
   * For `zipkin*`, use the `timestamp_millis` time filter.

   **Tip**: The `field names` become available as events are ingested and those
   names are discovered in the collected log file data.

1. Click `Create` to save the index pattern.

   ![Create logstash-* index](images/kibana-landing-page-configure-index.png)

1. To configure another index pattern, click **Create Index Pattern** near
   the top of the page to reopen the
   [Configure an index pattern](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/management/kibana/index)
   page.

### 3. Viewing log files

After [starting Kibana](#starting-kibana) and
[configuring index patterns](#configuring-index-patterns), you can use the
various pages in Kibana to view, graph, and visualize your logs.

You can open one of the following pages depending on how you want to view your
log files:

 * Use the [Discover](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/discover)
   page to search, filter, and view the data of your log files. Quickly view the
   events of the index patterns that you create, and also run queries to find
   and view the log files of your Knative resources.
   [More details in the section below](accessing-log-files-for-Knative-resources).

   For example, you can use the controls at the top of the `Discover` page to
   view specific log files, like Knative resource files, or filter by time
   range:

   ![Kibana UI Discover tab](./images/kibana-discover-tab-annotated.png)

 * Use the [Visualize](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/visualize)
   page to create, view, and compare the your log file data in charts, tables,
   and more.

 * Use the [Graph](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/graph)
   page to create and view graphs and explore connections in your log file data.

[Learn more about Kibana](https://www.elastic.co/guide/en/kibana/current/index.html).

#### Viewing log files for Knative resources

You can use the 'Discover' page of Kibana to view the log files of your
[Knative resources](https://github.com/knative/serving/blob/master/docs/spec/overview.md#service).

Using either of the `logstash-*` or `zipkin*` index patterns, you can locate
events by:

 * Using the list of **Available Fields**. You click **Add** to select and view
   those fields.
 * Running a search query. First obtain the name of the resource by using the
   `kubectl get` command, and then run a query in the `Discover` page.

Knative resources that you can view:

* [`Configuration`](https://github.com/knative/serving/blob/master/docs/spec/spec.md#configuration)
  resources: `kubernetes.labels.serving_knative_dev\/configuration: [RESOURCE_NAME]`
* [`Revision`](https://github.com/knative/serving/blob/master/docs/spec/spec.md#revision)
  resources: `kubernetes.labels.serving_knative_dev\/revision: [RESOURCE_NAME]`
* [`Build`](https://github.com/knative/docs/tree/master/build)
  resources: `kubernetes.labels.build\-name: [RESOURCE_NAME]`

      Tip: Your `Build` resource names are also located in their [`.yaml`
      configuration files](https://github.com/knative/docs/blob/master/build/builds.md#syntax).
* Requests by revision - `istio-system` [namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/): `tag: "requestlog.logentry.istio-system"`
  To learn how to trace requests in Knative, see
  [Accessing Traces](./accessing-traces.md).

#### Requests example

This example demonstrates how you can retrieve and view the available log files
for the request data for an example `Revision` resource.

If the following search query is run in the
[Discover](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/discover)
page:

  ```text
  tag: "requestlog.logentry.istio-system"
  ```

The following example event might get listed. If you click that event,
you can view the details for that that HTTP request, including the
`configuration-example-00001` revision and
`route-example.default.example.com` host names:

  ```text
  @timestamp                   July 10th 2018, 10:09:28.000
  destinationConfiguration     configuration-example
  destinationNamespace         default
  destinationRevision          configuration-example-00001
  destinationService           configuration-example-00001-service.default.svc.cluster.local
  latency                      1.232902ms
  method                       GET
  protocol                     http
  referer                      unknown
  requestHost                  route-example.default.example.com
  requestSize                  0
  responseCode                 200
  responseSize                 36
  severity                     Info
  sourceNamespace              istio-system
  sourceService                unknown
  tag                          requestlog.logentry.istio-system
  traceId                      986d6faa02d49533
  url                          /
  userAgent                    curl/7.60.0
  ```

#### Configuration, Revision, or Build examples

This example demonstrates how to access the log files for
`Configuration`, `Revision`, or `Build` resources:

1. If one of the following `kubectl get` commands are run to retrieve all the
   corresponding Knative resources:

   * `Configuration`:

      ```shell
      kubectl get configurations
      ```

   * `Revision`:

      ```shell
      kubectl get revisions
      ```

   * `Build`:

      ```shell
      kubectl get builds
      ```

1. You can use the name of one of the listed resource names along with the
   corresponding [label](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) to run a search query in the
   [Discover](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/discover)
   page of Kibana:

   * `Configuration`:

      ```text
      kubernetes.labels.serving_knative_dev\/configuration: [RESOURCE_NAME]
      ```

   * `Revision`:

      ```text
      kubernetes.labels.serving_knative_dev\/revision: [RESOURCE_NAME]
      ```

   * `Build`:

      ```text
      kubernetes.labels.build\-name: [RESOURCE_NAME]
      ```
   where [RESOURCE_NAME] is the name that you obtained using the `kubectl get`
   command.

## Accessing log files with Stackdriver Logging

If you have the monitoring plugin for Stackdriver installed, you can use the
'Stackdriver Logging' page in Google Cloud Platform to view and visualize
your log files: [**Open Stackdriver Logging**](https://console.cloud.google.com/logs/viewer)

If you are unsure if you have the right plugin, you can
[view which monitoring plugin is installed in your cluster](./installing-logging-metrics-traces.md#viewing-which-monitoring-plugin-is-installed).

  ![Stackdriver Logging](images/stackdriver-logging.png)

[Learn more about Stackdriver Logging](https://cloud.google.com/logging/docs/).

---

Except as otherwise noted, the content of this page is licensed under the
[Creative Commons Attribution 4.0 License](https://creativecommons.org/licenses/by/4.0/),
and code samples are licensed under the
[Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).
