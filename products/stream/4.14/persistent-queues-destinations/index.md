# Optimize Destination Persistent Queues


Cribl Stream's Destination persistent queues supplement in-memory queues to prevent data loss during a backpressure event, such as when a downstream Destination receiver is unreachable or slow to respond. See [Supported Destinations](persistent-queues#supported-destinations) for more information about which Destinations support persistent queueing.

Read this guide to learn how to optimize your Destination persistent queue. This guide is helpful when you are in the initial planning and configuration stage and also when you have an active deployment that you want to performance tune. This guide explains:

- How Destination persistent queues respond to a backpressure event.
- The overall goals and key metrics involved in optimizing Destination persistent queues.
- An in-depth discussion of the Destination persistent queue configuration options and their potential impact on your system.
- The available notifications, monitoring dashboards, and troubleshooting tools for persistent queues.

See also:

- [About Persistent Queues](persistent-queues)
- [About Destination Backpressure Triggers](destinations-backpressure-triggers)

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers with an [Enterprise plan](cloud-enterprise), you can enable persistent queue for supported Destinations. However, be aware that mode options and storage limits have specific restrictions in Cribl.Cloud. See [For Cribl.Cloud Only: Whether to Use a Hybrid Group](#dpq-cloud) for more information.
>
{.box .cloud}

## How Destination Persistent Queues Work {#how-does-dpq-work}

Backpressure occurs when there is a temporary imbalance between the rates of inbound and outbound data that is either coming in or going out of Cribl Stream.

Different events trigger persistent queues on Destinations. For Destinations, the triggers depend on what kind of Destination it is, such as whether it is filesystem-based, TCP, UDP, and so on. See [Destination Backpressure Triggers](destinations-backpressure-triggers) for more detailed information.

When persistent queue (PQ) engages, it writes and stores data on a local file system in your environment until the backpressure event resolves.

Destinations handle data rate imbalances using the following order of operations:

1. **In-memory buffers store data under normal conditions.** Under normal conditions, Destinations store data in in-memory buffers before sending the data to a receiver. These in-memory buffers on Destinations help absorb temporary imbalances between inbound and outbound data rates.
1. **If inbound data rates begin to spike, data shifts to storage in all the available buffers for that Destination.** Cribl Stream then attempts to output events at a more appropriate rate for the downstream receiver.

   > The threshold for triggering a backpressure event varies by Destination type. See [Destination Backpressure Triggers](destinations-backpressure-triggers) for more information.
   >
   {.box .info}

1. **A backpressure event triggers when buffers are full.**
1. **Backpressure behavior engages.** During a backpressure event, Cribl Stream responds in one of the following ways, based on the Destination **Backpressure behavior** options you configured on the Destination:

   | Setting | Description |
   | ------- | ----------- |
   | **Block** | All data is blocked until the backpressure event has resolved. If set to Block, the output will refuse to accept new data until the receiver is ready. The system will return block signals back to the sender.<br><br>If the sender also supports backpressure backpressure response mechanisms, the sender may stop sending data. Consult the documentation for the sender to ensure you know how that specific sender handles a backpressure signal. In general, TCP-based senders support backpressure and stop sending data once Cribl Stream stops sending TCP acknowledgments back to it. |
   | **Drop Events** | The Destination will discard all new events until the backpressure event has resolved and the receiver is ready. |
   | **Persistent Queue** | If the Destination supports it, the Destination will engage the persistent queue behavior. |

1. **If enabled, persistent queue engages.** If the persistent queue behavior engages, Cribl Stream responds in one of the following ways, based on its configured backpressure mode:

   | Mode | Description |
   | ---- | ----------- |
   | **Error** | Queues and stores data on a disk when the Destination is unavailable or in an error state. |
   | **Backpressure** | Queues and stores data to a disk when it detects backpressure from the Destination until the backpressure event resolves. |
   | **Always On** | Cribl Stream immediately queues and stores all data on a disk for all events, even when there is no backpressure. |

   > You can configure additional settings to determine:
   >
   > - How much data to store in memory.
   > - What happens to data when the queue reaches its capacity limit.
   >
   {.box .info}

1. **When the backpressure event ends, data starts flowing out of the persistent queue and into the Destination.** Cribl Stream begins to drain persistent queues when the receiver can accept data. In the case of load-balanced receivers, this means when at least one receiver can accept data. By default, after data flow is re-established, the persistent queue flushes every 10 seconds. It uses FIFO (first in, first out) event forwarding, ensuring Cribl Stream sends earlier queued events first when receivers recover. However, in Cribl Stream 4.1 and newer, you can instead prioritize new events by disabling **Strict ordering** on a Destination.

> If you turn off Destination persistent queue (or shut down the whole Destination) before its queues drain, you will orphan any events still queued on disk. See [Steps to Safely Disable and Clear Persistent Queues](#clear-pq) for more information.
>
{.box .warning}

## Configure Destination Persistent Queue

You configure Destination persistent queue behavior when you configure each individual Destination. Consult the documentation for that Destination for more information about initial configuration.

This section provides a configuration quick start for experienced users. However, new Cribl Stream users should consider reading the full guidance in this topic to get a deeper understanding of the decisions and ideal system architecture design for persistent queue.

To configure persistent queue on a Destination:

1. In the **General Settings** tab, set the **Backpressure Behavior** option to **Persistent Queue**. See [Whether to Enable Persistent Queues for Backpressure Events](#enable-pq) for guidance about the decision to enable persistent queue.

1. In the **Persistent Queue Settings** tab, select an appropriate **Mode** from the available options. See [When and How to Store Data to Disk During a Backpressure Event](#backpressure-mode) for information about which mode to select.

1. Set the **File size limit** to specify the maximum amount of data stored in each queue file before the file closes and starts a new file. Set the **Queue size limit** to determine the maximum amount of disk space that Cribl allows the queue to consume on each Worker Process. See [How Much Data to Store in Memory](#max-file-size) for more information about these settings.

1. Set the **Queue file path** to determine the location for the persistent queue files. See [Where to Store Data and How Much Disk Space to Allocate](#queue-file-path) for more information.

1. Optional: Set the **Compression** type to apply compression to the data before the queue file closes. See [Whether to Compress Data](#compression) for more information.

1. Set the **Queue-full behavior** to determine what happens when the persistent queue reaches its **Queue size limit** and runs out of space. See [What Happens to Data when the Queue Reaches Its Configured Limits](#queue-full) for more information.

1. The **Backpressure duration limit** setting displays when the **Mode** is set to `Backpressure`. It controls the duration a Destination must experience backpressure before data is queued to disk. The default waiting time is `30` seconds. Lowering this limit strengthens data loss prevention, while increasing it helps prevent unnecessary queue activation in environments with frequent, short network interruptions.

1. Optional: If needed, turn off **Strict Ordering** to more finely tune the **Drain rate limit (EPS)**. See [How to Drain Persistent Queues When Backpressure Events End](#strict-ordering) for more information.

## Overview of Destination Persistent Queue Optimization

[Snippet not found: content/shared/4.14/snippets/_persistent-queue-optimization.md]

## Key Decisions When Configuring Destination Persistent Queues

You configure Destination persistent queue behavior when you configure each individual Destination. Consult the documentation for that Destination for more information about initial configuration.

This section explains the decision-making process you should consider when implementing persistent queue on a Destination. It explains the benefits and tradeoffs of different configuration settings for most use cases.

### Whether to Enable Persistent Queues for Backpressure Events {#enable-pq}

The first decision you need to make is whether to use persistent queue to respond to backpressure. See [When to Use Persistent Queues](persistent-queues#when-to-use-pq) for a detailed explanation of which kinds of environments benefits from persistent queues.

In general, you should not use persistent queue is you are already using a backpressure mechanism on a downstream receiver. Implementing backpressure mechanisms such as persistent queue on both Cribl Stream and the Destination is redundant and could cause unnecessary data latency or impact data throughput.

The alternatives to using persistent queue are to block data or to drop events when a backpressure event occurs. Consider blocking events if you need Cribl to return block signals back to an upstream sender. If the sender also supports backpressure backpressure response mechanisms, the sender may stop sending data.

Consult the documentation for the sender to ensure you know how that specific sender handles a backpressure signal. In general, TCP-based senders support backpressure and stop sending data once Cribl Stream stops sending TCP acknowledgments back to it.

### For Cribl.Cloud Only: Whether to Use a Hybrid Group {#dpq-cloud}

On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](cloud-enterprise)), persistent queue does not come with many of the configuration options described in the rest of this page. For Cribl.Cloud, enabling persistent queue only exposes the destructive **Clear Persistent Queue** button. Use this button with caution to avoid data loss. See [Steps to Safely Disable and Clear Persistent Queues](#clear-pq) for more information.

When you enable persistent queues in Cribl.Cloud, Cribl automatically allocates up to 1 GB of disk space per Destination, per Worker Process. This 1 GB limit applies to outbound uncompressed data, and the queue does not perform any compression. If the queue fills up, Cribl Stream blocks data flow.

You cannot change this limit. If the queue fills up, Cribl Stream/Edge will block outbound data. Use a [hybrid Group](cloud-enterprise#hybrid) if you need to configure:

- Queue size
- Compression
- Backpressure modes
- Other persistent queue settings

### When and How to Store Data to Disk During a Backpressure Event {#backpressure-mode}

Once the backpressure queue behavior engages, you need to determine how it should write data to disk. You have three available options (modes) for storing data during a backpressure event:

| Mode | Description |
| ---- | ----------- |
| **Error** | Queues and stores data on a disk when the Destination is unavailable or in an error state. |
| **Backpressure** | Queues and stores data to a disk when it detects backpressure from the Destination until the backpressure event resolves. |
| **Always On** | Cribl Stream immediately queues and stores all data on a disk for all events, even when there is no backpressure. |

Consider using the **Always On** mode if your business has a low tolerance for data loss. However, do so with some caution. Enabling this mode could have impacts on the [persistent queue optimization metrics](#metrics), such as disk space, throughput, data latency, and CPU utilization.

Consider experimenting with this mode to see whether it's right for your business needs. Establish a baseline for system performance before you enable **Always On** and then observe the impacts when you turn that mode on. Ensure you have enough disk storage space to handle your data storage needs.

### How Much Data to Store in Memory {#max-file-size}

[Snippet not found: content/shared/4.14/snippets/_persistent-queue-common-settings.md]

### What Happens to Data when the Queue Reaches Its Configured Limits {#queue-full}

You can use the **Queue-Full Behavior** setting to determine what happens when the persistent queue reaches its **Queue size limit** and runs out of space. There are two options:

| Setting | Description |
| ------- | ----------- |
| **Block** | Blocks all data until the backpressure event has resolved. If set to Block, the output will refuse to accept new data until the receiver is ready. The system will return block signals back to the sender. |
| **Drop new data** | The Destination will discard all new events until the backpressure event has resolved and the receiver is ready. |

### How to Drain Persistent Queues When Backpressure Events End {#strict-ordering}

When a backpressure event ends, the persistent queue begins to drain to the downstream Destination. You can configure the order of event delivery and the rate at which Cribl Stream forwards events.

Use the **Strict Ordering** setting to configure the order that Cribl Stream sends data to the Destination. Toggle **Strict Ordering** on (default) to use FIFO (first in, first out) processing. This means Cribl prioritizes earlier queued events before sending newly arrived data. Toggle **Strict Ordering** off to change this behavior, prioritizing new data over the events already in the queue.

If you toggle **Strict Ordering** off, you must configure the **Drain rate limit (EPS)**. This setting allows you to throttle the rate at which events drain from the persistent queue to the Destination. The default value of `0` disables throttling. Throttling the drain rate helps Cribl manage the incoming stream of live data more effectively, preventing delays caused by queue processing. It also improves the throughput of active connections by reserving additional resources for them.

The Drain rate limit (EPS) is set in Events Per Second (EPS). To determine your system's EPS value:

1. Open the Monitoring page. From Cribl Stream's sidebar, select **Monitoring** to access dashboards displaying data for all Worker Groups and all nodes.
1. Filter by a specific node. Use the **Groups** menu at the top to restrict monitoring data to a single Worker Node.
1. Adjust the time granularity. Use the time menu change the display granularity from the default `15 min` to `1 day` for better accuracy.
1. Locate the EPS metrics. On the Monitoring page, find the **Events In and Out** metrics group. Copy the value listed under THROUGHPUT IN (AVG). For example, a typical value might be `31,000 EPS`.

Cribl recommends setting the **Drain Rate Limit** to 10% of the Worker Process EPS to prioritize live data processing. To calculate an appropriate drain rate:

1. Determine EPS per Worker Process. Divide the Worker Process EPS by the number of Worker Processes (WPs). For example: `31,000 EPS / 14 WPs = 2,200 EPS per WP`.
1. Calculate 10% of EPS per Worker Process. Multiply the result by `0.10` to find the recommended drain rate. For example: `10% of 2,200 EPS = 220 EPS`.
1. Configure the **Drain Rate Limit**. In the persistent queue settings for the Destination, enter this value in the **Drain rate limit (EPS)** field. For example: `220`.

After applying this setting, monitor the queue's drain rate. If the queue is draining too slowly, carefully increase the **Drain rate limit (EPS)** while ensuring it doesn�t interfere with live data processing. Avoid setting the limit too high, as this can impact the performance of live streams. If needed, consider optimizing Worker startup connections and managing CPU load to further enhance performance at **Group Settings** > [Worker Processes](group-settings#processes).

> Cribl's guarantee for data delivery is that all data is delivered at least once, but not exactly once. Sometimes Cribl Stream sends duplicate data events when the persistent queue drains.
>
{.box .info}

## Steps to Safely Disable and Clear Persistent Queues {#clear-pq}

If you want to disable persistent queuing, always wait for the queue to fully drain. The **Clear Persistent Queue** button is available as a destructive fallback. This action frees up disk space, but it also deletes the queued data *without* sending it on to receivers.

After the queue has drained, then you can select a different backpressure behavior. Save the configuration after changing it.

## Destination Persistent Queue Notifications {#notifications}

Some [Cribl licenses](https://cribl.io/pricing/) allow you to configure Notifications for Destination persistent queues for on-prem deployments. In general, Cribl recommends setting Notifications for these events when using persistent queue to ensure you are aware of important system events. These Notifications always appear in Cribl Stream's user interface and internal logs. You can also send them to external systems.

You can set notifications for these scenarios:

- Persistent queue files exceed a configurable percentage of allocated storage.
- A Destination reaches the queue-full state, meaning it is running out of available disk space.
- A Destination backpressure engages, causing blocked or dropped events.
- Persistent Queue Usage (saturation).

See [About Notifications](notifications) for information about adding Notifications for Destinations.

## Monitor Destination Persistent Queues {#monitoring}

Cribl Stream provides metrics and health indicators to monitor your Destination persistent queue through the **Status** tab on your Destination. Cribl Stream provides metrics and health indicators to monitor your Source persistent queue through the **Status** tab on your Source. The **Status** tab aggregates metrics and health from all Worker Processes, allowing you to assess the health of your persistent queues and the status of the Destination in real time.

![Example of the Destination Status tab](persistent-queue-status-destination.png)
{scale="90%"}

To view the Destination persistent queue metrics:

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Data**, select **Destinations**.

1. From the sidebar, select the type of Destination you want to view.

1. Select the Destination from the list of currently configured Destination.

1. In the resulting modal, select the **Status** tab.

Inside this view, you can expand individual Worker Nodes for more details.

For additional analysis, you can also use [Monitoring](monitoring) dashboards:

1. Navigate to **Monitoring** on the product sidebar.

1. Navigate to **System** > **Queues (Sources)** and view relevant data.

## Troubleshooting Tools

Cribl internal logs for Cribl-managed Workers might include entries labeled `Revived PQ Worker Process`. These indicate where a Cribl-managed Worker was newly assigned to drain a persistent queue left over from a shut-down Worker Node. You can use these log events to troubleshoot Cloud PQ issues.

## Reference: Destination Persistent Queue Fields

[Snippet not found: content/shared/4.14/snippets/_destinations-persistent-queue-settings.md]