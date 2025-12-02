# Optimize Source Persistent Queues


Cribl Stream's Source persistent queues can supplement in-memory queues to prevent data loss during a backpressure event, such as when a configured Source sends a higher volume of data than Cribl Stream can process. See [Supported Sources](/persistent-queues#supported-sources) for more information about which Sources support persistent queueing.

Read this guide to learn how to optimize your Source persistent queue. This guide is helpful when you are in the initial planning and configuration stage and also when you have an active deployment that you want to performance tune. This guide explains:

- How Source persistent queues respond to a backpressure event.
- The overall goals and key metrics involved in optimizing Source persistent queues.
- An in-depth discussion of the Source persistent queue configuration options and their potential impact on your system.
- The available notifications, monitoring dashboards, and troubleshooting tools for Source persistent queues.

For a general overview of persistent queues, see [About Persistent Queues](/persistent-queues).

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers with an [Enterprise plan](cloud-enterprise), you can enable persistent queue for supported Sources. However, be aware that mode options and storage limits have specific restrictions in Cribl.Cloud. See [For Cribl.Cloud Only: `Always On` is the Only Available Mode](#spq-cloud) for more information.
>
{.box .cloud}

## How Source Persistent Queues Work {#how-does-spq-work}

Backpressure occurs when there is a temporary imbalance between the rates of inbound and outbound data that is either coming in or going out of Cribl Stream.

Different events trigger persistent queues on Sources:

- Error conditions.
- Degraded performance—such as slowdowns or short backups—in downstream Cribl resources.
- External receivers might trigger backpressure.

When Source persistent queue engages, it writes and stores data on a local file system in your environment until the backpressure event resolves.

### How Sources Handle Backpressure {#source-triggers}

Source persistent queue can serve as a stopgap for upstream senders that don’t provide their own buffering, or whose small buffers cannot queue for long. This applies to several syslog senders and HTTP-based Sources.

Sources that have persistent queue support have the option to supplement in-memory buffering with persistent queue. When you enable persistent queue, you can choose between these Source persistent queue trigger conditions:

| Condition | Description |
| --------- | ----------- |
| **Always On** | Uses persistent queue as a disk buffer for all events. |
| **Smart** | Only engages persistent queue upon backpressure from downstream receivers or Cribl internal processes. |

In `Always On` mode, the persistent queue constantly writes all data to a backup disk buffer regardless of whether there is a backpressure event or normal conditions. After the backpressure event ends, the persistent queue flushes every 10 seconds when it drains in `Always On` mode.

In `Smart` mode, persistent queue engages under any of these conditions:

- A connected Destination is fully down and Destination-side persistent queue is not enabled.
- A connected Destination is slow or blocking.
- A downstream Cribl Pipeline or Route is slow.

When persistent queue engages on a Source in `Smart` mode:

- Events queue *before* any pre-processing Pipeline takes effect.
- Metrics (data in) will show `0` bytes/events while the system buffers events to the disk queue.
- Cribl Stream Pipelines and Routes process the data with a delay as the persistent queue drains.

`Smart` mode only engages when necessary, such as when a downstream Destination becomes blocked *and* the **Buffer size limit** reaches its limit. When persistent queue is set to `Smart` mode, Cribl attempts to flush the queue when every new event arrives. The only time events stay in the buffer is when a downstream Destination becomes blocked.

> Enabling persistent queue on a Source is redundant if the upstream sender already safeguards events in its own disk buffer. Enabling persistent queue on both the sender and Cribl Stream could cause data latency. Check the settings for the sender to ensure you know how whether that specific sender already safeguards against data loss.
>
{.box .info}

> If you turn off Source persistent queue (or shut down the whole Source) before its queues drain, you will orphan any events still queued on disk.
>
{.box .warning}

## Configure Source Persistent Queue

You configure Source persistent queue behavior when you configure each individual Source. Consult the documentation for that Source for more information about initial configuration.

This section provides a configuration quick start for experienced users. However, new Cribl Stream users should consider reading the full guidance in this topic to get a deeper understanding of the decisions and ideal system architecture design for persistent queue.

To configure persistent queue on a Source:

1. In the **Persistent Queue Settings** tab, toggle **Enable Persistent Queue** off. See [Whether to Enable Persistent Queues for Backpressure Events](#enable-pq) for guidance about making the decision to enable persistent queue.

1. For On-Prem only: Select an appropriate **Mode** from the available options. See [When and How to Store Data to Disk During a Backpressure Event](#backpressure-mode) for information about which mode to select.

   > On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers with an [Enterprise plan](cloud-enterprise), you can enable persistent queues for supported Sources. By default, Source persistent queue is not enabled, so you must explicitly decide to turn it on.
   >
   > When you enable Source persistent queue in Cribl.Cloud, the only available mode is `Always On`, which writes data to disk at all times. `Smart` mode is not available. See [For Cribl.Cloud Only: `Always On` is the Only Available Mode](#spq-cloud) for more information.
   >
   {.box .info}

1. For On-Prem only: Set the **Buffer size limit** to determine the maximum number of events to hold in memory before reporting backpressure to the sender. See [When to Report Backpressure to the Sender](#report-to-sender) for more information.

1. For On-Prem only: Set the **Commit frequency** to determine how many events to send downstream before committing that Cribl Stream has read them. See [How Many Events to Commit During a Backpressure Event](#how-many-events) for more information.

1. For On-Prem only: Set the **File size limit** to specify the maximum amount of data stored in each queue file before the file closes and starts a new file. Set the **Queue size limit** to determine the maximum amount of disk space that Cribl allows the queue to consume on each Worker Process. See [How Much Data to Store in Memory](#max-file-size) for more information about these settings.

1. Set the **Queue file path** to determine the location for the persistent queue files. See [Where to Store Data and How Much Disk Space to Allocate](#queue-file-path) for more information.

1. Optional: Set the **Compression** type to apply compression to the data before the queue file closes. See [Whether to Compress Data](#compression) for more information.

## Overview of Source Persistent Queue Optimization

[Snippet not found: content/shared/4.11/snippets/_persistent-queue-optimization.md]

## Key Decisions When Configuring Source Persistent Queues

You configure Source persistent queue behavior when you configure each individual Source. Consult the documentation for that Source for more information about initial configuration.

This section explains the decision-making process you should consider when implementing persistent queue on a Source. It explains the benefits and tradeoffs of different configuration settings for most use cases.

### Whether to Enable Persistent Queues for Backpressure Events {#enable-pq}

The first decision you need to make is whether to use persistent queue to respond to backpressure. See [When to Use Persistent Queues](/persistent-queues#when-to-use-pq) for a detailed explanation of which kinds of environments benefits from persistent queues.

In general, you should not use persistent queue is you are already using a backpressure mechanism on a upstream sender. Implementing backpressure mechanisms such as persistent queue on both Cribl Stream and the Source is redundant and could cause unnecessary data latency or impact data throughput.

The alternatives to using persistent queue are to block data or to drop events when a backpressure event occurs. Consider blocking events if you need Cribl to return block signals back to an upstream sender. If the sender also supports backpressure response mechanisms, the sender may stop sending data.

Consult the documentation for the sender to ensure you know how that specific sender handles a backpressure signal. In general, TCP-based senders support backpressure and stop sending data once Cribl Stream stops sending TCP acknowledgments back to it.

### When and How to Store Data to Disk During a Backpressure Event {#backpressure-mode}

Once the backpressure queue behavior engages, you need to determine how it should write data to disk. Source persistent queues have two available options (modes) for storing data during a backpressure event:

| Mode | Description |
| ---- | ----------- |
| **Always On** | Uses persistent queue as a disk buffer for all events. |
| **Smart** | Only engages persistent queue upon backpressure from downstream receivers or Cribl internal processes. |

The default is **Always On** mode. This mode is helpful if your business has a low tolerance for data loss. However, using **Always On** this mode could have impacts on the [persistent queue optimization metrics](#metrics), such as disk space, throughput, data latency, and CPU utilization.

Considerations for using **Always On** mode:

- This mode can increase memory demands compared to Sources without persistent queue enabled. If memory is a constraint, you can experiment with tuning the **Buffer size limit** setting. However, smaller buffers will reduce the efficiency of disk writes, adding more latency.
- **Always On** mode has higher disk requirements than **Smart** mode. Cribl generally recommends that you allocate each Worker Node enough disk space for at least 1 day’s worth of queued throughput. For example: If one Source is sending 1 TB/day to two evenly distributed Worker Processes, you should provision at least 500 GB queue storage for each Worker Process. Expand this basic calculation for additional Sources. From there, expand or contract it based on the typical length of outages you experience.
- Cribl also recommends that you deploy the highest-performance storage available. This prevents IOPS (input/output operations per second) and throughput bottlenecks for both read and write operations.

Considerations for using **Smart** mode:

- `Smart` mode only engages when necessary, such as when a downstream Destination becomes blocked *and* the **Buffer size limit** reaches its limit. When persistent queue is set to `Smart` mode, Cribl attempts to flush the queue when every new event arrives. The only time events stay in the buffer is when a downstream Destination becomes blocked.
- If you're using **Smart** mode and observe that it causes Source persistent queue to engage frequently, on option is to reduce the **File size limit** setting. Source persistent queue excessively triggers backpressure only when it reaches the last queued file *and* that file is large. Keeping this threshold low prevents that condition.

Consider experimenting with these modes to determine what is appropriate for your needs. Establish a baseline for system performance before you enable **Always On** and then observe the impacts when you turn that mode on.

#### For Cribl.Cloud Only: `Always On` is the Only Available Mode {#spq-cloud}

On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers with an [Enterprise plan](cloud-enterprise), you can enable persistent queues for supported Sources. By default, Source persistent queue is not enabled, so you must explicitly decide to turn it on.

When you enable Source persistent queue in Cribl.Cloud, the only available mode is `Always On`, which writes data to disk at all times. `Smart` mode is not available.

When you enable Source persistent queue in Cribl.Cloud, Cribl automatically allocates up to 1 GB of disk space per Source, per Worker Process. You cannot change this limit. This 1 GB limit applies to outbound uncompressed data, and the queue does not perform any compression. If the queue fills up, Cribl Stream blocks data flow, including outbound data.

### When to Report Backpressure to the Sender {#report-to-sender}

You can use the **Buffer size limit** setting to determine the maximum number of events to hold in memory before reporting backpressure to the sender. Once the number of events reaches this limit, Cribl Stream begins writing the queue to disk. This setting defaults to `1000`.

This buffer is for all connections, not just per Worker Process. For that reason, this can dramatically expand memory usage. Connections share this limit, which may result in slightly lower throughput for higher number of connections. For higher numbers of connections, consider increasing the limit.

### How Many Events to Commit During a Backpressure Event {#how-many-events}

Use the **Commit frequency** setting to determine how many events to send downstream before committing that Cribl Stream has read them. The defaults is `42`.

### How Much Data to Store in Memory {#max-file-size}

[Snippet not found: content/shared/4.11/snippets/_persistent-queue-common-settings.md]

## Source Persistent Queue Notifications {#notifications}

Some [Cribl licenses](https://cribl.io/pricing/) allow you to configure Notifications for Source persistent queues for on-prem deployments. In general, Cribl recommends setting Notifications for these events when using persistent queue to ensure you are aware of important system events. These Notifications always appear in Cribl Stream's user interface and internal logs. You can also send them to external systems.

You can set Notifications for these Source persistent queue scenarios:

- Persistent queue files exceed a configurable percentage of allocated storage.
- A Source reaches the queue-full state, meaning it is running out of available disk space.
- Persistent Queue Usage (saturation).

See [About Notifications](notifications) for information about adding Notifications for Sources.

## Monitor Source Persistent Queues {#monitoring}

Cribl Stream provides various Monitoring dashboards to gather information about your persistent queues. You can access these dashboards from Cribl Stream's sidebar by selecting **Monitoring**.

To view data about Source persistent queues:

1. Open the Monitoring page. From Cribl Stream's sidebar, select **Monitoring** to access dashboards displaying data for all Worker Groups and all nodes.
1. Navigate to **System** > **Queues (Sources)** and view relevant data.

Other Monitoring resources may become unreliable when persistent queue engages during a backpressure event. These include the main Monitoring dashboards for Sources and Destinations. When persistent queue engages, these dashboards will not accurately represent queued data because Cribl Stream queues data to disk rather than arriving into Pipelines or affected Destinations.

The affected Sources will not show EPS and bytes ingested while their data is queueing.

Some other Monitoring considerations:
- Activity in the **System** > **Queues (Sources)** dashboards sometimes shows up only when queues drain.
- With or without persistent queues enabled, Monitoring dashboards do not show usage of in-memory buffers for the Destination.
- Cribl Stream currently does not provide a Monitoring dashboard for the disk volume of persistent queues.
- While events go to a Source persistent queue, Cribl Stream does not record metrics about that Source's inbound data (events/second or bytes).

For these reasons, consider enabling persistent queue notifications.

After the unhealthy condition resolves, and disk queues start to drain, dashboards resume accurately representing delivery rates and volumes.

## Reference: Source Persistent Queue Fields

[Snippet not found: content/shared/4.11/snippets/_sources-persistent-queue-settings.md]