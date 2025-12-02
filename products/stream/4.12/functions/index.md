# Functions


When events enter a Pipeline, they're processed by a series of Functions. At its core, a Function is code that executes on an event, and it encapsulates the smallest amount of processing that can happen to that event.

The term "processing" means a variety of possible options: string replacement, obfuscation, encryption, event-to-metrics conversions, and so on. For example, a Pipeline can be composed of several Functions – one that replaces the term `foo` with `bar`, another one that hashes `bar`, and a final one that adds a field (say, `dc=jfk-42`) to any event that matches `source=='us-nyc-application.log'`.

## How Do They Work

Functions are atomic pieces of JavaScript code that are invoked on each event that passes through them. To help improve performance, configure Functions with filters to further scope their invocation to matching events only. See [Build Custom Logic to Route and Process Your Data: Filter Expressions](filter-and-transform-data#filters) for more information.

You can add as many Functions in a Pipeline as necessary, though the more you have, the longer it will take each event to pass through. Also, you can enable and disable Functions within a Pipeline as necessary. This lets you preserve structure as you optimize or debug.

![Functions stack in a Pipeline](functions-screen-4101.png)
{border="true"}

You can reposition Functions up or down the Pipeline stack to adjust their execution order. Use a Function's left grab handle to drag and drop it into place.

## The **Final** Toggle

The **Final** toggle in Function settings controls what happens to the results of a Function.

When **Final** is toggled off (default), events will be processed by this Function,
**and** then passed on to the next Function below.

When **Final** is toggled on, the Function "consumes" the results,
meaning they will not pass down to any other Function below.

A flag in Pipeline view indicates that a Function is **Final**.

![Functions in a Pipeline, some of them have Final flags.](pipeline-final-flag-functions-4101.png)
{border="true" caption="The Eval and first Drop Functions bearing the Final flag"}

## Functions and Shared-Nothing Architecture  {#shared-nothing}

Cribl Stream is built on a shared-nothing architecture, where each Node and its **Worker Processes operate separately, and process events independently of each other**. This means that all Functions operate strictly in a Worker Process context – state is not shared across processes.

For example, consider two events that meet a Pipeline's criteria to be aggregated. If these two events arrive on **separate** Workers or Worker Processes, Cribl Stream will **not** aggregate them together.

This is particularly important to understand for certain Functions that might imply state-sharing, such as [Aggregations](aggregations-function), [Rollup Metrics](rollup-metrics-function), [Dynamic Sampling](dynamic-sampling-function), [Sampling](sampling-function), [Suppress](suppress-function), and so on.

If you have a large number of Worker Processes, consider implementing a distributed caching tier, such as [Redis](https://redis.io/docs/about/), to aggregate events across Workers. (See our [Redis Function](redis-function) topic.)

## Out-of-the-Box Functions {#oob}

Cribl Stream ships with several Functions out-of-the-box, and you can chain them together to meet your requirements. For more details, see individual **Functions**, and the **Use Cases** section, within this documentation.

## Accessing Event Fields with `__e` {#special-e}

The special variable `__e` represents the `(context)` event inside a JavaScript expression. Using  `__e` with square bracket notation, you can access any field within the event object, for example,  `__e['hostname']`. Make sure to use single quotes around the field name. Functions use `__e` [extensively](/stream/usecase-code-function#accessing-fields-in-an-event).

You also **must** use this notation for fields that contain a special (non-alphanumeric) character like `user-agent`, `kubernetes.namespace_name`, or `@timestamp`. For more details, see MDN's [Property Accessors](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_accessors) documentation. In any other place where such fields are referenced – for example, in an [Eval Function](eval-function)'s Field names – you should use a single-quoted literal, of the form: `'<field-name-here>'`.

## Supported JavaScript Standard

Cribl Stream supports the [ECMAScript® 2015 Language Specification](https://262.ecma-international.org/6.0/).

## Very Large Integer Values
Cribl Stream's JavaScript implementation can safely represent integers only up to the [Number.MAX_SAFE_INTEGER](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER) constant of about 9 quadrillion (precisely, {2^53}‑1). Cribl Stream Functions will round down any integer larger than this, in Data Preview and other contexts. Trailing 0’s might indicate such rounding down of large integers.

## Specify Fields' Precedence Order in Expressions  {#precedence}

For Sources that support adding **Fields (Metadata)**, you can use a **Value** expression to allow fields in events to override the predefined field values. For example, the following expression's left-to-right OR logic ensures that if an inbound event includes an `index` field, its value is used: `${__e['index'] || 'myIndex'}`.

If no `index` field is present, the expression falls back to the constant `myIndex` defined in the expression.

## What Functions to Use When

* Add, remove, update fields:
[Eval](eval-function), [Lookup](lookup-function), [Regex Extract](regex-extract-function)

* Find & Replace, including basic `sed`-like, obfuscate, redact, hash, and so on:
[Mask](mask-function), [Eval](eval-function)

* Add GeoIP information to events:
[GeoIP](geoip-function)

* Extract fields:
[Regex Extract](regex-extract-function), [Parser](parser-function)

* Extract timestamps:
[Auto Timestamp](auto-timestamp-function)

* Drop events:
[Drop](drop-function), [Regex Filter](regex-filter-function), [Sampling](sampling-function), [Suppress](suppress-function), [Dynamic Sampling](dynamic-sampling-function)

* Sample events (such as high-volume, low-value data):
[Sampling](sampling-function), [Dynamic Sampling](dynamic-sampling-function)

* Suppress events (such as duplicates):
[Suppress](suppress-function)

* Serialize events to CEF format (send to various SIEMs):
[CEF Serializer](cef-serializer-function)

* Serialize / change format (for example, convert JSON to CSV):
[Serialize](serialize-function)

* Serialize compliant events into SNMP traps:
[SNMP Trap Serialize](snmp-trap-serialize-function)

* Convert JSON arrays into their own events:
[JSON Unroll](json-unroll-function), [XML Unroll](xml-unroll-function)

* Flatten nested structures (for example, nested JSON):
[Flatten](flatten-function)

* Aggregate events in real-time (statistical aggregations):
[Aggregations](aggregations-function)

* Convert events to metrics format:
[Publish Metrics](publish-metrics-function), [Prometheus Publisher (beta)](prometheus-publisher-function)

* Transforms dimensional metrics events into the OTLP (OpenTelemetry Protocol) format: [OTLP Metrics](otlp-metrics-function)

* Resolve hostname from IP address:
[Reverse DNS (beta)](reverse-dns-function)

* Extract numeric values from event fields, converting them to type `number`:
[Numerify](numerify-function)

* Send events out to a command or a local file, via `stdin`, from any point in a Pipeline:
[Tee](tee-function)

* Convert an XML event's elements into individual events:
[XML Unroll](xml-unroll-function)

* Duplicate events in the same Pipeline, with optional added fields:
[Clone](clone-function)

* Break events **within**, instead of [before](event-processing-order) they reach, a Pipeline: [Event Breaker](event-breaker-function)

* Add a text comment within a Pipeline's UI, to label steps without changing event data:
[Comment](comment-function)

> For more usage examples, download Cribl's [Stream Cheat Sheet](https://cribl.io/wp-content/uploads/2020/10/CriblRefCard8.pdf) (Quick Reference Card).
>
{.box .success}

## Function Groups {#groups}

A Function group is a collection of consecutive Functions that can be moved up and down a Pipeline's Functions stack together. Groups help you manage long stacks of Functions by streamlining their display. They are a UI visualization only, purely for visual context, and do not affect the movement of data through Functions. While Functions are in a group, those Functions maintain their global position order in the Pipeline: data moves down the listed Functions, ignoring any grouping assignments.

> Function groups work much like [Route groups](routes#groups).
>
{.box .info}

To build a group from any Function, select the Function's **•••** (Options) menu, then select **Group Actions > Create Group**.

![Creating a group](fn-group-create.e44853737b.png)
{scale="50%"}

You'll need to enter a **Group Name** before you can save or resave the Pipeline. Optionally, enter a  **Description**.

Once you've saved at least one group to a Pipeline, other Functions' **•••** (Options) > **Group Actions** submenus will add options to **Move to Group** or **Ungroup/Ungroup All**.

You can also use a Function's left grab handle to drag it into, or out of, a group. A saved group that's empty displays a dashed target into which you can drag Functions.

![Drag-and-drop target](fn-group-drag.b5fb04d853.png)
{border="true"}
