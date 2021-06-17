# salesdemo-grafana-dashboards

Example Grafana dashboards for monitoring a Codefresh Runner + pipelines with Prometheus:
* **Codefresh / Build Metrics** - enter a build number (get this from the URL of a build) to see its Storage, CPU, Memory, and Network stats
* **Codefresh / Pipeline Metrics** - enter a pipeline ID (get this from the pipeline's Settings) to see its stats across multiple builds

Side note, I also highly recommend using the **Kubernetes / Compute Resources / Namespace (Pods)** graph, which comes [pre-installed](https://github.com/monitoring-mixins/docs) in most popular Prometheus+Grafana stacks like [Grafana Cloud](https://grafana.com/products/cloud/) and the [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) Helm chart. This graph provides detailed stats for any Kubernetes namespace, such as `codefresh` (for the Hybrid Runner) or `argocd`.

## About the Storage graphs
* **Volume Alocation** - shows PVCs allocated for the Codefresh cache volume. Size reflects the Capacity specified in the PVC and configured in the [Runtime Environment](https://support.codefresh.io/hc/en-us/articles/360016090559-How-to-Setting-up-default-resources-for-your-Runner-Runtime-Environment).
* **Volume Stats** - shows stats of the bound PVs for the Codefresh cache volume. Sizes reflect that of the PV's actual file system. For local volumes on cluster nodes, this will show the usage and capacity of the node's entire disk.

## Prerequisites


## Notes

Testing:
* Grafana (Cloud) 7.5.7, kube-state-metrics 2.0.0
* Grafana 7.0.3, kube-state-metrics 1.9.7, Prometheus 2.18.1