# Installing the Monitoring, Logging, and Tracing Plugin

You can install a monitoring plugin in your Knative Serving installation to
enable data logging, metrics, and request tracing in that cluster.

The monitoring plugin is based on Fluentd, and Knative Serving currently
supports a
[daemonset](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
configuration for either Elasticsearch or Stackdriver, both of which are
configurable.
[Learn more about the Fluentd container image and requirements](./fluentd/README.md).

If you already have a monitoring plugin installed and configured, see the
following topics for details about accessing the data:

  * [Accessing Logs](./accessing-logs.md)
  * [Accessing Metrics](./accessing-metrics.md)
  * [Accessing Traces](./accessing-traces.md)

## Table of contents

* [Before you begin](#before-you-begin)
* [Viewing which monitoring plugin is installed](#viewing-which-monitoring-plugin-is-installed)
* [Choosing and installing a monitoring plugin](#choosing-and-installing-a-monitoring-plugin)
  * [Monitoring with Elasticsearch](#monitoring-with-elasticsearch-kibana-prometheus-and-grafana)
  * [Monitoring with Stackdriver](#monitoring-with-stackdriver-prometheus-and-grafana)
* [Uninstalling your monitoring plugin](#uninstalling-your-monitoring-plugin)

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

* Before trying to install a monitoring plugin, you should first
  [check to see if a monitoring plugin is already
  installed](#viewing-which-monitoring-plugin-is-installed).

## Viewing which monitoring plugin is installed

To determine if you already have a monitoring plugin installed, you can run the
following command to list all the monitoring related pods in your Knative
cluster:

  ```shell
  kubectl get pods --namespace knative-monitoring
  ```

If either monitoring plugin is installed, the following common pods are
listed:

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
    section for more information if the `elasticsearch` and `kibana` pods are
    also listed. For example:

      ```shell
      elasticsearch-logging-0
      elasticsearch-logging-1
      kibana-logging-7d474fbb45-6qb8x
      ```

  * See the
    [Stackdriver](#monitoring-with-stackdriver-prometheus-and-grafana) section
    for more information if only the common set of pods are listed.

Note that the suffix of each pod differs for every Knative cluster.

## Choosing and installing a monitoring plugin

You can choose from either an
[Elasticsearch and Kibana](#monitoring-with-elasticsearch-kibana-prometheus-and-grafana)
based configuration, or
[Stackdriver](#monitoring-with-stackdriver-prometheus-and-grafana) configuration
for monitoring your Knative Serving installation. Both types of monitoring
plugin utilize Prometheus and Grafana for visualizing metrics data.

**Important**: Simultaneously monitoring your Knative cluster with both
Elasticsearch and Stackdriver is unsupported. Only a single monitoring plugin
can be configured and run per Knative Serving installation.

If you have a monitoring plugin installed but want to switch to the other type,
you must first
[uninstall the existing monitoring plugin](#uninstalling-your-monitoring-plugin)
before you can install another.


### Monitoring with Elasticsearch, Kibana, Prometheus, and Grafana

You can use [Elasticsearch](https://www.elastic.co/products/elasticsearch),
[Kibana](https://www.elastic.co/products/kibana),
[Prometheus](https://prometheus.io/) & [Grafana](https://grafana.com/) in
combination to monitor your installation of Knative Serving. This configuration
is supported across the various Knative cluster hosts.

**Note**: This daemonset configuration gets installed by default during the
Knative Serving component installation.

#### Installing the Elasticsearch and Kibana based monitoring plugin

Use the following steps to install and run a daemonset configuration for
Elasticsearch, Kibana, Prometheus, and Grafana:

1. Choose a Fluentd based container image to use for the monitoring daemonset:

   * You can use the daemonset configuration files provided in the
     `knative/serving` repository, which use the public
     [`k8s.gcr.io/fluentd-elasticsearch:v2.0.4`](https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/fluentd-elasticsearch/fluentd-es-image)
     container image.

   * Alternatively, you can specify and create a custom container image. Your
     custom container image must meet the
     [Knative requirements for Fluentd](./fluentd/README.md). That container
     image must also be hosted in a container registry and accessible to your
     Knative cluster.

1. Optional: If you create a custom Fluentd container image, you can reuse and
   modify the provided daemonset configuration file to specify the details of
   your image.

     The
     [`fluentd-ds.yaml`](https://github.com/knative/serving/blob/master/third_party/config/monitoring/common/kubernetes/fluentd/fluentd-ds.yaml)
     daemonset configuration file is located in the
     `third_party/config/monitoring/common/` directory. Open the file in an
     editor and modify elements to align with your custom image. For example,
     replace the `image` and `version` values.

     In this example, the `CUSTOM-VERSION` and `gcr.io/FLUENT-CUSTOM-IMAGE`
     values are fake:

     ```shell
     ...
     metadata:
     ...
         version: CUSTOM-VERSION
     ...
     spec:
     ...
         image: gcr.io/FLUENT-CUSTOM-IMAGE
     ...
     ```

1. Optional: Skip this step if you want to use the default settings
   of the provided daemonset configuration files.

   To modify the default settings and specify how you want the monitoring plugin
   to output your data, follow the instructions in the
   ["Setting up a logging plugin"](setting-up-a-logging-plugin.md#Configuring)
   topic.

1. Install and run the daemonset by running the following command from the
   `knative/serving` directory where you cloned the
   [repository](https://github.com/knative/serving):

     ```shell
     kubectl apply --recursive --filename config/monitoring/100-common \
        --filename config/monitoring/150-elasticsearch \
        --filename third_party/config/monitoring/common \
        --filename third_party/config/monitoring/elasticsearch \
        --filename config/monitoring/200-common \
        --filename config/monitoring/200-common/100-istio.yaml
     ```

1. Verify that the daemonset installation successfully completes. Run the
   following command to watch and ensure that all the monitoring pods make it to
   the `Running` state:

     ```shell
     kubectl get pods --namespace knative-monitoring --watch
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

1. When all pods are running, hit `CTRL+C` in the terminal to exit the *watch*
   mode.

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

#### Installing the Stackdriver based monitoring plugin

Use the following steps to install and run a daemonset configuration for
Stackdriver, Prometheus, and Grafana:

1. Choose a container image that meets the
   [Fluentd image requirements](fluentd/README.md#requirements). For example, you can use a
   public image. Or you can create a custom one and upload the image to a
   container registry which your cluster has read access to.
1. Follow the instructions in
   ["Setting up a logging plugin"](setting-up-a-logging-plugin.md#Configuring)
   to configure the stackdriver components settings.
1. Install the monitoring plugin by running the following command from the
   `knative/serving` directory where you cloned the
   [repository](https://github.com/knative/serving):

      ```shell
      kubectl apply --recursive --filename config/monitoring/100-common \
        --filename config/monitoring/150-stackdriver \
        --filename third_party/config/monitoring/common \
        --filename config/monitoring/200-common \
        --filename config/monitoring/200-common/100-istio.yaml
      ```

1. Verify that the daemonset installation successfully completes. Run the
   following command to watch and ensure that all the monitoring pods make it to
   the `Running` state:

     ```shell
     kubectl get pods --namespace knative-monitoring --watch
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

1. When all pods are running, hit `CTRL+C` in the terminal to exit the *watch*
   mode.

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
    kubectl delete --filename $FLUENTD_DAEMONSET_CONFIG \
        --filename third_party/config/monitoring/common/kubernetes/fluentd/fluentd-ds.yaml \
        --filename config/monitoring/200-common/100-fluentd.yaml \
        --filename config/monitoring/200-common/100-istio.yaml
    ```

1. You can verify that all the pods are no longer running with the following
   command:

    ```shell
    kubectl get pods --namespace knative-monitoring
    ```

---

Except as otherwise noted, the content of this page is licensed under the
[Creative Commons Attribution 4.0 License](https://creativecommons.org/licenses/by/4.0/),
and code samples are licensed under the
[Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).
