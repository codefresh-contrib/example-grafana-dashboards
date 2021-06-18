# Example Grafana Dashboard for Codefresh

Example Grafana dashboard for monitoring your Codefresh Hybrid Runner with Prometheus:
* **Codefresh / Build Metrics** - enter a build ID (get this from the URL of a build) to see its Storage, CPU, Memory, and Network stats

I also highly recommend using the **Kubernetes / Compute Resources / Namespace (Pods)** dashboard, which [comes included](https://github.com/monitoring-mixins/docs) in most popular Prometheus+Grafana stacks like [Grafana Cloud](https://grafana.com/products/cloud/) and the [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) Helm chart. This dashboard shows detailed graphs for any Kubernetes namespace that you specify, such as `codefresh` (to monitor the entire Hybrid Runner) or `argocd`.

## About the Storage graphs
* **Volume Allocation** - shows the size of the Codefresh cache volume, as specified in its PVC `capacity` and configured in its [Runtime Environment](https://support.codefresh.io/hc/en-us/articles/360016652900-How-to-Configuring-an-existing-Runtime-Environment-with-GCE-disks) `pvcs.dind.volumeSize`.
* **Volume Stats** - shows *actual* usage and capacity of the Codefresh cache volume's bound PVs. Sizes reflect that of the PV's file system. For example, when using local volumes on cluster nodes, this will show the usage and capacity of the node's *entire* disk.

## Prerequisites

The dashboard includes some metrics that come from the [kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) exporter. This is included by default with most popular Prometheus+Grafana stacks like the [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) Helm chart. **Grafana Cloud**, however, is an exception. If you're a Grafana Cloud user, then you'll need to install both kube-state-metrics and the [grafana-agent](https://grafana.com/docs/grafana-cloud/quickstart/agent_k8s/) into any cluster(s) where you've installed the Hybrid Runner. You can install kube-state-metrics via its [community Helm chart](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-state-metrics). 

After installing kube-state-metrics, you'll need to update your Prometheus configuration to scrape it for metrics. Here is a very simple scrape config that you can use - just replace `monitoring` with the namespace where you've installed kube-state-metrics, and then paste this into your Prometheus configuration under the `scrape_configs` section.
```
- job_name: kube-state-metrics
    honor_labels: true
    honor_timestamps: true
    static_configs:
    - targets: ['kube-state-metrics.monitoring.svc.cluster.local:8080']
```

For Grafana Cloud users, note that each grafana-agent actually pushes a Prometheus configuration to its Prometheus SaaS in the cloud. You can update the Prometheus configuration that it pushes by editing its `configmap/grafana-agent` and then restarting its `deployment/grafana-agent`.

## Testing

* Grafana (Cloud) 7.5.7, kube-state-metrics 2.0.0