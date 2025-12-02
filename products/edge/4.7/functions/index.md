# Functions


When events enter a Pipeline, they're processed by a series of Functions. At its core, a Function is code that executes on an event, and it encapsulates the smallest amount of processing that can happen to that event.

The term "processing" means a variety of possible options: string replacement, obfuscation, encryption, event-to-metrics conversions, etc. For example, a Pipeline can be composed of several Functions – one that replaces the term `foo` with `bar`, another one that hashes `bar`, and a final one that adds a field (say, `dc=jfk-42`) to any event that matches `source=='us-nyc-application.log'`.

## How Do They Work 

Functions are atomic pieces of JavaScript code that are invoked on each event that passes through them. To help improve performance, Functions can be configured with [filters](introduction-reference#filters) to further scope their invocation to matching events only. 

You can add as many Functions in a Pipeline as necessary, though the more you have, the longer it will take each event to pass through. Also, you can enable and disable Functions within a Pipeline as necessary. This lets you preserve structure as you optimize or debug.

![Functions stack in a Pipeline](functions-screen.cd84ff15f8.png)
{border="true"}

You can reposition Functions up or down the Pipeline stack to adjust their execution order. Use a Function's left grab handle to drag and drop it into place.

## The `Final` Toggle

The `Final` toggle in Function settings controls what happens to the results of a Function.

When `Final` is toggled to `No` (default), events will be processed by this Function,
**and** then passed on to the next Function below.

When `Final` is toggled to `Yes`, the Function "consumes" the results,
meaning they will not pass down to any other Function below.

A flag in Pipeline view indicates that a Function is `Final`.

![Functions in a Pipeline, some of them have Final flags.](pipeline-final-flag-4.5.png)
{border="true" caption="The Eval and first Drop Functions bearing the Final flag"}

## Functions and Shared-Nothing Architecture  {#shared-nothing}

Cribl Edge is built on a shared-nothing architecture, where each Node and its **Worker Processes operate separately, and process events independently of each other**. This means that all Functions operate strictly in a Worker Process context – state is not shared across processes. 

For example, consider two events that meet a Pipeline's criteria to be aggregated. If these two events arrive on **separate** Workers or Worker Processes, Cribl Edge will **not** aggregate them together. 

This is particularly important to understand for certain Functions that might imply state-sharing, such as [Aggregations](aggregations-function), [Rollup Metrics](rollup-metrics-function), [Dynamic Sampling](dynamic-sampling-function), [Sampling](sampling-function), [Suppress](suppress-function), etc.

If you have a large number of Worker Processes, consider implementing a distributed caching tier, such as [Redis](https://redis.io/docs/about/), to aggregate events across Workers. (See our [Redis Function](redis-function) topic.)

## Out-of-the-Box Functions {#oob}

Cribl Edge ships with several Functions out-of-the-box, and you can chain them together to meet your requirements. For more details, see individual **Functions**, and the **Use Cases** section, within this documentation.

## Accessing Event Fields with `__e` {#special-e}

The special variable `__e` represents the `(context)` event inside a JavaScript expression. Using  `__e` with square bracket notation, you can access any field within the event object, for example,  `__e['hostname']`. Functions use `__e` [extensively](/stream/usecase-code-function#accessing-fields-in-an-event). You also **must** use this notation for fields that contain a special character, like `-`, `.`, or `@`.

## Supported JavaScript Standard

Cribl Edge supports the [ECMAScript® 2015 Language Specification](https://262.ecma-international.org/6.0/).

## Very Large Integer Values
Cribl Edge's JavaScript implementation can safely represent integers only up to the [Number.MAX_SAFE_INTEGER](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER) constant of about 9 quadrillion (precisely, {2^53}‑1). Cribl Edge Functions will round down any integer larger than this, in Data Preview and other contexts. Trailing 0’s might indicate such rounding down of large integers.

## What Functions to Use When

* Add, remove, update fields:
[Eval](eval-function), [Lookup](lookup-function), [Regex Extract](regex-extract-function) 

* Find & Replace, including basic `sed`-like, obfuscate, redact, hash, etc.:
[Mask](mask-function), [Eval](eval-function) 

* Add GeoIP information to events:
[GeoIP](geoip-function) 

* Extract fields:
[Regex Extract](regex-extract-function), [Parser](parser-function)

* Extract timestamps:
[Auto Timestamp](auto-timestamp-function) 

* Drop events:
[Drop](drop-function), [Regex Filter](regex-filter-function), [Sampling](sampling-function), [Suppress](suppress-function), [Dynamic Sampling](dynamic-sampling-function) 

* Sample events (e.g, high-volume, low-value data):
[Sampling](sampling-function), [Dynamic Sampling](dynamic-sampling-function) 

* Suppress events (e.g, duplicates, etc.):
[Suppress](suppress-function) 

* Serialize events to CEF format (send to various SIEMs):
[CEF Serializer](cef-serializer-function)

* Serialize / change format (e.g., convert JSON to CSV):
[Serialize](serialize-function) 

* Convert JSON arrays into their own events:
[JSON Unroll](json-unroll-function), [XML Unroll](xml-unroll-function) 

* Flatten nested structures (e.g., nested JSON):
[Flatten](flatten-function) 

* Aggregate events in real-time (i.e., statistical aggregations):
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

A Function group is a collection of consecutive Functions that can be moved up and down a Pipeline's  Functions stack together. Groups help you manage long stacks of Functions by streamlining their display. They are a UI visualization only: While Functions are in a group, those Functions maintain their global position order in the Pipeline.

> Function groups work much like [Route groups](routes#groups).
>
{.box .info}

To build a group from any Function, click the Function's **•••** (Options) menu, then select **Group Actions > Create Group**. 

![Creating a group](fn-group-create.e44853737b.png)
{scale="50%"}

You'll need to enter a **Group Name** before you can save or resave the Pipeline. Optionally, enter a  **Description**.

Once you've saved at least one group to a Pipeline, other Functions' **•••** (Options) > **Group Actions** submenus will add options to **Move to Group** or **Ungroup/Ungroup All**.

You can also use a Function's left grab handle to drag and drop it into, or out of, a group. A saved group that's empty displays a dashed target into which you can drag and drop Functions.

![Drag-and-drop target](fn-group-drag.b5fb04d853.png)
{border="true"}
