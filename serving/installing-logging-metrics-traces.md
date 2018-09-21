# Installing the Monitoring, Logging, and Tracing Components

You can install and run components that allow you to collect and log data,
view metrics, and trace requests.

If you already have monitoring installed and configured, see the following
topics for details about accessing the data:

  * [Accessing Logs](./accessing-logs.md)
  * [Accessing Metrics](./accessing-metrics.md)
  * [Accessing Traces](./accessing-traces.md)

## Before you begin

Ensure that you have the Knative Serving repository cloned. For example, in a
`knative` working directory, you can run the following commands:

   ```shell
   git clone https://github.com/knative/serving.git
   cd serving
   git checkout v0.1.1
   ```

If you do not have Knative Serving installed and running, see
[Installing Knative](https://github.com/knative/docs/tree/master/install) for
details.

### Viewing what monitoring components are installed

To determine which monitoring components are already installed, you can run the
following command:

  ```shell
  kubectl get pods --namespace monitoring
  ```

## Choose how to monitor Knative Serving

There are two configurations that you can choose to use for monitoring Knative
Serving:

* [Elasticsearch & Kibana](#elasticsearch-kibana-prometheus--grafana-setup)
* [Stackdriver](#stackdriver-prometheus--grafana-setup)

Both configuration types utilize Prometheus and Grafana for monitoring.

Note: Simultaneously monitoring Knative Serving with both Elasticsearch
and Stackdriver is unsupported. Only a single monitoring configuration can be
configured and run per Knative Serving installation.


### Monitoring with Elasticsearch, Kibana, Prometheus, & Grafana

You can use [Elasticsearch](https://www.elastic.co/products/elasticsearch),
[Kibana](https://www.elastic.co/products/kibana),
[Prometheus](https://prometheus.io/) & [Grafana](https://grafana.com/) in
combination to monitor your installation of Knative Serving.

This monitoring configuration gets installed by default with the Knative Serving
component. To see if you have th

After you install the monitoring components, you need to
[create Elasticsearch Indices](#create-elasticsearch-indices) before you can
start monitoring.

#### Installing the Elasticsearch and Kibana monitoring components

If you previously uninstalled the monitoring components, use the following steps
to install the Elasticsearch, Kibana, Prometheus, and Grafana monitoring
components:

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

   The installation is complete when logging & monitoring components are all
   reported `Running` or `Completed`:

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

  CTRL+C to exit watch.

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


### Monitoring with Stackdriver, Prometheus & Grafana

You can use [Stackdriver](https://cloud.google.com/stackdriver/),
[Prometheus](https://prometheus.io/), and [Grafana](https://grafana.com/) in
combination to monitor your installation of Knative Serving, especially when
your Knative Serving installation runs on Google Cloud Platform (GCP).

Note: The sample Stackdriver configuration file works only with GCP. You must
configure and build your own Fluentd container image if either of the following
are true:

 * Your Knative Serving component is not hosted on a GCP based cluster.
 * You want to send logs to another GCP project.

#### Installing the Elasticsearch and Kibana monitoring components

If you previously [uninstalled the monitoring components](), use the following steps
to install the Stackdriver, Prometheus, and Grafana monitoring
components:

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

   The installation is complete when logging & monitoring components are all
   reported `Running` or `Completed`:

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

  CTRL+C to exit watch.

## Uninstalling all the monitoring components

To uninstall a logging plugin, run:

```shell
kubectl delete -f <the-fluentd-config-for-daemonset> \
    -f third_party/config/monitoring/common/kubernetes/fluentd/fluentd-ds.yaml \
    -f config/monitoring/200-common/100-fluentd.yaml
    -f config/monitoring/200-common/100-istio.yaml
```

---

Except as otherwise noted, the content of this page is licensed under the
[Creative Commons Attribution 4.0 License](https://creativecommons.org/licenses/by/4.0/),
and code samples are licensed under the
[Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).
