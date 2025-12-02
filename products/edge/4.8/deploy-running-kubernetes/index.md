# Deploying via Kubernetes


This page outlines how to deploy Cribl Edge in Kubernetes Pods to collect logs and metrics. 

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

## Deploying

To deploy the Cribl Edge agent in your Kubernetes cluster, you can use our [Helm charts](https://github.com/criblio/helm-charts). These charts provide the minimum configuration required to install Cribl Edge correctly. 

You can customize our Helm charts with additional configurations required for your Kubernetes platform – such as annotations for load balancers, environment variables for secrets, etc. For details, follow the link above.

A simple deployment example using Cribl's Helm chart:

```shell
helm repo add cribl https://criblio.github.io/helm-charts/

helm install -n cribl-edge --set "cribl.leader=tcp://<token>@leader:4200" cribl-edge cribl/edge
```

You can also use the UI to concatenate and copy/paste the [bootstrap script](managing-edge-nodes#add-node) for adding a Kubernetes node.  

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
4. Install your updated values to Helm, using this command: `helm install -f /bar/values.yaml`.
   
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

By default, Cribl Edge's API listens on port `9420`, instead of on Cribl Stream's default `9000` port. This requires special care when you configure your Edge Fleet in the Cribl UI. 

By default, the API Server is configured to listen on the loopback interface (`127.0.0.1`).
Change the listening interface address to `0.0.0.0` in **Fleet Settings** > **System** > **General Settings** > **API Server Settings** > **General**.
This will bind to all IPs in the container. If you do not make this change, your health checks will start failing after the Edge agent downloads its initial config bundle.

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