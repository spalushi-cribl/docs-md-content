# Suppress


The Suppress Function either drops or tags specified events over a specified time period, based on evaluation of a key expression.

> Each Worker Process executes this Function independently on its share of events. For details, see [Functions and Shared-Nothing Architecture](functions#shared-nothing).
>
{.box .info}

## Usage

**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

**Key expression**: Suppression key expression used to uniquely identify events to suppress. For example, ``` `${ip}:${port}` ``` will use the fields `ip` and `port` from each event to generate the key.

**Number to allow**: The number of events to allow per time period. Defaults to `1`.

**Suppression period (sec)**: The number of seconds to suppress events after **Number to allow** events are received. Defaults to `30`.

**Drop suppressed events**: Specifies if suppressed events should be dropped, or just tagged with `suppress=1`. Defaults to toggled on, meaning drop.

### Advanced Settings

**Cache size limit**: The maximum number of keys that can be cached before idle entries are removed. Before changing the default `50000`, contact Cribl Support to understand the implications.

**Suppression period timeout**: The number of suppression periods of inactivity before a cache entry is considered idle. This defines a multiple of the **Suppression period (sec)** value. Before changing the default `2`, contact Cribl Support to understand the implications.

**Num events to trigger cache clean-up**: Check cache for idle sessions every N events when cache size exceeds the **Cache size limit**. Before changing the default `10000`, contact Cribl Support to understand the implications.

### Seeing the Results

If you've enabled **Drop suppressed events**, such events will be omitted from logs as they exit this Function. However, the next event allowed through will include a `suppressCount: N` field, whose `N` value indicates the number of events dropped in the preceding **Suppression period**.

![suppressCount shows number of events dropped](suppressCount-field.81a7a4da18.png)

## Examples

In the examples below, **Filter** is the Function-level Filter expression:

1. Suppress by the value of the `host` field:
Filter: `true`
Key expression: `host`
Number to allow: `1`
Suppression period (sec): `30`  <br/>
  Using a datagen sample as a source, generate at least 100 events over 2 minutes.

  **Result**: One event per unique `host` value will be allowed in every 30s. Events without a `host` field will **not** be suppressed.


2. Suppress by the value of the `host` and `port` tuple :
Filter: `true`
Key expression: ``` `${host}:${port}` ```
Number to allow: `1`
Suppression period (sec): `300`

  **Result**: One event per unique `host`:`port` tuple value will be allowed in every 300s.

> Suppression will **also** apply to events without a `host` or a `port` field. The reason is that if `field` is not present, ``` `${field}` ``` results in the literal `undefined`.
>
{.box .warning}

3. To **guarantee** that suppression applies **only** to events with `host` and `port`, check for their presence using a Filter:
  Filter: `host && port!=null`
  Key expression: ``` `${host}:${port}` ```
  Number to allow: `1`
  Suppression period (sec): `300`

4. Decorate events that qualify for suppression:
Filter: `true`
Key expression: ``` `${host}:${port}` ```
Number to allow: `1`
Suppression period (sec): `300`
Drop suppressed events: Toggle off.

  **Result**: No events will be suppressed. But all qualifying events will gain an added field `suppress=1`, which can be used downstream to further transform these events.

> For further use cases, see Cribl's [Streaming Data Deduplication with Cribl](https://cribl.io/blog/streaming-data-deduplication-with-cribl/) blog post.
> For further details on filter usage, see [Build Custom Logic to Route and Process Your Data: Filter Expressions](filter-and-transform-data#filters).
>
{.box .info}
