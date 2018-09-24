# Installing the Monitoring, Logging, and Tracing Plugin

You can install a monitoring plugin in your Knative Serving installation to
enable data collection and logging, metrics, and request tracing in that
cluster.

The monitoring plugin is based on Fluentd, and Knative Serving currently
supports a daemonset configuration for either Elasticsearch or Stackdriver.
[Learn more about the Fluentd container image and requirements](./fluentd/README.md).

If you already have a monitoring plugin installed and configured, see the
following topics for details about accessing the data:

  * [Accessing Logs](./accessing-logs.md)
  * [Accessing Metrics](./accessing-metrics.md)
  * [Accessing Traces](./accessing-traces.md)

To check if you already have monitoring installed, see
[Viewing which monitoring plugin is installed](#viewing-which-monitoring-plugin-is-installed).

## Before you begin

* Before you can install a monitoring plugin, you need to have
  [Knative Serving installed and running](https://github.com/knative/docs/tree/master/install).

* Ensure that you have the Knative Serving repository cloned. Some commands in
  this topic require the daemonset configuration files located in the
  [`knative/serving/config/monitoring`](https://github.com/knative/serving/tree/master/config/monitoring)
  directory. For example, from in a `knative` working directory, you can run the
  following commands to clone and then switch to the `serving` directory:

     ```shell
     git clone https://github.com/knative/serving.git
     cd serving
     git checkout v0.1.1
     ```

### Viewing which monitoring plugin is installed

To determine if you already have a monitoring plugin installed, you can run the
following command to list all the monitoring related pods in your Knative
cluster:

  ```shell
  kubectl get pods --namespace monitoring
  ```

If you have a monitoring plugin installed, see the corresponding section below
for details about configuring that plugin. The following pods are common
between the two types of monitoring plugins:

  ```shell
  fluentd-ds-5kc85
  fluentd-ds-vhrcq
  fluentd-ds-xghk9
  grafana-798cf569ff-v4q74
  kube-state-metrics-75bd4f5b8b-8t2h2
  node-exporter-cr6bh
  node-exporter-mf6k7
  node-exporter-rhzr7
  prometheus-system-0
  prometheus-system-1
  ```

  * See the
    [Elasticsearch](#monitoring-with-elasticsearch-kibana-prometheus-and-grafana)
    section, if the `elasticsearch` and `kibana` pods are also listed, for
    example:

      ```shell
      elasticsearch-logging-0
      elasticsearch-logging-1
      kibana-logging-7d474fbb45-6qb8x
      ```

  * Otherwise, see the
    [Stackdriver](#monitoring-with-stackdriver-prometheus-and-grafana) section.


## Choosing and installing a monitoring plugin

There are two monitoring plugins that you can choose from and use for monitoring
your Knative Serving installation:

* [Elasticsearch & Kibana](#elasticsearch-kibana-prometheus--grafana-setup)
* [Stackdriver](#stackdriver-prometheus--grafana-setup)

Both plugins utilize Prometheus and Grafana for monitoring.

**Note**: Simultaneously monitoring your Knative cluster with both Elasticsearch
and Stackdriver is unsupported. Only a single monitoring plugin can be
configured and run per Knative Serving installation.

If you have a monitoring plugin installed but want to switch to the other type,
see [Uninstalling your monitoring plugin](#uninstalling-your-monitoring-plugin)
for details about uninstalling that monitoring daemonset.


### Monitoring with Elasticsearch, Kibana, Prometheus, and Grafana

You can use [Elasticsearch](https://www.elastic.co/products/elasticsearch),
[Kibana](https://www.elastic.co/products/kibana),
[Prometheus](https://prometheus.io/) & [Grafana](https://grafana.com/) in
combination to monitor your installation of Knative Serving.

This daemonset configuration gets installed by default during the Knative
Serving component installation.

If this monitoring plugin is already installed, you need to
[create Elasticsearch Indices](#create-elasticsearch-indices) before you can
start monitoring your Knative cluster.

#### Installing the Elasticsearch and Kibana monitoring plugin

Use the following steps to run a daemonset that is configured to monitor your
Knative cluster with Elasticsearch, Kibana, Prometheus, and Grafana:

1. Choose a container image that meets the
   [Fluentd image requirements](fluentd/README.md#requirements). For example, you can use the
   public image [k8s.gcr.io/fluentd-elasticsearch:v2.0.4](https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/fluentd-elasticsearch/fluentd-es-image).
   Or you can create a custom one and upload the image to a container registry
   which your cluster has read access to.
1. Follow the instructions in
   ["Setting up a logging plugin"](setting-up-a-logging-plugin.md#Configuring)
   to configure the Elasticsearch components settings.
1. Install Knative monitoring components by running the following command from the root directory of
   [knative/serving](https://github.com/knative/serving) repository:

   ```shell
   kubectl apply --recursive --filename config/monitoring/100-common \
      --filename config/monitoring/150-elasticsearch \
      --filename third_party/config/monitoring/common \
      --filename third_party/config/monitoring/elasticsearch \
      --filename config/monitoring/200-common \
      --filename config/monitoring/200-common/100-istio.yaml
   ```

1. Verify that the daemonset installation successfully completes. Run the
   following command to ensure that all the monitoring pods are all `Running`:

     ```shell
     kubectl get pods --namespace monitoring --watch
     ```
     For example:
     ```shell
     NAME                                  READY     STATUS    RESTARTS   AGE
     elasticsearch-logging-0               1/1       Running   0          2d
     elasticsearch-logging-1               1/1       Running   0          2d
     fluentd-ds-5kc85                      1/1       Running   0          2d
     fluentd-ds-vhrcq                      1/1       Running   0          2d
     fluentd-ds-xghk9                      1/1       Running   0          2d
     grafana-798cf569ff-v4q74              1/1       Running   0          2d
     kibana-logging-7d474fbb45-6qb8x       1/1       Running   0          2d
     kube-state-metrics-75bd4f5b8b-8t2h2   4/4       Running   0          2d
     node-exporter-cr6bh                   2/2       Running   0          2d
     node-exporter-mf6k7                   2/2       Running   0          2d
     node-exporter-rhzr7                   2/2       Running   0          2d
     prometheus-system-0                   1/1       Running   0          2d
     prometheus-system-1                   1/1       Running   0          2d
     ```

1. When all pods are running, hit `CTRL+C` to exit the *watch* mode in the
   terminal.

#### Create Elasticsearch Indices

To visualize logs with Kibana, you need to set which Elasticsearch indices to explore. We will
create two indices in Elasticsearch using `Logstash` for application logs and `Zipkin`
for request traces.

- To open the Kibana UI (the visualization tool for [Elasticsearch](https://info.elastic.co)),
  you must start a local proxy by running the following command:

  ```shell
  kubectl proxy
  ```

  This command starts a local proxy of Kibana on port 8001. For security
  reasons, the Kibana UI is exposed only within the cluster.

- Navigate to the
  [Kibana UI](http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana).
  _It might take a couple of minutes for the proxy to work_.

- Within the "Configure an index pattern" page, enter `logstash-*` to
  `Index pattern` and select `@timestamp` from `Time Filter field name` and
  click on `Create` button.

![Create logstash-* index](images/kibana-landing-page-configure-index.png)

- To create the second index, select `Create Index Pattern` button on top left
  of the page. Enter `zipkin*` to `Index pattern` and select `timestamp_millis`
  from `Time Filter field name` and click on `Create` button.


### Monitoring with Stackdriver, Prometheus, and Grafana

You can use [Stackdriver](https://cloud.google.com/stackdriver/),
[Prometheus](https://prometheus.io/), and [Grafana](https://grafana.com/) in
combination to monitor your installation of Knative Serving, especially when
your Knative Serving installation runs on Google Cloud Platform (GCP).

Note: The sample Stackdriver configuration file works only with GCP. You must
configure and build your own Fluentd container image if either of the following
are true:

 * Your Knative Serving component is not hosted on a GCP based cluster.
 * You want to send logs to another GCP project.

#### Installing the Stackdriver monitoring components

Use the following steps to run a daemonset that is configured to monitor your
Knative cluster with  Stackdriver, Prometheus, and Grafana:

1. Choose a container image that meets the
   [Fluentd image requirements](fluentd/README.md#requirements). For example, you can use a
   public image. Or you can create a custom one and upload the image to a
   container registry which your cluster has read access to.
2. Follow the instructions in
   ["Setting up a logging plugin"](setting-up-a-logging-plugin.md#Configuring)
   to configure the stackdriver components settings.
3. Install Knative monitoring components by running the following command from the root directory of
   [knative/serving](https://github.com/knative/serving) repository:

      ```shell
      kubectl apply --recursive --filename config/monitoring/100-common \
        --filename config/monitoring/150-stackdriver \
        --filename third_party/config/monitoring/common \
        --filename config/monitoring/200-common \
        --filename config/monitoring/200-common/100-istio.yaml
      ```

1. Verify that the daemonset installation successfully completes. Run the
   following command to ensure that all the monitoring pods are all `Running`:

     ```shell
     kubectl get pods --namespace monitoring --watch
     ```
     For example:

     ```shell
     NAME                                  READY     STATUS    RESTARTS   AGE
     fluentd-ds-5kc85                      1/1       Running   0          2d
     fluentd-ds-vhrcq                      1/1       Running   0          2d
     fluentd-ds-xghk9                      1/1       Running   0          2d
     grafana-798cf569ff-v4q74              1/1       Running   0          2d
     kube-state-metrics-75bd4f5b8b-8t2h2   4/4       Running   0          2d
     node-exporter-cr6bh                   2/2       Running   0          2d
     node-exporter-mf6k7                   2/2       Running   0          2d
     node-exporter-rhzr7                   2/2       Running   0          2d
     prometheus-system-0                   1/1       Running   0          2d
     prometheus-system-1                   1/1       Running   0          2d
     ```

1. When all pods are running, hit `CTRL+C` to exit the *watch* mode in the
   terminal.

## Uninstalling your monitoring plugin

You must completely uninstall the daemonset before you can install a different
monitoring plugin configuration.

To remove all of the pods of the monitoring plugin from your Knative Serving
installation, run the following commands:

1. Set a variable to specify which monitoring plugin you wanted uninstalled:

   Tip: To verify which plugin is installed, see
   [Viewing which monitoring plugin is installed](#viewing-which-monitoring-plugin-is-installed).

  * If the Elasticsearch and Kibana pods are installed, run the following
    command to set the variable to the Elasticsearch daemonset configuration:

    ```shell
    export FLUENTD_DAEMONSET_CONFIG="config/monitoring/150-elasticsearch"
    ```

  * Otherwise, set the variable to the Stackdriver daemonset configuration:

    ```shell
    export FLUENTD_DAEMONSET_CONFIG="config/monitoring/150-stackdriver"
    ```

1. Run the following command to uninstall the monitoring plugin, including the
   specified daemonset configuration:


  ```shell
  kubectl delete -f $FLUENTD_DAEMONSET_CONFIG \
      -f third_party/config/monitoring/common/kubernetes/fluentd/fluentd-ds.yaml \
      -f config/monitoring/200-common/100-fluentd.yaml
      -f config/monitoring/200-common/100-istio.yaml
  ```

1. You can verify that all the pods are no longer running with the following
   command:

  ```shell
  kubectl get pods --namespace monitoring
  ```

---

Except as otherwise noted, the content of this page is licensed under the
[Creative Commons Attribution 4.0 License](https://creativecommons.org/licenses/by/4.0/),
and code samples are licensed under the
[Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).
