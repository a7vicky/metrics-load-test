# Metrics-load-test

This document covers the steps to use opensource [avalanche](https://github.com/prometheus-community/avalanche) tool which generate lots of merics for load testing. Avalanche serves a text-based Prometheus metrics endpoint.

Avalanche binary can be run from a local server or it can be run as a container.

## How to run test on K8s/Openshift cluster

### Prequesite

* Must have a Kubernetes/OpenShift cluster
* Deploy dynatrace component on Kubernetes/OpenShift cluster [ref doc](https://www.dynatrace.com/support/help/setup-and-configuration/setup-on-k8s/installation/classic-full-stack#manifest)
    * Note: ActiveGate version 1.217+ running inside the Kubernetes/openshift cluster
* Connect your Kubernetes/OpenShift cluster to Dynatrace [ref doc](https://www.dynatrace.com/support/help/setup-and-configuration/setup-on-container-platforms/kubernetes/enable-kubernetes-api-monitoring/connect-your-k8s-clusters)
* Configure Kubernetes cluster settings [ref doc](https://www.dynatrace.com/support/help/platform-modules/infrastructure-monitoring/container-platform-monitoring/kubernetes-monitoring/monitor-prometheus-metrics#prerequisites)
    * In Dynatrace, go to your Kubernetes cluster settings page and enable
        * Monitor Kubernetes namespaces, services, workloads, and pods
        * Monitor annotated Prometheus exporters


### Create avalanche service

```shell
kubectl create -f https://raw.githubusercontent.com/a7vicky/metrics-load-test/main/manifest.yaml
```

**Note:** Yaml file has conatiner argument and replica count which can be change to generate the count of metrics per pod and also other paramneter describe below which can be changed based on requirement.

### Tweaking configuration to generate load

Container argument  | Details
------------------  | -------------
--metric-count=     | Number of metrics to serve.
--label-count=      | Number of labels per-metric.
--series-count=     | Number of series per-metric.
--metricname-length=| Modify length of metric names.
--labelname-length= | Modify length of label names
--value-interval=30 | Change series values every {interval} seconds.
--series-interval=60| Change series_id label values every {interval} seconds.
--metric-interval=120| Change __name__ label values every {interval} seconds.

### Unknowns
This document does not talk about upper limit of ActiveGate that how much metrics can ActiveGate scrap from avalanche service and successfully forward it to Dynatrace tenant (without dropping). ActiveGate configuration tweaking might be needed.
