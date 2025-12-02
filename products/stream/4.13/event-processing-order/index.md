# Event Processing Order


The expanded schematic below shows how all events in the Cribl Stream ecosystem are processed linearly, from left to right.

![Cribl Stream in great detail](event-processing-order.png)
{border="false"}

Here are the stages of event processing:

1. [Sources](sources): Data arrives from your choice of external providers. (Cribl Stream supports Splunk, HTTP/S, Elastic Beats, Amazon Kinesis/S3/SQS, Kafka, TCP raw or JSON, and many others.) Many [internal fields](event-model#internal-fields) are added during the event parsing within the Source.

1. Custom command: Optionally (where available), you can pass this input's data to an external command before the data continues downstream. This external command will consume the data via `stdin`, will process it and send its output via `stdout`.

1. [Event Breakers](event-breakers) can, optionally, break up incoming bytestreams into discrete events. (Note that because Event Breakers precede tokens, Breakers cannot see or act on tokens.)

1. [Time filters](collectors-schedule-run#time-range) in Collector jobs discard events outside the specified time range, ensuring only time-relevant events proceed further in the workflow.

1. Fields/Metadata: Optionally, you can add these enrichments to each incoming event. You add fields by specifying key-value pairs, per Source, in a format similar to Cribl Stream's [Eval](eval-function) Function. Each key defines a field name, and each value is a JavaScript expression (or constant) used to compute the field's value.

1. [Persistent queue](persistent-queues#source-triggers) (Source-side): Optionally, you can enable PQ per Source instance, to disk-buffer and maximize data delivery despite downstream backpressure. Events will be written to a disk queue at this point.

1. The [internal field](event-model#internal-fields) `__inputId` is added at this stage, after the event exits the Source.

1. Pre-processing Pipeline: Optionally, you can use a single Pipeline to condition (normalize) data from this input before the data proceeds further.

1. [Routes](routes) map incoming events to Processing Pipelines and Destinations. A Route can accept data from multiple Sources, but each Route can be associated with only one Pipeline and one Destination. ([QuickConnect](quickconnect) bypasses Routes.)

1. Processing [Pipelines](pipelines) perform most event transformations. Within a Pipeline, you define these transformations as a linear series of [Functions](functions). A Function is an atomic piece of JavaScript code invoked on each event. ([QuickConnect](quickconnect) optionally bypasses processing Pipelines.)

1. Post-processing Pipeline: Optionally, you can append a Pipeline to condition (normalize) data on its way into its Destination.

1. [Persistent queue](persistent-queues#destination-triggers) (Destination-side): Optionally, you can enable PQ per Destination instance, to disk-buffer and maximize data delivery during imbalances between inbound and outbound data rates. Events will be written to a disk queue at this point.

1. [Destinations](destinations): Each Route/Pipeline combination forwards processed data to your choice of streaming or storage Destination. (Cribl Stream supports Splunk, Syslog, Elastic, Kafka/Confluent, Amazon S3, Filesystem/NFS, and many other options.)

Troubleshoot event processing in Cribl Stream from right to left. Start at the Destination, and check for block status from the Destination back to the Source. If one Worker Group is sending to another, the right-most destination will be in the receiving Worker Group.

> ##### Pipelines Everywhere
>
> All Pipelines have the same basic internal structure – they're a series of Functions. The three Pipeline types identified above differ only in their position in the system.
>
{.box .info}
