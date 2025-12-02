# Kubernetes Metrics Source

 
The Kubernetes Metrics Source generates events periodically based on the status and configuration of a [Kubernetes](https://kubernetes.io/docs/home/) cluster and its nodes, pods, and containers. For metrics details, see [Kubernetes Metrics Details](/edge/system-metrics-kubernetes-output).

For a sample of the events this Source emits, see [Live Data](#livedata).
 
You must authorize this Source to read metric information from many resources in your cluster. For details, see [RBAC for Kubernetes Metrics Source](/edge/deploy-running-kubernetes#metrics).

 
> Type: **System and Internal**  |  TLS Support: **N/A** | Event Breaker Support: **No**  {{< id `restrictions` >}}
>
> You can't configure custom Kubernetes Metrics Sources.
>
{.box .info}

> This Source is currently unavailable on Cribl-managed Workers in Cribl.Cloud.
>
{.box .cloud}

## Discover and Filter Kubernetes Objects to Monitor {#discover}

The Kubernetes Metrics Source connects to the Kubernetes API and loads the lists of K8s nodes, Pods, and containers in the cluster, on a configurable **Polling interval**. The Source then runs the lists through the **Filter Rules** to determine what to report on. 

If no rules are configured, or if all of the rules evaluate to `true`, the Source generates metrics for the object. Conversely, if any of the rules evaluates to `false`, the Source skips the object, and does not generate any metrics for it.

## Cluster/Daemonset Best Practices

For the Kubernetes Metrics Source to work as designed, Cribl recommends deploying Cribl Edge as a Daemonset. For details, see [Deploying via Kubernetes](deploy-running-kubernetes).

This Source can generate redundant events when running inside the cluster and it isn't able to determine which DaemonSet it's in or which Node it's on. This happens when you run Cribl Edge inside the cluster, but in a way that it can't detect the local Pod/node. In these cases, Edge will emit an error when it initializes this Source. 

## Configure Cribl Edge to Collect Kubernetes Metrics

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
      - To configure via [QuickConnect](quickconnect), navigate to **Collect**. Select **Add Source** and select the Source you want from the list, then choose **Select Existing**.
     - To configure via the [Routes](routes), select **More** > **Sources**. Select the Source you want.
1. Select the default `in_kube_metrics` to open the configuration modal.
1. Configure the following under **General Settings**: 
   - **Input ID**: This is prefilled with the default value `in_kube_metrics`, which cannot be changed via the UI, due to the [single‑Source restrictions](#restrictions) above.
    - **Description**: Optionally, enter a description.
1. Next, you can configure the following **Optional Settings**:
   - **Polling interval**: How often, in seconds, to collect metrics. If not specified, defaults to `15s`.
   - **Filter Rules**: Optionally, specify the Kubernetes objects which this Source should parse to generate events. If you specify no restrictive rules here, the Source will emit all events.
   - **Filter expression**: JavaScript expression to filter Kubernetes objects. For details, see [Filter Expression](#filters) below.
   - **Description**: Optional description of the rule.
   - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
1. Optionally, configure any [Processing](#processing), [Disk Spooling](#disk), and [Advanced](#advanced-tab) settings, or [Connected Destinations](#connected) outlined in the sections below.
1. Select **Save**, then **Commit & Deploy**. 

#### Filter Expression

 Filters are based on the [Kubernetes Pod Object definition](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/), and evaluated from top to bottom. The first expression evaluating to `false` excludes the Pod from collection. The default filter is `!metadata.namespace.startsWith('kube-')`, which ignores Pods in the kube-* namespace.

Other **Filter expressions** examples include:

- Collect metrics from Pods on a specific Node – `spec.nodeName == 'node1'`

- Ignore all DaemonSets – `metadata.ownerReferences[0].kind != 'DaemonSet'`

- Ignore Pods with specific Container names – `spec.containers[0].name != 'edge'`



### Processing Settings {#processing}

#### Fields 

[Snippet not found: content/shared/4.12/snippets/_sources-add-fields.md]

#### Pre-Processing

Select a **Pipeline** (or Pack) from the drop-down to process this Source's data. Required to configure this Source via **Data Routes**; optional to configure via **Collect**/**QuickConnect**.

#### Disk Spooling {#disk}

**Enable disk persistence**: Whether to save metrics to disk. Toggle on to expose this section's remaining fields.

**Bucket time span**: The amount of time that data is held in each bucket before it’s written to disk. The default is 10 minutes (`10m`).

**Max data size**: Maximum disk space the persistent metrics can consume. Once reached, Cribl Edge will delete older data. Example values: `420 MB`, `4 GB`. Default value: `100 MB`. 

**Max data age**: How long to retain data. Once reached, Cribl Edge will delete older data. Example values: `2h`, `4d`. Default value: `24h` (24 hours).

**Compression**: Optionally compress the data before sending. Defaults to `gzip` compression. Select `none` to send uncompressed data.

**Path location**: Path to write metrics to. Default value is `$CRIBL_HOME/state/kube_metrics`.

### Advanced Settings {#advanced-tab}

**Environment**: If you're using [GitOps](/stream/GitOps), optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Source Operational State {#state} 

To check the Source's operational state, go to **Status** and expand the host details. The **Operational State** Column shows either an **Active** state or **Standby**. **Active** indicates the Source is running or won the election. **Standby** means it's waiting to be re-elected and not currently running. 

![Operational State](ed-k8metric-operationstate.213b42d295.png)
{border="true"}

## Source Live Data {#livedata}

To see a sample of the Source's data, select the **Live Data** tab.

![Live Data](ed-k8-metrics-source-livedata.d1f96c4aea.png)
{border="true"}