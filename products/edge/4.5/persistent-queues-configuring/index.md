# Configuring Persistent Queues


The following sections outline configuring PQ for Cribl.Cloud Workers managed by Cribl, and then for on-prem and hybrid Cribl.Cloud Workers.

## Configuring PQ for Cribl-Managed Cribl.Cloud Workers {#cloud-managed}

Persistent queuing may be supported for Cribl-managed Workers in Cribl.Cloud deployments, depending on your [Cribl license](https://cribl.io/pricing/). Configuration is considerably simpler with Cribl-managed Workers than with hybrid and on-prem Workers (both covered [below](#on-prem)). You configure PQ individually on each Source and Destination that supports it.

### Sources

1. Click a Source to open its configuration modal. 
2. Select the **Persistent Queue Settings** tab.
3. Toggle **Enable Persistent Queue** to on. 

On Cribl-managed Workers, this tab exposes only the **Enable Persistent Queue** toggle. When enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB of data per PQ‑enabled Source, per Worker Process. The 1 GB limit is on uncompressed inbound data, and no compression is applied to the queue. 

These options are not configurable. If you want to finely configure the maximum queue size, compression, PQ mode, and other options, use a [hybrid Group](#on-prem).

### Destinations

1. Click a Destination to open its configuration modal.
2. Set **Backpressure behavior** to `Persistent Queueing`. 

On Cribl-managed Workers, the resulting **Persistent Queue Settings** tab exposes only a **Clear Persistent Queue** button. Once enabled, PQ is automatically configured with a maximum queue size of 1 GB of uncompressed data per PQ-enabled Destination, per Worker Process. The 1 GB limit is on uncompressed outbound data, and no compression is applied to the queue. If the queue fills up, Cribl Edge will block data flow. 

These options are not configurable. If you want to finely configure the maximum queue size, compression, queue-full fallback behavior, and other options, use a [hybrid Group](#on-prem).

If you want to disable persistent queuing:

1. Wait for the queue to fully drain.  
  (The **Clear Persistent Queue** button is available as a destructive fallback. This will free up disk space, but will do so by deleting the queued data **without** sending it on to receivers.)
2. In the config modal's **General Settings**, select a different **Backpressure behavior**. 
3. Re-save the config.

### Logs

Cribl internal logs for Cribl-managed Workers might include entries labeled `Revived PQ Worker Process`. These indicate where a Cribl-managed Worker was newly assigned to drain a persistent queue left over from a shut-down Worker Node. You can use these log events to troubleshoot Cloud PQ issues.

## Configuring PQ for On-Prem and Hybrid Workers {#on-prem}

Persistent queuing may be supported for Workers in on-prem deployments and for [hybrid](cloud-enterprise#hybrid) Cribl.Cloud Workers, depending on your [Cribl license](https://cribl.io/pricing/). (Cribl-managed Cloud Workers have a [simpler configuration](#cloud-managed).) You configure persistent queueing individually for each Source and Destination that supports it. 

On Sources:
1. Click a Source to open its configuration modal. 
2. Select **Persistent Queue Settings**. 
3. Toggle **Enable Persistent Queue** to on.

On Destinations:
1. Click a Destination to open its configuration modal.
2. Set **Backpressure behavior** to `Persistent Queueing`.

These selections expose the additional controls outlined below.

> For an example of optimizing several PQ throttling options listed below, see our [How Does Persistent Queuing Work Inside Cribl Stream?](https://cribl.io/blog/configure-cribl-logstream-to-avoid-data-loss-with-with-persistent-queuing/) blog post.
>
{.box .success}

### Source-Side PQ Only {#source}

**Mode**: Select a condition for engaging persistent queues.
- `Always On`: This default option will always write events to the persistent queue, before forwarding them to Cribl Edge's data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from Cribl Edge's data processing engine.

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the sender and writing the queue to disk. Defaults to `1000`. (This buffer is per connection, not just per Worker Process – and this can dramatically expand memory usage.)

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.


#### Always On versus Smart Mode {{< id `source-pq-busy` >}}

In Cribl Edge 4.1 and later, Source-side PQ defaults to `Always on` mode when you configure a new Source. `Always on` offers the highest data durability, by minimizing backpressure and potential data loss on senders. However, this mode can slightly slow down data throughput, and can require you to provision more machines and/or faster disks.

If you are upgrading from a pre-4.1 version where `Smart` mode was the default, this change does not affect your existing Sources' configurations. However:
- If you create new Stream Sources programmatically, and you want to enforce the previous `Smart` mode, you'll need to update your existing code.
- You can optimize Workers' startup connections and CPU load at **Fleet Settings** > [Worker Processes](settings-group-fleet#processes).

Consider switching to `Always on` mode if you find `Smart` mode causing Source-side PQ to frequently engage, and you've configured PQ with a large **Max file size** (e.g., 1 GB). However, a second option here is to retain `Smart` mode, but reduce the **Max file size** to a value no higher than the default `1 MB`.

(Source-side PQ excessively triggers backpressure only when it reaches the last queued file **and** that file is large. Keeping this threshold low prevents that condition.)

### Common PQ Settings (Source and Destination Sides) {#common}

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, etc. If not specified, Cribl Edge applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume, on each Worker Process. Once this limit is reached, this Source or Destination will stop queueing data; Sources will block, and Destinations will apply your configured **Queue‑full behavior**. Defaults to `5 GB`. Enter a numeral with units of KB, MB, GB, or TB. For details, see [Optimizing Max Queue Size](#max).

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this value, Cribl Edge will append `/<worker‑id>/<output‑id>`.

**Compression**: Codec to use to compress the persisted data, once a file is closed. Defaults to `None`; `Gzip` is also available.

#### Optimizing Max Queue Size {#max}

> When you upgrade Fleets, and customer-managed (on-prem) and hybrid Groups, to Cribl Edge 4.4 and later: Any undefined PQ **Max queue size** values in pre-4.4 Source/Destination configs will be set to the new 5 GB default value. This is a guardrail to control queues' consumption of host machines' disk space.
>
{.box .warning}

Adjust this default to match your needs. If some of your integrations' persistent queues had an undefined **Max queue size**, queues larger than 5 GB will trigger PQ fallback behavior until you resize this limit.

Also in Cribl Edge 4.4 and later, this field's highest accepted value is defined in the new **Group Settings** > **General Settings** > **Limits** > **Storage** > **Max PQ size per Worker Process** field. The corresponding `limits.yml` [key](limitsyml) is `maxPQSize`. Consult Cribl Support before increasing that limit's 1 TB default.

The changes here do not affect any existing config with a defined **Max queue size**, and do not affect Cribl-managed Cloud Workers' nonconfigurable 1 GB **Max queue size**.

##### Max Queue Size with Compression {#max-compressed}

If you enable **Compression** and also enter a **Max queue size** limit, be aware of how these options interact:
- Cribl Edge currently applies the **Max queue size** limit against the **uncompressed** data volume entering the queue. 
- With this combination, Cribl recommends that you set the **Max queue size** limit **higher** than the volume's total available disk space – again disregarding compression. This will maximize queue saturation and minimize data loss. (For details, see [Known Issues](known-issues#CRIBL-10965).)

### Destination-Side PQ Only {#destination}

**Queue-full behavior**: Determines whether to block or drop events when the queue is exerting backpressure (because disk is low or at full capacity). **Block** is the same behavior as non-PQ blocking, corresponding to the **Block** option on the **Backpressure behavior** drop-down. **Drop new data** throws away incoming data, while leaving the contents of the PQ unchanged.

**Clear Persistent Queue**: Click this "panic" button if you want to delete the files that are currently queued for delivery to this Destination. A confirmation modal will appear - because this will free up disk space by permanently deleting the queued data, without delivering it to downstream receivers. (Appears only after **Output ID** has been defined.)

> {{< id `dest-4.1` >}} This section's remaining controls are available in Cribl Edge 4.1 and later.
>
{.box .success}

**Strict ordering**: The default `Yes` position enables FIFO (first in, first out) event forwarding. When receivers recover, Cribl Edge will send earlier queued events before forwarding newly arrived events. To instead prioritize new events before draining the queue, toggle this off. Doing so will expose this additional control:
- **Drain rate limit (EPS)**: Optionally, set a throttling rate (in events per second) on writing from the queue to receivers. (The default `0` value disables throttling.) Throttling the queue's drain rate can boost the throughput of new/active connections, by reserving more resources for them. You can further optimize Workers' startup connections and CPU load at **Fleet Settings** > [Worker Processes](settings-group-fleet#processes).

### Minimum Free Disk Space {#disk}

> For queuing to operate properly, you must provide sufficient disk space. In distributed mode, you configure the minimum disk space to support queues (and other features) on each Fleet, at **Fleet Settings** > **General Settings** > **Limits** > **Min free disk space**. If available disk space falls below this threshold, Cribl Edge will stop maintaining persistent queues, and data loss will begin. The default minimum is 5 GB.
>
{.box .warning}
