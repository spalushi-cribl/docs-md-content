# Persistent Queues


Cribl Edge's persistent queuing (PQ) feature helps minimize data loss if a downstream receiver or internal process is unreachable or slow to respond. PQ provides durability by writing data to disk for the duration of the outage, and then forwarding it upon recovery. 

Persistent queues can be enabled:
* On [Push Sources](sources#push).
* On [Streaming Destinations](destinations#streaming-destinations). (Sources can also take advantage of a Destination's queue to keep data flowing.)

> The PQ feature may be restricted based on your Cribl license. See the [Pricing](https://cribl.io/pricing/) page for details. 
>
{.box .info}

## Persistent Queues Supplement In‑Memory Queues {#how-does-persistent-queueing-work}

Persistent queues trigger differently on the Destination versus Source side. 

### Destination Side {#destination-triggers}

On each Cribl Edge Destination, an in-memory buffer helps the Destination absorb temporary imbalances between inbound and outbound data rates. For example, if there is an inbound burst of data, the Destination will store events in its buffer, and will then output them at the rate that the downstream receiver can catch up. 

Only when this buffer is full will the Destination impose backpressure upstream. (This threshold varies per Destination type.) This is where persistent queues help safeguard your data.

#### Life Without PQ

You can configure each Destination's backpressure behavior to one of **Block**, or **Drop Events**, or (on Destinations that [support](#support) it) **Persistent Queue**. 

In **Block** mode, the output will refuse to accept new data until the receiver is ready. The system will back propagate block "signals" all the way back to the sender (assuming that the sender supports backpressure, too). In general, TCP-based senders support backpressure, but this is not a guarantee: Each upstream application's developer is responsible for ensuring that the application stops sending data once Cribl Edge stops sending TCP acknowledgments back to it.

In **Drop** mode, the Destination will discard new events until the receiver is ready. In some environments, the in-memory queues and their block or drop behavior are acceptable. 

### PQ = Durability

Persistent queues serve environments where more durability is required (such as, outages last longer than memory queues can sustain), or where upstream senders do not support backpressure (such as, ephemeral/network senders).

Engaging persistent queues in these scenarios can help minimize data loss. Once the in-memory buffer is full, the Source or Destination will write its data to disk. Then, when the downstream receiver or Cribl process is ready, the Source/Destination will start draining its queue.

By default, after data flow is re-established, a Destination will forward events in FIFO (first in, first out) order: It will send out earlier queued events before newly arriving events. However, in Cribl Edge 4.1 and later, you can instead prioritize new events by disabling Destinations' **Strict ordering** [control](#destination). 

### Source Side {#source-triggers}

Push Sources' config modals also provide a PQ option. When you enable PQ, you can choose between two trigger [conditions](#source-pq-busy): 
- `Always On` mode will use PQ as a buffer for **all** events.
- `Smart` mode will engage PQ only upon backpressure from downstream receivers or Cribl processes.

### Source and/or Destination PQ?


If your Source(s) and Destination(s) both support persistent queues, which side should you enable? If you prioritize maximum data retention and delivery over performance, Cribl recommends that you enable both Source PQ  and Destination PQ.

When PQ is engaged, throughput will be somewhat slower. But in exchange for this extra latency, you'll minimize your risk of data loss.

> Because of this latency penalty, it is redundant to enable PQ on a Source whose upstream sender is configured to safeguard events in its own disk buffer.
>
{.box .info}

## Persistent Queue Details and Constraints {#constraints}

Persistent queues are: 

- Available on [Push Sources](sources#push).
- Available on the output side (that is, after processing) of all streaming Destinations, with these exceptions: Syslog and Graphite (when you select UDP as the outbound protocol) and SNMP Trap.
- Implemented at the Worker Process level, with independent sizing configuration and dynamic engagement per Worker Process.
- With load-balanced Destinations (such as [Splunk Load Balanced](destinations-splunk-lb), [Splunk HEC](destinations-splunk-hec), [Elasticsearch](destinations-elastic), [TCP JSON](destinations-tcp-json), and [Syslog](destinations-syslog) with TCP), engaged only when **all of the Destination's receivers** are blocking data flow. (Here, a single live receiver will prevent PQ from engaging on the corresponding Destination.)

  Note that PQ will not engage if at least one outbound TCP socket connection is active, even if the receiver is sending a backpressure signal.
  - For Kafka-based Destinations (such as [Kafka](destinations-kafka), [Confluent Cloud](destinations-confluent),
  [Amazon MSK](destinations-msk), and [Azure Event Hubs](destinations-azure-event-hubs)),
  PQs engage when the brokers can't be reached.
- On Destinations, engaged only when receivers are down, unreachable, blocking, or throwing a serious error (such as a connection reset). Destination-side PQ is not designed to engage when receivers' data consumption rate simply slows down.
- Drained when at least one receiver can accept data.
- Not infinite in size. This means that if data cannot be delivered out, you might run out of disk space.
- Not able to fully protect in cases of application failure. For example, if a crash occurs, in-memory data might get lost.
- Not able to protect in cases of hardware failure, such as disk failure, corruption, or machine/host loss.
- TLS-encrypted only for data in flight, and only on Destinations where TLS is supported and enabled. To encrypt data at rest, including disk writes/reads, you must configure encryption on the underlying storage volume(s).

> Beyond the failure scenarios above: If you turn off PQ on a Source or Destination (or shut down the whole Source/Destination) before the disk queue has drained, the queued events will be orphaned on disk.
>
{.box .warning}

## Configuring Persistent Queueing {#configuring}

The following sections outline configuring PQ for Cribl.Cloud Workers managed by Cribl, and then for on-prem and hybrid Cribl.Cloud Workers.

### Configuring PQ for Cribl-Managed Cribl.Cloud Workers {#cloud-managed}

Persistent queuing may be supported for Cribl-managed Workers in Cribl.Cloud deployments, depending on your [Cribl license](https://cribl.io/pricing/). Configuration is considerably simpler with Cribl-managed Workers than with hybrid and on-prem Workers (both covered [below](#on-prem)). You configure PQ individually on each Source and Destination that supports it.

#### Sources

1. Click a Source to open its configuration modal. 
2. Select the **Persistent Queue Settings** tab.
3. Toggle **Enable Persistent Queue** to on. 

On Cribl-managed Workers, this tab exposes only the **Enable Persistent Queue** toggle. When enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB of data per PQ‑enabled Source, per Worker Process. The 1 GB limit is on uncompressed inbound data, and no compression is applied to the queue. 

These options are not configurable. If you want to finely configure the maximum queue size, compression, PQ mode, and other options, use a [hybrid Group](#on-prem).

#### Destinations

1. Click a Destination to open its configuration modal.
2. Set **Backpressure behavior** to `Persistent Queueing`. 

On Cribl-managed Workers, the resulting **Persistent Queue Settings** tab exposes only a **Clear Persistent Queue** button. Once enabled, PQ is automatically configured with a maximum queue size of 1 GB of uncompressed data per PQ‑enabled Destination, per Worker Process. The 1 GB limit is on uncompressed outbound data, and no compression is applied to the queue. If the queue fills up, Cribl Edge will block data flow. 

These options are not configurable. If you want to finely configure the maximum queue size, compression, queue-full fallback behavior, and other options, use a [hybrid Group](#on-prem).

If you want to disable persistent queuing:

1. Click the **Clear Persistent Queue** button.
2. Follow the resulting link to make sure the queue fully drains.
3. In the config modal's **General Settings**, select a different **Backpressure behavior**, then re-save the config.

#### Logs

Cribl internal logs for Cribl-managed Workers might include entries labeled `Revived PQ Worker Process`. These indicate where a Cribl-managed Worker was newly assigned to drain a persistent queue left over from a shut-down Worker Node. You can use these log events to troubleshoot Cloud PQ issues.

### Configuring PQ for On-Prem and Hybrid Workers {#on-prem}

Persistent queuing may be supported for Workers in on-prem deployments and for [hybrid](cloud-enterprise#hybrid) Cribl.Cloud Workers, depending on your [Cribl license](https://cribl.io/pricing/).(Cribl-managed Cloud Workers have a [simpler configuration](#cloud-managed).) You configure persistent queueing individually for each Source and Destination that supports it. 

On Sources:
1. Click a Source to open its configuration modal. 
2. Select **Persistent Queue Settings**. 
3. Toggle **Enable Persistent Queue** to on.

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

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the sender and writing the queue to disk. Defaults to `1000`. (This buffer is per connection, not just per Worker Process – and this can dramatically expand memory usage.)

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.


##### Always On versus Smart Mode{#source-pq-busy}

In Cribl Edge 4.1 and later, Source-side PQ defaults to `Always on` mode when you configure a new Source. `Always on` offers the highest data durability, by minimizing backpressure and potential data loss on senders. However, this mode can slightly slow down data throughput, and can require you to provision more machines and/or faster disks.

If you are upgrading from a pre-4.1 version where `Smart` mode was the default, this change does not affect your existing Sources' configurations. However:
- If you create new Stream Sources programmatically, and you want to enforce the previous `Smart` mode, you'll need to update your existing code.
- You can optimize Workers' startup connections and CPU load at **Fleet Settings** > [Worker Processes](settings-group-fleet#processes).

Consider switching to `Always on` mode if you find `Smart` mode causing Source-side PQ to frequently engage, and you've configured PQ with a large **Max file size** (such as, 1 GB). However, a second option here is to retain `Smart` mode, but reduce the **Max file size** to a value no higher than the default `1 MB`.

(Source-side PQ excessively triggers backpressure only when it reaches the last queued file **and** that file is large. Keeping this threshold low prevents that condition.)

#### Common PQ Settings (Source and Destination Sides) {#common}

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, and so forth. If not specified, Cribl Edge applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume, on each Worker Process. Once this limit is reached, this Source or Destination will stop queueing data; Sources will block, and Destinations will apply your configured **Queue‑full behavior**. Enter a numeral with units of KB, MB, and so forth. If not specified, the implicit `0` default will enable Cribl Edge to fill all available disk space on the volume.

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

> For queuing to operate properly, you must provide sufficient disk space. In distributed mode, you configure the minimum disk space to support queues (and other features) on each Fleet, at **Fleet Settings** > **General Settings** > **Limits** > **Min free disk space**. If available disk space falls below this threshold, Cribl Edge will stop maintaining persistent queues, and data loss will begin. The default minimum is 5 GB.
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

Filesystem-based Destinations, and Destinations that use the UDP protocol, do not support PQ. These include:

* Amazon S3 Compatible Stores
* Data Lakes > Amazon S3
* Data Lakes > Amazon Security Lake
* Azure Blob Storage
* Filesystem/NFS
* Google Cloud Storage
* MinIO
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
