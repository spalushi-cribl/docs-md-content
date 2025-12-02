# Persistent Queues


Cribl Stream's persistent queuing (PQ) feature helps minimize data loss if a downstream receiver or is unreachable or slow to respond, or (when configured on Sources) during backpressure from Cribl Stream internal processing. PQ provides durability by writing data to disk for the duration of the outage, and then forwarding it upon recovery. 

Persistent queues can be enabled:
* On [Push Sources](sources#push).
* On [Streaming Destinations](destinations#streaming-destinations). (Sources can also take advantage of a Destination's queue to keep data flowing.) For details, see [Persistent Queues Support](persistent-queues-support).

> The PQ feature may be restricted based on your Cribl license. See the [Pricing](https://cribl.io/pricing/) page for details. 
>
{.box .info}

## Persistent Queues Supplement In‑Memory Queues {#how-does-persistent-queueing-work}

Persistent queues trigger differently on the [Destination](#destination-trigger) versus [Source](#source-triggers) side. 

### Destination Side {#destination-triggers}

On Cribl Stream Destinations, in-memory buffers help absorb temporary imbalances between inbound and outbound data rates. For example, if there is an inbound burst of data, a Worker Process will store events in Destination buffers, and will then output them at the rate that the downstream receiver can catch up. 

Only when buffers are saturated will the Destination impose backpressure upstream. (This threshold varies per Destination type.) This is where enabling persistent queues helps safeguard your data.

#### Destinations Without PQ

You can configure each Destination's backpressure behavior to one of **Block**, or **Drop Events**, or (on Destinations that [support](persistent-queues-support) it) **Persistent Queue**. 

In **Block** mode, the output will refuse to accept new data until the receiver is ready. The system will back propagate block "signals" all the way back to the sender (assuming that the sender supports backpressure, too). In general, TCP-based senders support backpressure, but this is not a guarantee: Each upstream application's developer is responsible for ensuring that the application stops sending data once Cribl Stream stops sending TCP acknowledgments back to it.

In **Drop** mode, the Destination will discard new events until the receiver is ready. In some environments, the in-memory queues and their block or drop behavior are acceptable. 

#### PQ = Durability

Persistent queues serve environments where more durability is required (e.g., outages last longer than memory queues can sustain), or where upstream senders do not support backpressure (e.g., ephemeral/network senders).

Engaging persistent queues in these scenarios can help minimize data loss. Once the in-memory buffer is full, the Source or Destination will write its data to disk. Then, when the downstream receiver or Cribl process is ready, the Source/Destination will start draining its queue.

By default, after data flow is re-established, a Destination will forward events in FIFO (first in, first out) order - it will send out earlier queued events before newly arriving events. However, in Cribl Stream 4.1 and later, you can instead prioritize new events by disabling Destinations' **Strict ordering** [control](persistent-queues-configuring#destination). 

### Source Side {#source-triggers}

Push Sources' config modals also provide a PQ option to supplement in-memory buffering. When you enable PQ, you can choose between two trigger [conditions](persistent-queues-configuring#source-pq-busy): 
- `Always On` mode will use PQ as a disk buffer for **all** events.
- `Smart` mode will engage PQ only upon backpressure from downstream receivers or Cribl internal processes.

> To learn more about PQ options on both the Source and Destination sides, see [Planning for Persistent Queues](persistent-queues-planning). For implementation details, see [Configuring Persistent Queues](persistent-queues-configuring).
>
{.box .success}

## Persistent Queue Details and Constraints {#constraints}

Persistent queues are: 

- Available on [Push Sources](sources#push).
- Available on the output side (that is, after processing) of all [streaming Destinations](destinations#streaming-destinations), with these exceptions: Syslog and Graphite (when you select UDP as the outbound protocol), Azure Data Explorer (when you select Batching mode), and SNMP Trap.

  > For specifics on both sides, see [Persistent Queues Support](persistent-queues-support).
  {.box .success}

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
