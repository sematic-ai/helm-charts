# `sematic-grafana-dashboards` - Helm

## Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

```bash
helm repo add sematic-ai https://sematic-ai.github.io/helm-charts
```

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo
sematic` to see the charts.

To install the `sematic-grafana-dashboards` chart, you'll need to first configure your
deployment by filling out a copy of your own `values.yml` as described in the
Configuration section below, and then run:

```bash
helm install -n <namespace> sematic-grafana-dashboards sematic-ai/sematic-grafana-dashboards -f /path/to/values.yml
```

*NOTE:* your Grafana server deployment must be configured to read dashboard
configurations from the Kubernetes namespace into which you install the Helm
chart above.

If you have already used Helm to install the Sematic Grafana dashboards in your
cluster, you can upgrade your installation to the latest version by running:

```bash
helm repo update
helm upgrade -n <namespace> sematic-grafana-dashboards sematic-ai/sematic-grafana-dashboards -f /path/to/values.yml
```

To uninstall the chart:

```bash
helm delete -n <namespace> sematic-grafana-dashboards
```

## Configuration

All of the configuration options for the Sematic Grafana dashboards are
required to be specified in order for the dashboards to function correctly.


### Available options

| Parameter                                                 | Description                                                           | Default                                 |
|-----------------------------------------------------------|-----------------------------------------------------------------------|-----------------------------------------|
| `sematic_server_dashboard.url`                            | Public dashboard URL to K8S ingress for Sematic server                | `my.sematic`                            |
| `sematic_server_dashboard.ssl`                            | Whether to use HTTPS for Sematic server URLs                          | `true`                                  |
| `sematic_server_dashboard.subdomain_namespace`            | If `true`, the configured namespace will be used as a subdomain for the dashboard URL for Sematic server | `false`                                  |
| `datasources.loki.type`                                   | Datasource type string for Loki as configured in Grafana              | `loki`                                  |
| `datasources.loki.uid`                                    | Datasource uid string for Loki as configured in Grafana               | `XXXXAAAABBBBCCCCC`                     |
| `datasources.prometheus.type`                             | Datasource type string for Prometheus as configured in Grafana        | `prometheus`                            |
| `datasources.prometheus.uid`                              | Datasource uid string for Prometheus as configured in Grafana         | `prometheus`                            |
| `datasources.grafana.type`                                | Datasource type string for Grafana as configured in Grafana           | `grafana`                               |
| `datasources.grafana.uid`                                 | Datasource uid string for Grafana as configured in Grafana            | `-- Grafana --`                         |
| `namespaces.default`                                      | Default namespace to use in Grafana                                   | `foo`                                   |
| `namespaces.available`                                    | List of available namespaces shown in the namespace dropdown in Grafana | `[ "bar", "baz" ]`                    |
