# Install prometherus

# Helmfile

cd hello-helmfile
helmfile sync

# Manually
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

helm install prometheus prometheus-community/prometheus
OR
helm install prometheus prometheus-community/prometheus --version=25.12.0

# Edit config map prometheus-server adding:

    scrape_configs:
    - job_name: 'kube-state-metrics'
      kubernetes_sd_configs:
        - role: endpoints
          namespaces:
            names:
              - kube-state-metrics
      relabel_configs:
        - source_labels: [__meta_kubernetes_service_label_app_kubernetes_io_instance]
          action: keep
          regex: kube-state-metrics
        - source_labels: [__meta_kubernetes_service_name]
          action: keep
          regex: kube-state-metrics
