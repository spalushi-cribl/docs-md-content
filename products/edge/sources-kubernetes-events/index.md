# Kubernetes Events Source

 
The Kubernetes Events Source relies on the Kubernetes API watch capabilities to collect cluster-level events. These events are generated automatically in response to state changes or errors with nodes, pods, and containers. The Kubernetes API server acts as a notification server for the events we are watching in a given Kubernetes cluster. This Source acts in a listener/subscriber capacity to collect events for all namespaces from the cluster API. For a sample of the events captured, see [Live Data](#livedata). 

To collect cluster-level events, the Kubernetes Events Source needs watch access to the Kubernetes API. For details, see [RBAC for Kubernetes Events Source](/edge/deploy-running-kubernetes#events).

> Type: **System and Internal**  |  TLS Support: **N/A** | Event Breaker Support: **No**  {{< id `restrictions` >}}
> 
> You can't configure custom Kubernetes Events Sources.
>
{.box .info}

> On Cribl.Cloud, this Source is available on customer-managed [hybrid Workers](cloud-enterprise#hybrid), but Cribl-managed Workers in Cribl.Cloud omit this Source – Cribl manages these Workers' uptime and diagnostics on your behalf.
>
{.box .cloud}

## Discover and Filter Kubernetes Events {#discover}

The Kubernetes Events Source connects to the Kubernetes API to set up a watch, and to request a starting point from the latest event at 5-minute polling intervals. The Source then runs the lists through the **Filter Rules** to determine what to report on. 

This Source emits events in a way that might include some redundancies in the properties extracted. For details on the output, plus guidelines on what fields to drop, see [Event Structure Details](#events).

If no rules are configured, or if all of the rules evaluate to `true`, this Source generates events for the object. Conversely, if any of the rules evaluates to `false`, this Source skips the object, and does not generate any events for it.

## Cluster/Daemonset Best Practices

For the Kubernetes Events Source to work as designed, Cribl recommends deploying Cribl Edge as a Daemonset. For details, see [Deploying via Kubernetes](deploy-running-kubernetes).

This Source can generate redundant events when it is running inside the cluster, and can't determine which DaemonSet it's in or which Node it's on. This happens when you run Cribl Edge inside the cluster, but in a way that it can't detect the local Pod/node. In these cases, Edge will emit an error when it initializes this Source. 

## Configure Cribl Edge to Collect Kubernetes Events 

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
      - To configure via [QuickConnect](quickconnect), navigate to **Collect**. Select **Add Source** and select the Source you want from the list, then choose **Select Existing**. 
     - To configure via the [Routes](routes), select **More** > **Sources**. Select the Source you want.
1. Select the default `in_kube_events` to open the configuration modal.
1. Configure the following under **General Settings**: 
    - **Input ID**: This is prefilled with the default value `in_kube_events`, which cannot be changed via the UI, due to the [single‑Source restrictions](#restrictions) above.
    - **Description**: Optionally, enter a description.
1. Next, you can configure the following **Optional Settings**:
   - **Filter Rules**: Optionally, specify the Kubernetes objects which this Source should parse to generate events. If you specify no restrictive rules here, the Source will emit all events.
   - **Filter expression**: JavaScript expression to filter Event objects. For details, see [Filter Expression](#filters) below.
   - **Description**: Optional description of the rule.
   - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
1. Optionally, configure any [Processing](#processing) and [Advanced](#advanced-tab) settings, or [Connected Destinations](#connected) outlined in the sections below.
1. Select **Save**, then **Commit & Deploy**. 
   
### Filter Expression {#filters}

Filters are based on the [Event Objects](https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/event-v1/), and evaluated from top to bottom. The first expression evaluating to `false` excludes the Pod from collection. The default filter is: `!metadata.namespace.startsWith('kube-')`, which ignores Pods in the `kube-*` namespace.

Other **Filter expressions** examples include:
 - Ignores events in the `kube-*` namespaces – `!object.metadata.namespace.startWith(‘kube-’)` 
 - Ignore all events from DaemonSet – ` object.regarding.kind != ‘DaemonSet’`
 - Collects events matching a specific object name - `object.regarding.name == ‘object name’`

### Processing Settings {#processing}

#### Fields 

In this section, you can define new fields or modify existing ones using JavaScript expressions, similar to the [Eval](eval-function) function. 

* The **Field Name** can either be a new field (unique within the event) or an existing field name to modify its value.
* The **Value** is a JavaScript expression (enclosed in quotes or backticks) to compute the field's value (can be a constant). Select this field's advanced mode icon (far right) if you'd like to open a modal where you can work with sample data and iterate on results.

This flexibility means you can:

* Add new fields to enrich the event.
* Modify existing fields by overwriting their values.
* Compute logic or transformations using JavaScript expressions.

### Advanced Settings {#advanced-tab}

**Environment**: If you're using [GitOps](/stream/GitOps), optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Event Structure Details {#events} 

This Source emits a `_raw` property as a JSON-encoded object, as other Sources do. However, there are additional `object` and `type` properties from that extraction. The logic behind this is that `timestamp` and other fields had to be extracted to keep track of starting points.   

If you are sending the events over a metered connection, then you will get three copies of the same data: `_raw`, `__raw`, and the extracted `object` fields. Consider dropping the redundant fields before sending them to your Destination(s). 

![Sample Event](source-k8-events-1.6ebd056ac6.png)
{border="true"}


## Source Operational State {#state} 

To check the Source's operational state, go to **Status** and expand the host details. The **Operational State** Column shows either an **Active** state or **Standby**. **Active** indicates the Source is running or won the election. **Standby** means it's waiting to be re-elected and not currently running. 

![Operational State](ed-k8event-operationstate.b96c8b92c5.png)
{border="true"}

## Source Live Data {#livedata}

To see a sample of the Source's data, select the **Live Data** tab.

![Sample Event](ed-source-k8events-sample.741fd74648.png)
{border="true"}