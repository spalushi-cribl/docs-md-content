# Persistent Queues


Cribl Edge's persistent queuing (PQ) feature helps minimize data loss if a downstream receiver is unreachable. PQ provides durability by writing data to disk for the duration of the outage, and forwarding it upon recovery. 

Persistent queues are implemented:
* On [Push Sources](sources#push).
* On [Streaming Destinations](destinations#streaming-destinations). (Sources can also take advantage of a Destination's queue.)

## Persistent Queues Supplement In‑Memory Queues {#how-does-persistent-queueing-work}

Persistent queues trigger differently on the Destination versus Source side. 

### Destination Side {#destination-triggers}

On each Cribl Edge Destination that supports PQ, an in-memory buffer helps the Destination absorb temporary imbalances between inbound and outbound data rates. Such as, if there is an inbound burst of data, the Destination will store events in the queue, and will then output them at the rate to which the receiver can sync (as opposed to blocking or dropping the events). 

Only when this buffer is full will the Destination impose backpressure upstream. (This threshold varies per Destination type.) This is where persistent queues help safeguard your data.

### Source Side {#source-triggers}

In Cribl Edge 3.4 and later, Push Sources' config modals also provide a PQ option. When enabled, you can choose between two trigger conditions: `Always On` mode will use PQ as a buffer for **all** events, whereas `Smart` mode will engage PQ only upon backpressure from Destinations.

### Life Without PQ

On the Destination side, you can configure backpressure behavior to one of **Block**, **Drop Events**, or (on Destinations that [support](#support) it) **Persistent Queue**. In **Block** mode, the output will refuse to accept new data until the receiver is ready. 

The system will back propagate block "signals" all the way back to the sender (assuming that the sender supports backpressure, too). In general, TCP-based senders support backpressure, but this is not a guarantee: Each upstream application's developer is responsible for ensuring that the application stops sending data once Cribl Edge stops sending TCP acknowledgments back to it.

In **Drop** mode, the Destination will discard new events until the receiver is ready. In some environments, the in-memory queues and their block/drop behavior are acceptable. 

### PQ = Durability

Persistent queues serve environments where more durability is required (such as, outages last longer than memory queues can sustain), or where upstream senders do not support backpressure (such as, ephemeral/network senders).

Engaging persistent queues in these scenarios can help minimize data loss. Once the in-memory queue is full, the Cribl Edge Destination will write its data to disk. Then, when the receiver is ready, the output will start draining the queues.

By default, after data flow is re-established, Cribl Edge will forward events in FIFO (first in, first out) order: It will send out earlier queued events before newly arriving events. In Cribl Edge 4.1 and later, you can instead prioritize new events by disabling Destinations' **Strict ordering** control. 

### Source and/or Destination PQ?

If your Source(s) and Destination(s) both support persistent queues, which side should you enable? If you prioritize maximum data retention and delivery over performance, Cribl recommends that you enable both Source PQ  and Destination PQ.

When PQ is engaged, throughput will be somewhat slower. But in exchange for this extra latency, you'll minimize your risk of data loss.

> Because of this latency penalty, it is redundant to enable PQ on a Source whose upstream sender is configured to safeguard events in its own disk buffer.
>
{.box .info}

## Persistent Queue Details and Constraints

Persistent queues are: 

* Available on [Push Sources](sources#push).
* Available on the output side (that is, after processing) of all streaming Destinations, with these exceptions: Syslog and Graphite (when you select UDP as the outbound protocol) and SNMP Trap.
* Implemented at the Worker Process level, with independent sizing configuration and dynamic engagement per Worker Process.
* With load-balanced Destinations (Splunk Load Balanced, Splunk HEC, Elasticsearch, TCP JSON, and Syslog with TCP), engaged only when **all of the Destination's receivers** are blocking data flow. (Here, a single live receiver will prevent PQ from engaging on the corresponding Destination.)

* On Destinations, engaged only when receivers are down, unreachable, blocking, or throwing a serious error (such as a connection reset). Destination-side PQ is not designed to engage when receivers' data consumption rate simply slows down.
* Drained when at least one receiver can accept data.
* Capable of orphaning events if there are files on disk when a Source or Destination is turned off or if the number of Worker Processes is reduced.
* Not infinite in size. For example, if data cannot be delivered out, you might run out of disk space.
* Not able to fully protect in cases of application failure. Such as, in-memory data might get lost if a crash occurs.
* Not able to protect in cases of hardware failure. Such as, disk failure, corruption, or machine/host loss.
* TLS-encrypted only for data in flight, and only on Destinations where TLS is supported and enabled. To encrypt data at rest, including disk writes/reads, you must configure encryption on the underlying storage volume(s).

## Configuring Persistent Queueing {#configuring}

The following sections outline configuring PQ for Cribl.Cloud Workers managed by Cribl, and then for on-prem and hybrid Cribl.Cloud Workers.

### Configuring PQ for Cribl-Managed Cloud Workers

Persistent queuing is supported for Cribl-managed Workers in Cribl.Cloud deployments with an [Enterprise plan](deploy-cloud#enterprise). Configuration is considerably simpler with Cribl-managed Workers than with hybrid and on-prem Workers (both covered [below](#on-prem)). You configure PQ individually for each Source and Destination that supports it.

#### Sources

1. Click a Source to open its configuration modal. 
2. Select **Persistent Queue Settings**.
3. Toggle the **Enable Persistent Queue** slider on. 

When enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB of data per PQ‑enabled Source, per Worker Process. The 1 GB limit is on uncompressed inbound data, and no compression is applied to the queue. This limit is not configurable. (For a configurable **Max queue size**, use a [hybrid Group](#on-prem).)

> On Cribl-managed Workers, the **Persistent Queue Settings** tab exposes only the **Enable Persistent Queue** toggle. It omits the configuration options available with on-prem or hybrid Workers.
>
{.box .info}

#### Destinations

1. Click a Destination to open its configuration modal.
2. Set **Backpressure behavior** to `Persistent Queueing`. 

When enabled, PQ is automatically configured with a maximum queue size of 1 GB of uncompressed data per PQ‑enabled Destination, per Worker Process. The 1 GB limit is on uncompressed outbound data, and no compression is applied to the queue. This limit is not configurable. (For a configurable **Max queue size**, use a [hybrid Group](#on-prem).)

To disable persistent queuing, select **Persistent Queue Settings**, and then select **Clear Persistent Queue**. 

> On Cribl-managed Workers, the **Persistent Queue Settings** tab exposes only the **Clear Persistent Queue** button. It omits the configuration options available with on-prem or hybrid Workers. If the queue fills up, Cribl Edge will block outbound data.
>
{.box .info}

#### Logs

Cribl internal logs for Cloud-managed Workers might include entries labeled `Revived PQ Worker Process`. These indicate a Cribl-managed Worker newly assigned to drain a Persistent Queue that is left over from a shut-down Worker Node. They can be used to troubleshoot Cloud PQ issues.

### Configuring PQ for On-Prem and Hybrid Workers {#on-prem}

Persistent queuing may be supported for Workers in on-prem deployments and for [hybrid](deploy-cloud#hybrid) Cribl.Cloud Workers, depending on your [Cribl license](https://cribl.io/pricing/). You configure persistent queueing individually for each Source and Destination that supports it. 

On Sources:
1. Click a Source to open its configuration modal. 
2. Select **Persistent Queue Settings**. 
3. Toggle the **Enable Persistent Queue** slider on.

On Destinations:
1. Click a Destination to open its configuration modal.
2. Set **Backpressure behavior** to `Persistent Queueing`.

These selections expose the additional controls outlined below.

> For an example of optimizing several PQ throttling options listed below, see our [Configure Cribl Stream to Avoid Data Loss With Persistent Queuing](https://cribl.io/blog/configure-cribl-logstream-to-avoid-data-loss-with-with-persistent-queuing/) blog post.
>
{.box .success}

#### Source-Side PQ Only {#source}

**Mode**: Select a condition for engaging persistent queues.
- `Always On`: This default option will always write events to the persistent queue, before forwarding them to Cribl Edge's data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from Cribl Edge's data processing engine.

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the Source. Defaults to `1000`.

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.


##### Always On versus Smart Mode {#source-pq-busy}

As of version 4.1, Source-side PQ's default mode changed from `Smart` to `Always on`, for greater reliability. This change does not affect existing Sources' configurations. However:

> If you create Stream Sources programmatically, and you want to enforce the previous `Smart` mode, you'll need to update your existing code.
>
{.box .warning}

If you find `Smart` mode causing Source-side PQ to frequently engage, and PQ is configured with a large **Max file size** (such as, 1 GB), Cribl recommends either of two remedies:
- Switch the PQ configuration to `Always On`. This will avoid triggering backpressure and potential data loss on senders. (However, it might require provisioning more disk space for the queue.) 
- Reduce the **Max file size** to a value no higher than the default `1 MB`. (Source-side PQ excessively triggers backpressure only when it reaches the last queued file **and** that file is large. Keeping this threshold low prevents that condition.)

#### Common PQ Settings (Source and Destination Sides) {#common}

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, and so forth. If not specified, Cribl Edge applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume, on each Worker Process. Once this limit is reached, Cribl Edge will stop queueing data, and will apply the **Queue‑full behavior**. Enter a numeral with units of KB, MB, and so forth. If not specified, the implicit `0` default will enable Cribl Edge to fill all available disk space on the volume.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this value, Cribl Edge will append `/<worker‑id>/<output‑id>`.

**Compression**: Codec to use to compress the persisted data, once a file is closed. Defaults to `None`; `Gzip` is also available.

> ##### Max Queue Size with Compression {{< id `max-compressed` >}}
>
> If you enable **Compression** and also enter a **Max queue size** limit, be aware of how these options interact:
> - Cribl Edge currently applies the **Max queue size** limit against the **uncompressed** data volume entering the queue. 
> - With this combination, Cribl recommends that you set the **Max queue size** limit **higher** than the volume's total available disk space – again disregarding compression. This will maximize queue saturation and minimize data loss. (For details, see [Known Issues](known-issues#CRIBL-10965).)
>
{.box .warning}

#### Destination-Side PQ Only {#destination}

**Queue-full behavior**: Determines whether to block or drop events when the queue is exerting backpressure (because disk is low or at full capacity). **Block** is the same behavior as non-PQ blocking, corresponding to the **Block** option on the **Backpressure behavior** drop-down. **Drop new data** throws away incoming data, while leaving the contents of the PQ unchanged.

**Clear persistent queue**: Click this button if you want to delete the files that are currently queued for delivery to this Destination. A confirmation modal will appear. (Appears only after **Output ID** has been defined.)

> {{< id `dest-4.1` >}} This section's remaining controls are available in Cribl Edge 4.1 and later.
>
{.box .success}

**Strict ordering**: The default `Yes` position enables FIFO (first in, first out) event forwarding. When receivers recover, Cribl Edge will send earlier queued events before forwarding newly arrived events. To instead prioritize new events before draining the queue, toggle this off. Doing so will expose this additional control:
- **Drain rate limit (EPS)**: Optionally, set a throttling rate (in events per second) on writing from the queue to receivers. (The default `0` value disables throttling.) Throttling the queue's drain rate can boost the throughput of new/active connections, by reserving more resources for them. You can further optimize Workers' startup connections and CPU load at **Fleet Settings** > [Worker Processes](settings-group-fleet#processes).

#### Minimum Free Disk Space {#disk}

> For queuing to operate properly, you must provide sufficient disk space. You configure the minimum disk space in **Settings** > **Global Settings** > **General Settings** > **Limits** > **Min Free Disk Space**. If available disk space falls below this threshold, Cribl Edge will stop maintaining persistent queues, and data loss will begin. The default minimum is 5 GB. Be sure to set this on your Edge Nodes (rather than on the Leader Node) when in distributed mode.
>
{.box .warning}

## Persistent Queue Support by Destination Type {#support}

Persistent Queues support, behavior, and triggers vary by Destination type, as summarized below.

### HTTP-Based Destinations

HTTP-based Destinations handle backpressure based on HTTP response codes. The following conditions will trigger PQ:

1. Connection failure.
2. HTTP `500` responses.
3. Data overload – sending more data than the Destination will accept. 

HTTP `400` response errors will not engage PQ, and Cribl Edge will simply drop corresponding events. Cribl Edge cannot retry these requests, because they have been flagged as "bad," and would just fail again. If you see `400` errors, these often indicate a need to correct your Destination's configuration.

HTTP-based  Destinations include: 

* WebHook 
* New Relic Logs & Metrics 
* New Relic Events
* Open Telemetry 
* Sumo Logic 
* DataDog 
* Elastic 
* Honeycomb 
* Prometheus 
* Splunk HEC
* Signal FX 
* Wavefront 
* Google Chronicle 
* Loki 

###  TCP Load-Balanced Destinations

TCP load-balanced Destinations can be configured with one or multiple receivers. If one or more receivers go down, Cribl Edge will continue sending data to any healthy receivers. The following conditions will trigger PQ:

1. Connection errors on **all** receivers.

    > As long as even one of the Destination's receivers is healthy, Cribl Edge will redirect data to that receiver, and will **not** engage PQ.
    {.box .info}

2. Data overload – sending more data than the Destination will accept. 

TCP load-balanced Destinations include: 

* Splunk Load Balanced
* TCP JSON
* Syslog 

### Destinations Without PQ Support

Filesystem-based Destinations, and Destinations using the UDP protocol, do not support PQ. These include:

* Amazon S3
* Filesystem
* Azure Blob Storage
* Google Cloud Storage 
* SNMP Trap
* StatsD (with UDP)
* StatsD Extended (with UDP)
* Syslog (with UDP)

Filesystem-based Destinations do not support PQ because they already persist events to disk, before sending them to their final destinations.

UDP-based Destinations do not support PQ because the protocol is not reliable. Cribl Edge gets no indication whether the receiver received an event.

### Other Destinations

Other Destinations with a single receiver generally engage PQ based on these trigger conditions:

1. Connection errors.
2. Fail to send an event (for any reason).

## Getting Notified about PQ Status {#notifications}

Depending on your [Cribl license](https://cribl.io/pricing/), you may be able to configure [Notifications](/stream/notifications) to be sent when Persistent Queue files exceed a configurable percentage of allocated storage, or when they reach the queue-full state described above. 

These Notifications always appear in Cribl Edge's UI and internal logs, and you can also send them to external systems. For setup details, see [Destination-State Notifications](/stream/notifications#destination).


