# Accessing log files

Depending on the monitoring plugin, you can view and visualize your log files
using either Elasticsearch and Kibana, or Stackdriver Logging.

If you have not yet installed the logging and monitoring components, go through the
[installation instructions](./installing-logging-metrics-traces.md) to set up the
necessary components first.

## Table of contents

* [Elasticsearch and Kibana](#accessing-log-files-with-elasticsearch-and-kibana)
* [Stackdriver Logging](#accessing-log-files-with-stackdriver-logging)


## Accessing log files with Elasticsearch and Kibana

If you have the monitoring plugin for Elasticsearch installed, you can use the
Kibana to view and visualize your log files.

Before you can view and visualize your log files, you must first configure
Kibana by creating index patterns for the indices in Elasticsearch.

The typical work flow for viewing log files is to:

1. [Start Kibana](#starting-kibana)
1. [Configure index patterns](#configuring-index-patterns)
1. [View your log files](#viewing-log-files)

<!-- mdemirhan@ TODO: create a video walkthrough of the Kibana UI -->
---------------------

### Starting Kibana

To use Kibana, you must start a proxy:

1. Start a local proxy for the Kibana UI by running the following command:

   ```shell
   kubectl proxy
   ```

   Kibana uses port `8001`. For security, Kibana is only accessible within the
   cluster.

1. Access Kibana through your browser by navigating to
   http://localhost:8001/api/v1/namespaces/knative-monitoring/services/kibana-logging/proxy/app/kibana.

   Note: It might take a minute or two for proxy to work.

### Configuring index patterns

You need to set index patterns in Kibana that identify indices in Elasticsearch.

After index patterns are initially configured, you only need perform this step
when you want to add or modify patterns.

For Knative, you should create index patterns for the following indices:

  * `Logstash` - Index for application logs.
  * `Zipkin` - Index for request traces.

To create index patterns:

1. After [starting and opening Kibana](#starting-kibana), open the
   [Configure an index pattern](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/management/kibana/indices)
   page:

   * If you have not specified any index patterns, the
     [Configure an index pattern](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/management/kibana/indices)
     page opens by default.

   * To manually navigate to the [Configure an index pattern](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/management/kibana/indices)
     page, you click **Management** > **Index Patterns**.

1. Specify the index pattern by entering the corresponding Elasticsearch index
   in the `Index pattern`. You should append the `*` wildcard to match all
   variants, for example: `logstash-*` or `zipkin*`.

1. In the `Time Filter field name`, indicate the value of the global time
   that you want to use to filter the data.

   Examples:
   * For `logstash-*`, use the `@timestamp` time filter.
   * For `zipkin*`, use the `timestamp_millis` time filter.

   **Tip**: As events are ingested and the data is collected in your log files,
   new `field names` become available as they are discovered.

1. Click `Create` to save the index pattern.

   ![Create logstash-* index](images/kibana-landing-page-configure-index.png)

1. To configure another index pattern, click **Create Index Pattern** near
   the top of the page to reopen the
   [Configure an index pattern](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/management/kibana/index)
   page.

### Viewing log files

After [starting Kibana](#starting-kibana) and
[configuring index patterns](#configuring-index-patterns), you can use the
various pages in Kibana to view, graph, and visualize your logs.

You can open one of the following pages depending on how you want to view your
log files:

 * Use the [Discover](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/discover)
   page to search, filter, and view the data of your log files. Quickly view the
   events of the index patterns that you created. You can also run queries to
   view the log files of your Knative resources, see
   [accessing log files for Knative resources](accessing-log-files-for-Knative-resources)
   for details.

   For example, in the `Discover` page, you can use the controls at the top to
   view specific log files. Specify a time range to filter events or run queries
   to specify Knative resources:

   ![Kibana UI Discover tab](./images/kibana-discover-tab-annotated.png)

 * Use the [Visualize](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/visualize)
   page to create, view, and compare the your log file data in charts, tables,
   and more.

 * Use the [Graph](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/graph)
   page to create and view graphs and explore connections in your log file data.


#### Accessing log files for Knative resources

You use Kibana to view the log files of specific Knative resources but you must
first obtain the names and paths of those resource in your cluster. For example,
you first run `kubectl get` commands to retrieve the name for the following
resources:

* `[configuration](https://github.com/knative/serving/blob/master/docs/spec/spec.md#configuration)
* [revision]()
* [build]()
* requests

To obtain resource names and paths of the log files for your Knative resources:

1. Run the `kubectl get` command to list the Knative resource to which you want
   to view log files:

   * configurations:

      ```shell
      kubectl get configurations
      ```

   * revisions:

      ```shell
      kubectl get revisions
      ```

   * build:

      ```shell
      kubectl get builds
      ```

    Tip: Your build resource names are located in their `.yaml` configuration
    files under,
    [`metadata:` > `name:`](https://github.com/knative/docs/blob/master/build/builds.md#syntax).

1. Open the [Discover] page in Kibana and run the following search query:

      * Replace `<CONFIGURATION_NAME>` and enter the following search query in Kibana:
      ```
      kubernetes.labels.serving_knative_dev\/configuration: <CONFIGURATION_NAME>
      ```

      * Replace `<REVISION_NAME>` and enter the following search query in Kibana:
      ```
      kubernetes.labels.serving_knative_dev\/revision: <REVISION_NAME>
      ```

      * Replace `<BUILD_NAME>` and enter the following search query in Kibana:
      ```
      kubernetes.labels.build\-name: <BUILD_NAME>
      ```



   * To view your Knative request log files:

      To access the request logs, enter the following search in Kibana:

      ```text
      tag: "requestlog.logentry.istio-system"
      ```

        Request logs contain details about requests served by the revision. Below is
        a sample request log:

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

      To learn how to trace requests in Knative, see
      [Accessing Traces](./accessing-traces.md).

## Accessing log files with Stackdriver Logging

If you have the monitoring plugin for Stackdriver installed, you can use the
'Stackdriver Logging' page in Google Cloud Platform to view and visualize
your log files: [**Go to Stackdriver Logging**](https://console.cloud.google.com/logs/viewer)

[Learn more about Stackdriver](https://cloud.google.com/logging/docs/).

---

Except as otherwise noted, the content of this page is licensed under the
[Creative Commons Attribution 4.0 License](https://creativecommons.org/licenses/by/4.0/),
and code samples are licensed under the
[Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).
