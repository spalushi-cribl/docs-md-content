# Planning for Persistent Queues


The Insider's Guide

---

This page delves into:
- Use cases for implementing persistent queues (PQ). 
- Details about PQ behavior on Sources and Destinations, with various configuration and tuning options.
- Choices you should make before enabling PQ.
- Implications of those choices for your system's requirements and performance. 
 
For granular configuration procedures, see [Configuring Persistent Queueing](persistent-queues-configuring).

## Source-Side PQ {#source}

Source-Side PQ can serve as a stopgap for upstream senders that don’t provide their own buffering, or whose small buffers cannot queue for very long. This applies to several syslog senders and HTTP-based Sources. 

Here, PQ prevents data loss when the Source is unable to handle backpressure for an extended period. Backpressure might be triggered by error conditions, or by degraded performance (slowdowns or short backups), in downstream Cribl resources or outside receivers.

### Always On Versus Smart Mode {#spq-mode}

Where senders do not provide their own buffering, and where [Destination‑Side PQ](#destination) is not enabled: It is a good practice to enable Push Sources' PQ option, in either `Smart` or `Always On` mode. But which mode should you choose?

`Always On` mode is a good match for highly valuable data (such as security data), and for data ingested via UDP. (Relevant UDP Sources are Raw UDP, SNMP Trap, Metrics, and Syslog with UDP enabled.)  

`Smart` mode might be sufficient for lower-grade data, such as events that are overwhelmingly `debug`-level.

### Always On Requirements {#always-on}

`Always On` mode provides the most reliable data delivery, but you should plan for its impacts on throughput, memory, and hosts' disk resources.

#### Throughput {#throughput}

Each Source configured in `Always On` mode imposes a significant performance penalty on Workers' data ingestion rate, as compared to Sources without PQ enabled. This primarily involves the overhead to write to the filesystem. As a starting point, assume that each event will be burdened with approximately 1 second of added latency.

#### Memory {#memory}

`Always On` mode can also increase memory demands, as compared to Sources without PQ enabled. If you find memory to be a constraint, you can experiment with tuning the **Max buffer size** in each Source's UI, or the `maxBufferSize` [configuration key](inputsyml). However, smaller buffers will reduce the efficiency of disk writes, adding more latency. 

#### Disk {#disk}

`Always On` also imposes more disk requirements than `Smart` mode. Cribl generally recommends that you allocate each Worker Node enough disk space for at least 1 day’s worth of queued throughput. For example: If one Source is sending 1 TB/day to two Worker Processes (evenly weighted), you should provision at least 500 GB queue storage for each Worker Process. Expand this basic calculation for additional Sources, and expand or contract it based on the typical length of outages you experience.

Cribl also recommends that you deploy the highest-performance storage available. This prevents IOPS (input/output operations per second) and throughput bottlenecks for both read and write operations.

#### Compression {#compression}

Enabling the PQ compression option conserves disk space, with a compression ratio of about 8:1. But you get these savings at the cost of worsening the [performance hit](#throughput). Navigate this trade-off based on which factor is most valuable in your deployment.

### Smart Mode Details {#smart}

`Smart` mode engages only when a Source receives an unhealthy signal from a downstream process (Pipeline, Destination down or blocked, etc.). When engaged, `Smart` mode's latency penalty is about the same as with `Always On` mode. But this latency is highly variable, because `Smart` mode engages intermittently. So expect less latency overall, with less predictability – spikes and valleys.

`Smart` mode's CPU performance penalty is about twice that of `Always On`, because of the overhead to switch queueing on and off. But again, this is highly variable, because performance will degrade only when PQ engages/​disengages. As with `Always On`, if memory is a constraint, you can experiment with reducing **Max buffer size** (UI) or `maxBufferSize` (config-file) values. Compression is supported in `Smart` mode, as in `Always On` modes.

#### Smart Mode Interaction with Multiple Destinations {#interaction}

If you enable `Smart` mode on a Source that's routed to multiple Destinations, you should plan around the unintended interaction that PQ can cause among these Destinations:

If at least one connected Destination sends an unhealthy signal, this Source will queue new events to disk even if some peer Destinations remain up. This will delay data delivery to the peer healthy Destinations. The same interaction can occur between Destinations grouped in a connected [Output Router](destinations-output-router).

To prevent this unintended behavior, you can either:
- Disable PQ on the Source, and instead configure PQ on individual [Destinations](#destination); or
- Configure separate Worker Groups to connect this Source to separate Destinations.

### How Source PQ Works {#how-source}

Once enabled on a specific Source, in `Smart` mode, PQ will engage when any of the following occur:
* A connected Destination is fully down, and Destination-side PQ is not enabled.
* A connected Destination is slow, or blocking.
* A downstream Cribl Pipeline/Route is slow.
 
These triggers are not relevant if you've enabled Source PQ in [`Always On` mode](#always-on). In that case – it's always on.

When PQ engages on a Source:
* Events queue **before** any pre-processing Pipeline takes effect.
* Metrics (data in) will show `0` bytes/events while events are buffered to the disk queue.
* Cribl Stream Pipelines/Routes will process the data with a delay, when PQ later drains.

> Because PQ always carries some latency penalty, it is redundant to enable PQ on a Source whose upstream sender is configured to safeguard events in its own disk buffer.
> 
> If you turn off Source PQ (or the whole Source) before its queues drain, any events still queued on disk will be orphaned.
>
{.box .warning}

## Destination-Side PQ {#destination}

You can enable PQ on supported Destinations to safeguard data during extended errors, and to smooth out CPU load spikes when a Destination recovers from a transient outage. 

Unlike Source-side PQ, Destination-side PQ will engage only when the Destination is **completely** unavailable, not just slow.

Destination PQ is a good match for receivers like Splunk, where there are known outages. Because it has no `Always On` mode, Destination PQ imposes less overall performance overhead than Source PQ.

As with Source PQ, size your disk capacity according to your risk tolerance, but Cribl's initial recommendation is to size for at least 1 day’s worth of storage on each Worker Node.

### Fallback (Queue-Full) Behavior {#fallback}

Unlike Source PQ, on Destinations where you've enabled PQ, you also select a fallback **Queue-full behavior**. 

With the default `Block` option, if a Destination's persistent queue fills up, the Destination will block back to the corresponding Source. (Remember that a Destination can be fed by multiple Sources.) If the Source has its own PQ option enabled, it will begin queueing data. If not, the block signal will go all the way back to connected senders.

If you instead set the **Queue-full behavior** to `Drop new data`, Cribl Stream will simply discard data intended for the unavailable Destination, but will still allow the data to proceed to parallel Destinations.

Good use cases for `Block`: A simple Cribl Stream Route relaying Universal Forwarder data to a Splunk receiver; or, any Destination where you absolutely **must not** lose data.

## How Destination PQ Works {#how-destination}

If a Destination does not have load balancing enabled, PQ will engage upon a blocking or unavailable signal from the downstream receiver. 

But with a load-balanced Destination, PQ will engage only if **all** of the Destination's downstream receivers are down or blocked. If some individual targets in the LB Destination are up, Cribl Stream will attempt to discover those receivers and deliver data to them.

When PQ is enabled on a specific Destination:
* PQ will engage when the Destination is completely unavailable, but not when it is simply slow.
* Files are written to disk **after** any post-processing Pipeline takes effect. (This is the mirror image of Source PQ, which writes to disk **before** pre-processing Pipelines.)
* New incoming data still flows to **other** Destinations that are up.
* When the Destination recovers, the default draining behavior is FIFO (first in, first out) But you can toggle off this **Strict ordering** default– and doing so enables a configurable **Drain rate limit (EPS)**.
* When an [HTTP/S-based](destinations#available-destinations) Destination recovers, Cribl Stream minimizes the transmission of potential duplicate events that were retried on failure. However, some events that were in transit to the Destination might still emerge as duplicates.
* A **Clear Persistent Queue** button is available on each Destination, in case you need to clear out the queue's contents. This is destructive – it recovers the disk space, but abandons the queued data. Treat it as a panic button.

### Destination PQ Better Practices {#better-destination}

* Cribl generally recommends enabling the PQ **Compression** option on Destinations. (Disable compression if you find that it adds excessive latency.)
* When draining large volumes after recovery from an outage, you might find that PQ is crushing its Destination. In this case, disable **Strict ordering**, and tune the **Drain rate limit (EPS)** to maintain reasonable throughput for new data.

## Better Practices for Source and/or Destination PQ {#better-both}

If your Source(s) and Destination(s) both support PQ, which side should you enable? 

Enabling Source-side PQ (in `Always On` mode) is the most versatile and reliable choice. Source PQ is further upstream than Destination PQ, and can safeguard data bound for multiple Destinations.

However, you might opt for Destination-side PQ if your senders provide their own buffering, or if you don't want to add the system resources that Source Always On mode requires.

Enabling **both** Source- and Destination-side PQ on a Route is likely overkill. It can also complicate diagnosing issues.

### Warning for Auto-Scaling Environments

With **persistent** storage, events on disk survive a Cribl Stream/Edge or OS restart. But you cannot count on this with ephemeral/auto-scaling environments. Stream Workers must **not** be destroyed while data has not drained from a PQ.

## PQ Status Notifications and Monitoring {#notifications}

Depending on your [Cribl license](https://cribl.io/pricing/), you may be able to configure [Notifications](/stream/notifications) that will  be sent when PQ files exceed a configurable percentage of allocated storage, or when a Destination reaches the [queue‑full state](#fallback) described above. 

These Notifications always appear in Cribl Stream's UI and internal logs. You can also send them to external systems. For setup details, see [Destination-State Notifications](/stream/notifications#destination).

### Destination PQ Notifications {#destination-notifications}

With Destination PQ enabled, you can configure Notifications around two trigger conditions. Cribl recommends that you create both: 
* [Destination Backpressure Activated](notifications#backpressure). This will send Notifications whenever backpressure engages, causing blocked or dropped events. It will also send Notifications when a queue fills up, [falling back](#fallback) to blocked or dropped events.
* [Persistent Queue Usage](notifications#pq) (saturation). Note that you can set these Notifications for **multiple** saturation levels.

### Source PQ Alerting {#source-alerting}

Currently, Cribl Stream does not support Notifications around Source PQ states. However, if you've enabled `Always On` mode, alerts that PQ has engaged would be pointless – because all of the Source's incoming data is automatically written to the disk queue.

Also, if desired: On Fleets or customer-managed (on-prem) Worker Groups, you can enable the internal [CriblLogs Source](sources-cribl-internal#configuring), create a Route to forward Cribl's [internal logs](internal-logs#node) to a downstream service of your choice, and configure alerts from there.

### Monitoring PQ {#monitoring}

Cribl Stream's native Monitoring dashboards interact with PQ in particular ways. When PQ engages, assume that your sole sources of truth about Sources' or Destinations' throughput are the dashboards at:
* **Monitoring** > **System** > **Queues (Sources),** and
* **Monitoring** > **System** > **Queues (Destinations)**.

Other Monitoring resources become unreliable during queueing. These include the top-level **Monitoring** dashboard, and:
* **Monitoring** > **Data** > **Sources,** or
* **Monitoring** > **Data** > **Destinations**.

These will not accurately represent queued data because, once PQ engages, data will be queued to disk, rather than arriving into Pipelines and downstream resources (with Source PQ) or into the affected Destinations (with Destination PQ). While the data is queueing, the two above dashboards for:
* Affected Sources will show no events per second (EPS) or bytes ingested while their data is queueing.
* Affected Destinations will show EPS and bytes sent while their data is queueing, but these metrics can be misleading. Here, they measure data going into the queue, but not proceeding to downstream receivers.

Some other Monitoring quirks to keep in mind: 
* Activity in the **Monitoring** > **System** > **Queues (Sources)** and **Monitoring** > **System** > **Queues (Destinations)** dashboards sometimes shows up only when queues drain.
* Cribl Stream currently does not provide a Monitoring dashboard for disk queues' own volumes.

These gaps provide another reason to enable PQ [Notifications](#destination-notifications) where available. 

> Remember that with **or** without PQ enabled, Monitoring dashboards do not show usage of integrations' in-memory buffers.
>
{.box .info}

After the unhealthy condition resolves, and disk queues start to drain, dashboards will resume accurately representing delivery rates and volumes.

### Metrics and Capture Limitations {#troubleshooting}

Finally, while PQ provides many benefits, also note the following limitations: 
* While events go to a Source-side queue, Cribl Stream does not record metrics about that Source's inbound data (events/second or bytes).
* While events go to a Destination-side queue, metrics reflect events/second and bytes going into that Destination's queue, not proceeding beyond Cribl Stream.
* Data captures might not work while a Destination is blocked.
