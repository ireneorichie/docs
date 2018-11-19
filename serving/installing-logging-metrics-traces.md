# Installing an Observability Plugin

You can install an observability plugin in your Knative Serving installation to
enable data logging, metrics, and request tracing in that cluster.

The observability plugin is based on Fluentd, and Knative Serving currently
supports a
[daemonset](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
configuration for either Elasticsearch or Stackdriver.
[Learn more about the Fluentd container image and requirements](./fluentd/README.md).

If you already have an observability plugin installed and configured, see the
following topics for details about accessing the data:

  * [Accessing Logs](./accessing-logs.md)
  * [Accessing Metrics](./accessing-metrics.md)
  * [Accessing Traces](./accessing-traces.md)

Optionally, you can
[customize the default output configuration](setting-up-a-logging-plugin.md)
of your observability plugin.

## Table of contents

* [Before you begin](#before-you-begin)
* [Viewing which observability plugin is installed](#viewing-which-observability-plugin-is-installed)
* [Choosing and installing an observability plugin](#choosing-and-installing-a-observability-plugin)
  * [Monitoring with Elasticsearch](#monitoring-with-elasticsearch-kibana-prometheus-and-grafana)
  * [Monitoring with Stackdriver](#monitoring-with-stackdriver-prometheus-and-grafana)
* [Uninstalling your observability plugin](#uninstalling-your-observability-plugin)

## Before you begin

* Before you can install an observability plugin, you need to have
  [Knative Serving installed and running](https://github.com/knative/docs/tree/master/install).

* Ensure that you have the Knative Serving repository cloned. Some commands in
  this topic require the daemonset configuration files located in the
  [`knative/serving/config/monitoring`](https://github.com/knative/serving/tree/master/config/monitoring)
  directory. For example, from in a `knative` working directory, you can run the
  following commands to clone and then switch to the `serving` directory:

     ```shell
     git clone https://github.com/knative/serving.git
     cd serving
     git checkout v0.2.2
     ```

* Before trying to install an observability plugin, you should first
  [check to see if plugin is already installed on your cluster](#viewing-which-oberservability-plugin-is-installed).

## Viewing which observability plugin is installed

To determine if you already have an observability plugin installed, you can run the
`kubectl get` command to list all the pods in `knative-monitoring` namespace:

  ```shell
  kubectl get pods --namespace knative-monitoring
  ```

  Note that the suffix of each pod differs in every cluster.

* If no pods are listed,
  [learn how to choose and install an observability plugin](#choosing-and-installing-a-monitoring-plugin).

* If either observability plugin is installed, the following common pods are
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
      section for more information if the following `elasticsearch` and `kibana`
      pods are also listed:

        ```shell
        elasticsearch-logging-0
        elasticsearch-logging-1
        kibana-logging-7d474fbb45-6qb8x
        ```

    * See the
      [Stackdriver](#monitoring-with-stackdriver-prometheus-and-grafana) section
      for more information if only the common set of pods are listed.



## Choosing and installing an observability plugin

You can choose from either an
[Elasticsearch and Kibana](#monitoring-with-elasticsearch-kibana-prometheus-and-grafana)
based configuration, or
[Stackdriver](#monitoring-with-stackdriver-prometheus-and-grafana) configuration
to monitor your Knative cluster. Both types of monitoring
plugin utilize Prometheus and Grafana for visualizing metrics data.

**Important**: Simultaneously monitoring your Knative cluster with both
Elasticsearch and Stackdriver is unsupported. Only a single observability plugin
can be configured and run per Knative Serving installation.

If you have an observability plugin installed but want to switch to another plugin,
you must first
[uninstall the existing observability plugin](#uninstalling-your-observability-plugin)
before you can install another.


### Monitoring with Elasticsearch, Kibana, Prometheus, and Grafana

You can use [Elasticsearch](https://www.elastic.co/products/elasticsearch),
[Kibana](https://www.elastic.co/products/kibana),
[Prometheus](https://prometheus.io/) & [Grafana](https://grafana.com/) in
combination to monitor your installation of Knative Serving. This configuration
is supported across the various Knative cluster hosts.

**Note**: This daemonset configuration gets installed by default during the
Knative Serving component installation.

#### Installing the Elasticsearch and Kibana based observability plugin

Use the following steps to install and run a daemonset configuration for an
observability plugin based on Elasticsearch, Kibana, Prometheus, and Grafana:

1. Choose a Fluentd based container image to use for the daemonset that runs the
   observability plugin:

   * You can use the daemonset configuration files provided in the
     `knative/serving` repository, which use the public
     [`k8s.gcr.io/fluentd-elasticsearch:v2.0.4`](https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/fluentd-elasticsearch/fluentd-es-image)
     container image.

   * Alternatively, you can create a daemonset that usese a custom container image.
     Your custom Fluentd-based observability plugin must meet the
     [Knative requirements for Fluentd](./fluentd/README.md).

     After creating your custom Fluentd container image, you must hosted that image
     in a container registry that is accessible by your cluster.

     Tip: You can create a custom Fluentd container image by modifying the
     pre-configured daemonset file:
     [`fluentd-ds.yaml`](https://github.com/knative/serving/blob/master/third_party/config/monitoring/common/kubernetes/fluentd/fluentd-ds.yaml)

     For example, you can customize the `image` and `version` elements throughout the
     `fluentd-ds.yaml` file so that they define your custom container image:

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

       where `CUSTOM-VERSION` and `gcr.io/FLUENT-CUSTOM-IMAGE` are example values.

1. Optional: Customize the default observability plugin output. You can skip this
   step if you want to keep the default settings.

   To modify the default settings and specify how you want the observability plugin
   to output your data, follow the instructions in the
   ["Setting up a logging plugin"](setting-up-a-logging-plugin.md)
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

1. Learn how to access your log data:

   * [Accessing Logs](./accessing-logs.md)
   * [Accessing Metrics](./accessing-metrics.md)
   * [Accessing Traces](./accessing-traces.md)

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

#### Installing the Stackdriver based observability plugin

Use the following steps to install and run a daemonset configuration for a
observability plugin based on Stackdriver, Prometheus, and Grafana:

1. Choose a Fluentd based container image to use for the daemonset that runs the
   observability plugin:

   * You can use the daemonset configuration files provided in the
     `knative/serving` repository, which use the public
     [`k8s.gcr.io/fluentd-elasticsearch:v2.0.4`](https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/fluentd-elasticsearch/fluentd-es-image)
     container image.

   * Alternatively, you can create a daemonset that usese a custom container image.
     Your custom Fluentd-based observability plugin must meet the
     [Knative requirements for Fluentd](./fluentd/README.md).

     After creating your custom Fluentd container image, you must hosted that image
     in a container registry that is accessible by your cluster.

     Tip: You can create a custom Fluentd container image by modifying the
     pre-configured daemonset file:
     [`fluentd-ds.yaml`](https://github.com/knative/serving/blob/master/third_party/config/monitoring/common/kubernetes/fluentd/fluentd-ds.yaml)

     For example, you can customize the `image` and `version` elements throughout the
     `fluentd-ds.yaml` file so that they define your custom container image:

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

       where `CUSTOM-VERSION` and `gcr.io/FLUENT-CUSTOM-IMAGE` are example values.

1. Optional: Customize the default observability plugin output. You can skip this
   step if you want to keep the default settings.

   To modify the default settings and specify how you want the observability plugin
   to output your data, follow the instructions in the
   ["Setting up a logging plugin"](setting-up-a-logging-plugin.md)
   topic.

1. Install and run the daemonset by running the following command from the
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

1. Learn how to access your log data:

   * [Accessing Logs](./accessing-logs.md)
   * [Accessing Metrics](./accessing-metrics.md)
   * [Accessing Traces](./accessing-traces.md)

## Uninstalling your observability plugin

You must completely uninstall the daemonset for your observability plugin before
you can install and run a different plugin.

To remove all of the pods of the observability plugin from your Knative Serving
installation, run the following commands:

1. Create a command line variable to specify which observability plugin you
   want removed from your cluster:

   Tip: To verify which plugin is installed, see
   [Viewing which observability plugin is installed](#viewing-which-observability-plugin-is-installed).

    * If the Elasticsearch and Kibana pods are installed, run the following
      command to set a variable for the Elasticsearch daemonset configuration:

      ```shell
      export FLUENTD_DAEMONSET_CONFIG="config/monitoring/150-elasticsearch"
      ```

    * Otherwise, set a variable for the Stackdriver daemonset configuration:

      ```shell
      export FLUENTD_DAEMONSET_CONFIG="config/monitoring/150-stackdriver"
      ```

1. Run the following command to uninstall the observability plugin:

    ```shell
    kubectl delete --filename $FLUENTD_DAEMONSET_CONFIG \
        --filename third_party/config/monitoring/common/kubernetes/fluentd/fluentd-ds.yaml \
        --filename config/monitoring/200-common/100-fluentd.yaml \
        --filename config/monitoring/200-common/100-istio.yaml
    ```

1. You can run the following command to verify that all the pods of the
   observability plugin have been deleted:

    ```shell
    kubectl get pods --namespace knative-monitoring
    ```

---

Except as otherwise noted, the content of this page is licensed under the
[Creative Commons Attribution 4.0 License](https://creativecommons.org/licenses/by/4.0/),
and code samples are licensed under the
[Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).
