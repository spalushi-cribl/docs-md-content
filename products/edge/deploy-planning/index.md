# Deployment Planning


Plan your Cribl Edge deployment, taking into account requirements, deployment types,
scaling, and billing.

---

When you're planning an Edge deployment, consider these key factors: 

* The amount of data (metrics, logs, custom command outputs, and so on) that you
  plan for Edge to collect from the endpoint and ingest per unit of time. For
  example, MB/s or GB/day.
* The amount of processing that will happen on incoming data. For example, are
  there a lot of transformations, regex extractions, parsing functions, field
  obfuscations, and so on?
* Routing and/or cloning: Is most data going to a single Destination, or is it
  being cloned and routed to multiple places? This is important, because
  Destination-specific serialization tends to be relatively expensive.
* Deploying onto servers with no internet access: If you plan to deploy
  Edge Nodes on air-gapped on-prem servers (servers with no internet access),
  you must ensure that every Edge Node can communicate with a Leader that is
  also on-prem (that is, not in Cribl.Cloud).

> See [Requirements](requirements) for detailed information about system and OS requirements for a Cribl Edge deployment.
{.box .info}

## Types of Deployment

- **[Cribl.Cloud](deploy-cloud)**: Quickly launch a Cribl-managed
  deployment of the combined Cribl applications suite (Edge, [Stream](/stream/),
  [Search](/search/), and [Lake](/lake/)). In Cribl.Cloud, Cribl provisions and manages all
  infrastructure on your behalf.

- **Managed Edge**: A central Leader Node controls, manages, and pushes
  configuration updates to all managed Edge Nodes. This deployment type provides
  a centralized view for multiple Edge instances, which is beneficial for
  large-scale deployments of Cribl Edge.

- **Single-instance**: Single-instance Edge Nodes operate without a central
  Leader. You will configure and manage Edge Nodes locally.
  Single-instance Edge is suitable for smaller deployments or environments with
  limited network connectivity.

When you have a large number of Edge Nodes to manage, you can use Fleets to
group Nodes together and apply configurations centrally. Use [Fleet Management](fleet-management) 
to accommodate increased load. See [Add and Update Edge Nodes](edge-nodes-add-update#bootstrap)
to streamline deploying Edge Nodes via scripting.

## Leader Scalability {#scaling}

Our [System Requirements](#requirements) suffice for Leaders handling up to 10,000 Edge Nodes. 
To support more than 10,000 Nodes, see our [Planning Large-Scale Deployments of Cribl Edge](how-to-scale-edge) guide.

For large on-prem deployments handling high traffic, review the recommended OS-level tuning detailed in [OS Tuning for Large Deployments](os-tuning).

## About Billing and Usage

Cribl.Cloud offers a one-page view of your total credits consumed and remaining
credits for the current active contract in place. This is called Cribl FinOps
Center. Here, you can view per-product consumption for the Cribl products you
use. The **Invoices** tab lets you download and export your usage by month. For more
details, go to the [FinOps Center](cloud-billing) docs.

### On-Prem Licenses
If you're deploying on-prem, there are three license options. The [Cribl Plans](https://cribl.io/pricing/edge/?product=software-license) page provides details feature comparisons for each license.  

You can configure [Connected Environments](cloud-connections) to use the
Cribl.Cloud FinOps Center billing model with your on-prem deployments instead of
managing multiple licenses.

## Permissions and Rights

You do **not** need elevated privileges to install Edge. However, you will need:
* Administrator (or equivalent) rights in order to enable Edge to start on boot.
* Sufficient rights to access the resources that need monitoring – files, metrics, and so on.

> ##### Network Requirement (DPI/IPS Exclusion)
> Deep Packet Inspection (DPI) and Intrusion Prevention Systems (IPS) on network devices situated between your Edge Fleets can cause high resource utilization and connection instability in all environments (both Cribl.Cloud and on-prem).
> Explicitly exclude all Cribl data streams from DPI/IPS inspection for stable operations and optimal performance.
{.box .warning}


##  Performance Considerations

 As with most data-collection and -processing applications, Cribl Edge's expected resource utilization will be proportional to the data volume and type of processing. For instance, a Function that adds a static field on an event will perform faster than one that applies a regex to find and replace a string. 

* Processing performance is proportional to CPU clock speed.
* All processing happens in-memory.
* Processing does not require significant disk allocation.

Thinking about your planned Edge deployment as a whole, it's critical to consider **cardinality**: how many Fleets, of what size, will send metrics to a given Leader. In a very low-cardinality deployment, a Leader Node might aggregate metrics from one small Fleet; in a very high-cardinality deployment, a Leader Node might aggregate metrics from many large Fleets.

Too many Edge Nodes sending too many metrics can degrade Leader performance and/or integrity. By carefully choosing what metrics each Fleet sends to its Leader Node, you can ensure that you collect the metrics most important to you, within the limits of the Leader's capacity to process them.

As part of your deployment planning, decide which of the following four sets of metrics each Fleet in your deployment will send to its Leader Node:

* **Minimal**: Limited to metrics for events and bytes in and out. Recommended for high-cardinality deployments.
* **Basic** (the default set): All the metrics required to display all monitoring data available in the Edge UI. Contains all the metrics in the **Minimal** set, and more.
* **All**: All metrics, sent without filtering.
* **Custom**: Only metrics that match a JavaScript expression that you define.

For a listing of the metrics each set provides, see [Control Metrics in Cribl Edge](internal-metrics#edge-metrics-controls).

## How Edge Works with AppScope {#with-appscope}

[AppScope](https://appscope.dev/docs/overview/) is an Open Source utility from Cribl that can send events, metrics, and/or payloads from a process or application to an [AppScope Source](sources-appscope) on Edge.

Here's what to keep in mind when you plan to use Edge and AppScope together:

* AppScope is bundled with Edge and does not need to be installed.
* Running Edge with AppScope requires that Edge either run as root on a Linux host, or in a Docker container that is in privileged mode. (If this is a problem for you, see [a workaround using Appscope CLI](deploy-runtime-user#privileges-appscope)).
* To work with an AppScope host, all Edge nodes in a Fleet should be either all in Docker containers, or, all directly on Linux hosts. Plan to keep a ratio of one AppScope Source to one Fleet.

## Configure Edge Node Count

Enter the estimated number of Edge Nodes in this deployment, which is used to
calculate how many Edge-specific connections service processes to spawn.
Changing this number could restart any currently running service processes. If
left blank, defaults to 0.

To configure the Edge Node Count:
  
1. From **Settings**, select the **Global** tab, then select **General Settings**.
1. In an on-prem deployment, select **Limits**, and then **Others**.
1. Enter the number of Edge Nodes in the **Edge Nodes count** field. 

## Configure Connection Processes {#connection}

To configure the connection processes for your Edge Nodes in an on-prem deployment:

1. From **Settings**, select the **Global** tab, select **Service Processes**, and then select **Services**.
1. Confirm that **Connections listener number of processes** is set to its default `1`. 

> The **Connections listener number of processes** is automatically determined by the **Edge Node Count** setting you enter above. Any value you enter manually will be overridden by this calculation.
{.box .warning}