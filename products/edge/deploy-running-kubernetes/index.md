# Deploy Cribl Edge via Kubernetes


Deploy Cribl Edge in Kubernetes Pods to collect logs and metrics. 

---

> Cribl Edge supports both Docker and `containerd` runtimes. 
>
{.box .info}

## Considerations 

Cribl Edge connects to the Kubernetes Cluster API and the `kubelet` API on each node to read logs and metrics. To prevent under- or over-collection of logs and metrics, you must deploy the Cribl Edge agent as a DaemonSet in your Kubernetes cluster. Ensure that Cribl Edge is able to detect that it's running inside a Kubernetes Pod, by setting the `CRIBL_K8S_POD` environment variable.

To run the Kubernetes Sources (Metrics, Logs) inside a cluster, use the default `ServiceAccount`, and mount the connection details in `var/run/secrets/kubernetes.io/serviceaccount`. If you want the Source to look in a different directory, use the `CRIBL_SERVICEACCOUNT_PATH` [environment variable](environment-variables) to set the new path to the `ServiceAccount`.

Optionally, you can run the Kubernetes Sources (Metrics, Logs) outside of a cluster, by specifying connection details in a `kubeconfig` file (like `kubectl`). Either place the file in `$HOME/.kube/config`, or use the `$KUBECONFIG` variable to point to it. 

If you are enabling role-based access control (RBAC) in your Kubernetes cluster, Cribl Edge requires a `ClusterRole` to authorize the Service Account to read logs and metrics. This is a `read-only` role, which does not require `write` permissions. Plan ahead with your Kubernetes admins and security team to authorize the installation of this `ClusterRole`.

If your Kubernetes deployment doesn't use valid certificates, then set the `CRIBL_K8S_TLS_REJECT_UNAUTHORIZED` environment variable to `0` to disable that expectation. When you disable this environment variable, all Kubernetes features (including Metadata, Metrics, Logs, and AppScope metadata) will tolerate invalid TLS certificates (i.e., expired, self-signed, etc.) when connecting to the Kubernetes APIs. If this environment variable is not defined or set to `1` and the certificate validation fails, then all connection attempts to the Kubernetes API will be rejected. For details, see [Which Certificate Does kubelet Use?](https://serverfault.com/questions/1063490/which-certificate-does-kubelet-use/1063821#1063821)

All other Cribl Edge Kubernetes components can run inside a namespace of your choice. This page uses the example namespace `cribl-edge`. 

## Configure a Service Account

You can configure the Leader deployment to use a Kubernetes ServiceAccount for accessing cloud resources. This approach proves especially beneficial when you need to grant IAM roles directly to the Leader pod. A common use case is leveraging AWS KMS as the KMS provider without the need for access key IDs and secrets. 

To configure a ServiceAccount for the Leader chart, you can modify the following options within your `values.yaml` file or by using the `--set` parameters during Helm deployment:

| Value                     | Default |  Description                                                                                                                                                                                                                                                                                      |
| :------------------------ | :------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `serviceAccount.create`   | `false` | If set to `true`, the chart will create a new ServiceAccount specifically for the Leader Pod.                                                                                                                                                                                                    |
| `serviceAccount.name`     | `undefined` | Name of the ServiceAccount to use:<br>• If `create: true` and this value is unset, the name defaults to `<release>-leader`.<br>• If `create: true` and this value is set, the service account will use the provided name.<br>• If `create: false`, this value **must** match the name of an existing ServiceAccount. |
| `serviceAccount.annotations` | `{}`    | A key/value map of annotations to be added to the newly created ServiceAccount.                                                                                                                                                                                                                |

When `serviceAccount.create` is set to `false` and a `name` is provided, the chart assumes you are referencing an existing, pre-configured ServiceAccount. Conversely, if `serviceAccount.create` is `true`, the chart will generate a new ServiceAccount, either with a default name or the name you specify. When creating a service account, you can include annotations to declare the specific IAM roles that the service account should have permission to access.

**Example configuration for creating a ServiceAccount with IAM role annotations:**

```yaml
serviceAccount:
  create: true
  name: "cribl-leader-sa"
  annotations:
    [eks.amazonaws.com/role-arn](https://eks.amazonaws.com/role-arn): "arn:aws:iam::123456789012:role/cribl-leader-role"
```

## Deploying

To deploy the Cribl Edge agent in your Kubernetes cluster, you can use our [Helm charts](https://github.com/criblio/helm-charts). These charts provide the minimum configuration required to install Cribl Edge correctly. 

You can customize our Helm charts with additional configurations required for your Kubernetes platform – such as annotations for load balancers, environment variables for secrets, etc. For details, follow the link above.

A simple deployment example using Cribl's Helm chart:

```shell
helm repo add cribl https://criblio.github.io/helm-charts/

helm install -n cribl-edge --set "cribl.leader=tcp://<token>@leader:4200" cribl-edge cribl/edge
```

You can also use the UI to concatenate and copy/paste the [bootstrap script](edge-nodes-add-update#bootstrap) for adding a Kubernetes node.  

If you are building your own manifests, the DaemonSet pods must be configured with the following environment variables:
* `CRIBL_DIST_LEADER_URL: <changeme>`
* `CRIBL_DIST_MODE: managed-edge`
* `CRIBL_EDGE: true`
* `CRIBL_K8S_POD :<must match the name of the pod>`

## Tolerations

By default, Kubernetes will not schedule a Pod on a node that has taints. You must configure your deployment to allow scheduling on these nodes; otherwise, Cribl Edge will be unable to collect logs and metrics from them.

The defaults in our Helm chart's `values.yaml` file are configured like this:

```yaml
tolerations:
  - operator: Exists
```

## Kubernetes CPU Limit Configuration {#cpu-limit}

The Cribl Edge [Helm chart](https://github.com/criblio/helm-charts/tree/master/helm-chart-sources/edge) ensures that the number of Worker Processes is determined by the CPU resources allocated to the pod, rather than the physical CPU count on the host. 

To achieve this, it sets the `CRIBL_K8S_CPU_LIMIT` environment variable to the value of the `resources.limits.cpu` setting on the pod. Cribl Edge then uses the value as a CPU limit to determine how many Worker Processes to start. 

We recommend maintaining the default configuration of the `CRIBL_K8S_CPU_LIMIT` environment variable, to prevent performance problems like resource contention and pod instability from exceeding CPU limits.

## Updating Helm Charts for OpenShift or Non-Root Environments  {#nonroot}

To make sure your deployments run smoothly on OpenShift or non-root environments, you’ll need to configure the security context through the `values.yaml` file. The `values.yaml` file allows you to override default settings and customize your deployment without altering the core Helm chart templates.

To add or update the `values.yaml` :  

1. Locate or create a local [`values.yaml`](https://github.com/criblio/helm-charts/tree/master/helm-chart-sources/edge#values-to-override) file.
2. Modify the `securityContext` section.
3. Customize the capabilities to match your environment's needs:

```yaml
securityContext:
  capabilities:
    add:
      - CAP_NET_BIND_SERVICE
      - CAP_SYS_PTRACE
      - CAP_DAC_READ_SEARCH    
      - CAP_NET_ADMIN
```
4. Install your updated values to Helm, using this command: `helm install -f /bar/values.yaml`
   
For a description of the capabilities, see [Set Capabilities for Cribl Edge](deploy-runtime-user#cap).

## RBAC

Cribl Edge's Kubernetes Logs and Kubernetes Metrics Sources use the Kubernetes API to read logs and metrics from the cluster. If you have role-based access control (RBAC) implemented, you will need to authorize the Cribl Edge Pod Service Account to read the relevant data.

### Kubernetes Events Source {#events}

To collect cluster-level events, the Kubernetes Events Source needs watch access to the Kubernetes API.

An example RBAC rule is:

```yaml
    - apiGroups:
        - "events.k8s.io"
      resources:
        - events
      verbs: ['watch']
```

### Kubernetes Logs Source {#logs}

The Kubernetes Logs Source requires the ability to get, list, and watch Pods in all namespaces.

An example RBAC rule is:

```yaml
    - apiGroups:
        - ""
      resources:
        - pods
      verbs: ['get', 'list', 'watch']
```

### Kubernetes Metrics Source {#metrics}

The Kubernetes Metrics Source reads metric information from many resources in your cluster. The minimum permissions required are: 

```yaml
    - apiGroups:
        - ""
      resources:
        - configmaps
        - endpoints
        - limitranges
        - namespaces
        - nodes
        - persistentvolumeclaims
        - pods
        - replicationcontrollers
        - secrets
        - services
        - strategicMergePatches
        - nodes/log
        - nodes/metrics
        - nodes/proxy
        - nodes/spec
        - nodes/stats
      verbs: ['get', 'list', 'watch']

    - apiGroups:
        - "apps"
      resources:
        - daemonsets
        - deployments
        - replicasets
        - statefulsets
      verbs: ['get', 'list', 'watch']
    - apiGroups:
        - "batch"
      resources:
        - cronjobs
        - jobs
      verbs: ['get', 'list', 'watch']
    - apiGroups:
        - "autoscaling"
      resources:
        - horizontalpodautoscalers
      verbs: ['get', 'list', 'watch']
    - apiGroups:
        - "policy"
      resources:
        - poddisruptionbudgets
      verbs: ['get', 'list', 'watch']
    - apiGroups:
        - "networking.k8s.io"
      resources:
        - ingresses
        - networkpolicies
      verbs: ['get', 'list', 'watch']
    - apiGroups:
        - "admissionregistration.k8s.io"
      resources:
        - mutatingwebhookconfigurations
        - validatingwebhookconfigurations
      verbs: ['get', 'list', 'watch']
    - apiGroups:
        - "certificates.k8s.io"
      resources:
        - certificatesigningrequests
      verbs: ['get', 'list', 'watch']
    - apiGroups:
        - "storage.k8s.io"
      resources:
        - storageclasses
        - volumeattachments
      verbs: ['get', 'list', 'watch']
```

### Prometheus Edge Scraper {#scraper}

To discover a Kubernetes Node or Pods, the Prometheus Edge Scraper Source must be able to list Pods across all namespaces.

An example RBAC rule is:

```yaml
    - apiGroups:
        - ""
      resources:
        - pods
      verbs: ['list']
```

## Health Checks

An example health-check configuration for Kubernetes is:

``` yaml
readinessProbe: 
    httpGet:
        path: /api/v1/health
        port: 9420
        scheme: HTTP
    initialDelaySeconds: 15
    timeoutSeconds: 1
livenessProbe: 
    httpGet:
        path: /api/v1/health
        port: 9420
        scheme: HTTP
    initialDelaySeconds: 15
    timeoutSeconds: 1
```

> If you have Fleets running Edge Nodes with version 4.8.2 and older,
> you can encounter failing health checks that require specific API server configuration.
> See [Common Challenges on the Edge](edge-common-challenges#failing-health-checks) for more information.
{.box .info}

## Environment Variables

You can use the following environment variables to configure Cribl Edge on Kubernetes:
- `CRIBL_SERVICEACCOUNT_PATH` - by default, set to: `/var/run/secrets/kubernetes.io/serviceaccount`.  
- `CRIBL_K8S_POD` - the name of the Kubernetes Pod in which Cribl Edge is deployed.  
- `KUBECONFIG` - set this for local testing.
- `CRIBL_K8S_TLS_REJECT_UNAUTHORIZED` - set to `0` to disable certification validation when connecting to the Kubernetes APIs. When you disable this environment variable, all Kubernetes features (including Metadata, Metrics, Logs, and AppScope metadata) will tolerate invalid TLS certificates (i.e., expired, self-signed, etc.) when connecting to the Kubernetes APIs. 

See also our list of all Cribl [Environment Variables](environment-variables).

## Upgrading Kubernetes Edge Nodes

For details, see [Upgrading Kubernetes Edge Nodes](upgrading-containers##k8-upgrade).

## Uninstalling

To remove the Cribl Edge agent from your Kubernetes cluster, run the following command:

`helm uninstall -n <namespace> <releasename>`

The `helm uninstall` command automatically deletes all deployed resources associated with the Cribl Edge Helm Chart.