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

To install the sematic-server chart, you'll need to first configure your
deployment by filling out a copy of your own `valuse.yml` as described in the
Configuration section below, and then run:

```bash
helm install sematic-server sematic-ai/sematic-server -f /path/to/values.yml
```

If you have already used Helm to install Sematic in your cluster, you can
upgrade your installation to the latest version by running:

```bash
helm repo update
helm upgrade sematic-server sematic-ai/sematic-server -f /path/to/values.yml
```

To uninstall the chart:

```bash
helm delete sematic-server
```

## Configuration

### Minimally required

Most configuration options for the Sematic Helm deployment provide reasonable
defaults that you can as is, with a few exceptions.  The following YAML snippet
provides a minimally required settings configuration block for you to fill out
and use:

```YAML
database:
  url: <fill in>
aws:
  storage_bucket: <fill in>
  bucket_region: <fill in>
auth:
  google_oauth_client_id: <fill in>
  authorized_email_domain: <fill in>
```

More information about these configurations can be found in the [Deploy Sematic guide](https://docs.sematic.dev/cloud-execution/deploy).

### Available options

| Parameter                                 | Description                                                   | Default                                                 |
|-------------------------------------------|---------------------------------------------------------------|---------------------------------------------------------|
| `auth.enabled`                                            | Activates Google OAuth authentication                         | `true`                                  |
| `auth.google_oauth_client_id`                             | Google OAuth client id                                        | `xxxxxxxx.apps.googleusercontent.com`   |
| `auth.authorized_email_domain`                            | Email domains authorized to login via Google                  | `example.org`                           |
| `aws.enabled`                                             | Enables cloud storage of artifacts in AWS S3                  | `true`                                  |
| `aws.storage_bucket`                                      | AWS S3 bucket name for cloud artifacts                        | `my-s3-bucket`                          |
| `aws.bucket_name`                                      | AWS region for the S3 bucket                         | `my-s3-region`                          |
| `cleaner.enabled`                                            | Activates a cron job that cleans up stray resources and metadata.                         | `true`                                  |
| `cleaner.crontab`                                            | Cron notation for how often cleaning should be done. Allowed values described in Kubernetes [docs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)                        | `"0-55/5 * * * *"`                                  |
| `cleaner.ttlSecondsAfterFinished`                                            | Time each cleaner pod should live after it has completed, in seconds.                         | `300`                                  |
| `secret.create`                                           | Creates a K8S secret for Postgres URL                         | `true`                                  |
| `database.url`                                            | Fully qualified Postgres database URL                         | `postgres://postgresql:5432/postgres`   |
| `deployment.socket_io.dedicated`                          | Starts a separate dedicated Sematic server for Socket I/O     | `false`                                 |
| `deployment.worker_count`                                 | Number of WSGI workers per Sematic server pod (API only) `deployment.socket_io.dedicated` must be set to true if this is greater than 1 | `1` |
| `deployment.replica_count`                                | Number of replica pods for Sematic server                     | `1`                                     |
| `deployment.affinity`                                     | K8S node affinity for Sematic server pods                     | `{}`                                    |
| `deployment.annotations`                                  | K8S annotations for Sematic server pods                       | `{}`                                    |
| `deployment.tolerations`                                  | K8S tolerations for Sematic server pods                       | `{}`                                    |
| `deployment.node_selector`                                | K8S node selectors for Sematic server pods                    | `{}`                                    |
| `deployment.resources.limits.cpu`                         | K8S CPU limit for Sematic server pods                         | `500m`                                  |
| `deployment.resources.limits.memory`                      | K8S memory limit for Sematic server pods                      | `4000Mi`                                |
| `deployment.resources.requests.cpu`                       | K8S CPU requests for Sematic server pods                      | `500m`                                  |
| `deployment.resources.requests.memory`                    | K8S memory requests for Sematic server pods                   | `4000Mi`                                |
| `deployment.autoscaling.enabled`                          | Enables horizontal pod autoscaling for Sematic server pods    | `false`                                 |
| `deployment.autoscaling.min_replicas`                     | Minimum number of replicas for autoscaled Sematic server pods | `1`                                     |
| `deployment.autoscaling.max_replicas`                     | Maximum number of replicas for autoscaled Sematic server pods | `1`                                     |
| `deployment.autoscaling.target_cpu_utilization_pct`       | Target CPU utilization for autoscaled Sematic server pods     | `80`                                    |
| `deployment.autoscaling.target_memory_utilization_pct`    | Target memory utilization for autoscaled Sematic server pods  | Not set                                 |
| `deployment.security_context`                             | Pod security context for Sematic server                       | `{}`                                    |
| `deployment.security_context.fs_group`                    | Pod security filesystem group for Sematic server              | Not set                                 |
| `deployment.container_security_context`                   | Container security context for Sematic server pods            | `{}`                                    |
| `deployment.liveness_probe`                               | Liveness probe K8S configuration for Sematic server pods      | See default values.yml                  |
| `deployment.readiness_probe`                              | Readiness probe K8S configuration for Sematic server pods     | See default values.yml                  |
| `image.repository`                                        | Image repository for Sematic server container images          | `sematicai/sematic-server`              |
| `image.tag`                                               | Image tag for Sematic server container images                 | Defaults to version released in chart   |
| `image.pull_policy`                                       | Image pull policy for Sematic server container images         | `IfNotPresent`                          |
| `image.pull_secrets`                                      | Image pull secrets for Sematic server container images        | Not set                                 |
| `ray.enabled`                                             | Enables usage of Ray with Sematic[^1]                         | `false`                                  |
| `ray.supports_gpus`                                       | `true` if your Ray workers can use GPUs.                      | `false`                                  |
| `ray.gpu_node_selector`                                   | The K8s node selector for Ray workers using GPUs              | `{}`                                  |
| `ray.non_gpu_node_selector`                               | The K8s node selector for Ray workers not using GPUs          | `{}`                                  |
| `ray.gpu_tolerations`.                                    | List of K8s tolerations for Ray workers using GPUs            | `[]`                                  |
| `ray.non_gpu_tolerations`.                                | List of K8s tolerations for Ray workers not using GPUs        | `[]`                                  |
| `ray.gpu_resource_request_key`.                           | If GPUs are tracked as a resource by the K8s scheduler, this should be the key used to request a specific number of GPUs. Ex: "nvidia.com/gpu" to structure requests for 2 GPUs as `{"nvidia.com/gpu": 2}`         | `null`                                  |
| `rbac.create`                                             | Enables creation of K8S RBAC resources for Sematic server     | `true`                                  |
| `rbac.manage_ray`                                         | Allows the Sematic server to manage Ray clusters.             | `false`                                  |
| `service_account.create`                                  | Enables createion of K8S service account for Sematic server   | `true`                                  |
| `service_account.annotations`                             | K8S service account annotations for Sematic server            | `{}`                                    |
| `worker.service_account.name`                             | Sematic worker service account name                           | `default`                               |
| `service.create`                                          | Enables creation of K8S service for Sematic server            | `false`                                 |
| `service.type`                                            | K8S service type for Sematic server service                   | `ClusterIP`                             |
| `service.port`                                            | K8S service port for Sematic server service                   | `80`                                    |
| `ingress.create`                                          | Enables creation of K8S ingress for Sematic server            | `false`                                 |
| `ingress.sematic_dashboard_url`                           | Public dashboard URL to K8S ingress for Sematic server        | Not set                                 |
| `ingress.force_ssl`                                       | Forces redirects to HTTPS for Sematic Server even when HTTP is used | `true`                            |
| `ingress.class_name`                                      | K8S ingress class name for Sematic server                     | `""`                                    |
| `ingress.annotations`                                     | K8S ingress annotations for Sematic server                    | `{}`                                    |
| `ingress.hosts`                                           | K8S ingress hosts YAML spec for Sematic server                | See default values.yml                  |
| `ingress.tls`                                             | K8s ingress TLS YAML spec for Sematic server                  | `[]`                                    |

[^1]: This is an "Enterprise Edition" feature. Using this requires that you reach out to Sematic and have a Sematic EE server deployed.
