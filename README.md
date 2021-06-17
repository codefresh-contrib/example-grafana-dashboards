# salesdemo-grafana-dashboards

Example Grafana dashboards for monitoring a Codefresh Runner + pipelines with Prometheus
* Codefresh / Build Metrics - enter a build number (get this from the URL of a build) to see its Storage, CPU, Memory, and Network stats
* Codefresh / Pipeline Metrics - enter a pipeline ID (get this from the pipeline's Settings) to see stats for the pipeline's builds

Side note, I also highly recommend using the **Kubernetes / Compute Resources / Namespace (Pods)** graph, which is part of the [kubernetes-mixin](https://github.com/monitoring-mixins/docs). It comes pre-installed in popular Prometheus+Grafana stacks like [Grafana Cloud](https://grafana.com/products/cloud/) and the [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) Helm chart. The graph provides detailed stats for any Kubernetes namespace, such as `codefresh` (for the Hybrid Runner) or `argocd`.

## Requirements
