# Event Breaker


This Function enables you to split large blobs or streams of events into discrete events within a Pipeline. This is useful for Sources like Azure Event Hubs, which do not natively support [Event Breakers](event-breakers). 

Even with Sources that do support Event Breakers (like Raw HTTP), it sometimes makes sense to use both a Source-configured Event Breaker on the incoming stream, and an Event Breaker Function within a Pipeline.

## Limitations

> The Event Breaker Function operates only on data in `_raw`. For other events, move the array to `_raw` and stringify it before applying this Function.
> 
> The largest event that this Function can break is about about 128 MB (`134217728` bytes). Events exceeding this maximum size will be split into separate events, but left unbroken. Cribl Stream will set these events' `__isBroken` internal field to `false`.
> 
> Unlike regular [Event Breakers](event-breakers), Event Breaker Functions do not have names, only descriptions.
>
{.box .warning}

## Usage

Use the following options to define this Event Breaker Function.

**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

**Existing or new?**: Whether to use an existing ruleset or create a new one. Defaults to `Use Existing`.

* When **Existing or new?** is set to `Use Existing`, the **Existing ruleset** drop-down appears and you choose a ruleset from there.

* When **Existing or new?** is set to `Create New`, the ruleset creation UI appears and you configure its [settings](event-breakers#types).

### Advanced Settings

**Add to cribl_breaker**: Whether to add the `cribl_breaker` field to output events. Defaults to toggled on.

How this field behaves depends on whether you are **also** using a regular [Event Breaker](event-breakers) with your Source. That matters because a regular Event Breaker **always** adds the `cribl_breaker` field to events, which it does **before** the data reaches your Event Breaker Function.

When you are **not** using a regular Event Breaker, there is no `cribl_breaker` field yet when the data reaches your Event Breaker Function. At this point, `cribl_breaker` is added, and:

* When **Existing or new?** is set to `Use Existing`, `cribl_breaker`'s value is set to the name of the existing ruleset you chose.

* When **Existing or new?** is set to `Create New`, the `cribl_breaker`'s value is set to `"event_breaker_func"`. Since Event Breaker Functions do not have names, the string `event_breaker_func` serves as a kind of generic name that represents whatever new Event Breaker Function you created.

When you **are** also using a regular Event Breaker, the `cribl_breaker` field already exists when the data reaches your Event Breaker Function. At this point, the value of `cribl_breaker` is changed from a string to an array, with the first item in the array being the value originally set by the regular Event Breaker, and the second item determined as follows:

* When **Existing or new?** is set to `Use Existing`, the second item's value is set to the name of the existing ruleset you chose.

* When **Existing or new?** is set to `Create New`, the second item's value is set to `"event_breaker_func"`. Since Event Breaker Functions do not have names, the string `event_breaker_func` serves as a kind of generic name that represents whatever new Event Breaker Function you create.

> Cribl Stream truncates timestamps to three-digit (milliseconds) resolution, omitting trailing zeros.
> 
> In Cribl Stream 3.4.2 and above, where an Event Breaker Function has set an event's `_time` to the current time – rather than extracting the value from the event itself – it will mark this by adding the internal field `__timestampExtracted: false` to the event.
>
{.box .info}

## Examples

Handling syslog data and nested JSON data are two primary use cases for Event Breaker Functions.

### Event Breaker Functions for syslog Data

Event Breaker Functions can help you deal with syslog data that does not break correctly. Examples include multi-line syslog data, and, the kinds of non-standard syslog data emitted by Blue Coat proxy appliances, Layer7 API Gateways, and other appliances. See the Cribl video [Scaling syslog](https://youtu.be/m2yTNLwtIrA) for an in-depth discussion.

(In this context, "non-standard" means syslog data that only partly conforms to the standard Syslog Protocol defined in [RFC 5424](https://datatracker.ietf.org/doc/html/rfc5424), or its predecessor, the BSD syslog Protocol defined in [RFC 3164](https://datatracker.ietf.org/doc/html/rfc3164).) 

### Event Breaker Functions for Nested JSON Data

Kafka-based data from Confluent, Azure Event Hubs, Google Pub Sub, or Kafka itself, is nicely structured as events according to the Kafka [protocol](https://kafka.apache.org/protocol). But when these events contain JSON objects which you want to break into their constituent objects, you can use an Event Breaker Function to do that.

## Troubleshooting

 If you notice fragmented events, check whether Cribl Stream has added a `__timeoutFlush` internal field to them. This  diagnostic field's presence indicates that the events were flushed because the Event Breaker buffer timed out while processing them. These timeouts can be due to large incoming events, backpressure, or other causes.
