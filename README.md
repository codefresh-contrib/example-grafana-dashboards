# Example Grafana Dashboards for Codefresh

Example Grafana dashboards for monitoring your Codefresh Hybrid Runner with Prometheus:
* **Codefresh / Build Metrics** - enter a build ID (get this from the URL of a build) to see its Storage, CPU, Memory, and Network stats
* **Codefresh / Pipeline Metrics** - enter a pipeline ID (get this from the pipeline's Settings) to see its stats across multiple builds

I also highly recommend using the **Kubernetes / Compute Resources / Namespace (Pods)** dashboard, which [comes included](https://github.com/monitoring-mixins/docs) in most popular Prometheus+Grafana stacks like [Grafana Cloud](https://grafana.com/products/cloud/) and the [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) Helm chart. This dashboard shows detailed graphs for any Kubernetes namespace that you specify, such as `codefresh` (to monitor the entire Hybrid Runner) or `argocd`.

## About the Storage graphs
* **Volume Allocation** - shows the size of the Codefresh cache volume, as specified in its PVC `capacity` and configured in its [Runtime Environment](https://support.codefresh.io/hc/en-us/articles/360016652900-How-to-Configuring-an-existing-Runtime-Environment-with-GCE-disks) `pvcs.dind.volumeSize`.
* **Volume Stats** - shows *actual* usage and capacity of the Codefresh cache volume's bound PVs. Sizes reflect that of the PV's file system. For example, when using local volumes on cluster nodes, this will show the usage and capacity of the node's *entire* disk.

## Prerequisites

These dashboards use metrics that come from the [kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) exporter. This is included by default with most popular Prometheus+Grafana stacks like the [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) Helm chart. **Grafana Cloud**, however, is an exception. If you're a Grafana Cloud user, then you'll need to install both kube-state-metrics and the [grafana-agent](https://grafana.com/docs/grafana-cloud/quickstart/agent_k8s/) into any cluster(s) where you've installed the Hybrid Runner. You can install kube-state-metrics via its [community Helm chart](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-state-metrics). 

After installing kube-state-metrics, you'll need to update your Prometheus configuration to scrape it for metrics. This repo includes a very generic [scrape config](https://github.com/codefresh-contrib/salesdemo-grafana-dashboards/blob/main/prometheus/scrape_config-kube-state-metrics-2.0.0.yaml) that you can use - just edit your configuration and paste it to the `scrape_configs` section. For Grafana Cloud users, note that each grafana-agent actually pushes a Prometheus configuration to its Prometheus SaaS in the cloud. You can update the Prometheus config that it pushes by editing your `grafana-agent` ConfigMap and then restarting your `grafana-agent` pod.

## Testing

* Grafana (Cloud) 7.5.7, kube-state-metrics 2.0.0
* Grafana 7.0.3, kube-state-metrics 1.9.7, Prometheus 2.18.1