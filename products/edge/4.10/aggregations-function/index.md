# Aggregations


The Aggregations Function computes summary statistics (like averages, sums, and counts) from event data.

> Each Worker Process executes this Function independently on its share of events. For details, see [Functions and Shared-Nothing Architecture](functions#shared-nothing).
>
{.box .info}

## Usage

**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description about this Function. Defaults to empty.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

**Time window**: The time span of the tumbling window for aggregating events. Must be a valid time string (e.g., `10s`). Must match pattern `\d+[sm]$`.

**Aggregates**: Aggregation function(s) to perform on events. E.g., `sum(bytes).where(action=='REJECT').as(TotalBytes)`. Expression format: `aggFunction(<FieldExpression>).where(<FilterExpression>)​.as(<outputField>)`. See more examples below. 

> When used without `as()`, the aggregate's output will be placed in a field labeled `<fieldName>_<aggFunction>`. If there are conflicts, the last aggregate wins. For example, given two aggregates – `sum(bytes).where(action=='REJECT')` and `sum(bytes)`  – the latter one (`bytes_sum`) is the winner.
{.box .info}

**Group by fields**: Fields to group aggregates by. Supports wildcard expressions.
> Using the wildcard character `*` causes causes all fields in the event to be included in the aggregation, which can result in high cardinality and increased memory usage. To avoid this impact, first exclude fields that can result in high cardinality, **then** introduce wildcards, if any. 
> For example: `!_time, !_numericValue, *`
{.box .warning}

**Evaluate fields**: Set of key-value pairs to evaluate and add/set. Fields are added in the context of an aggregated event, before they’re sent out. Does not apply to passthrough events.

### Time Window Settings  {#tws}

**Cumulative aggregations**: Toggle on to retain aggregations for cumulative aggregations when flushing out an aggregation table event. Toggle off (default) to reset aggregations to `0` on flush.

**Lag tolerance**: The lag tolerance represents the tumbling window tolerance to late events. Must be a valid time string (e.g., `10s`). Must match pattern `\d+[sm]$`.

**Idle bucket time limit**: The amount of time to wait before flushing a bucket that has not received events. Must be a valid time string (e.g., `10s`). Must match pattern `\d+[sm]$`.

### Output Settings

**Passthrough mode**: Toggle on to pass through the original events along with the aggregation events. Default is toggled off.

**Metrics mode**: Determines whether to output aggregates as metrics. Default is toggled off, causing aggregates to be output as events.

**Sufficient stats mode**: Determines whether to output *only* statistics sufficient for the supplied aggregations. Default is toggled off, meaning output richer statistics.

**Preserve group by fields**: Determines whether to preserve the structure of the original aggregation event's group by fields.

**Output prefix**: A prefix that is prepended to all of the fields output by this Aggregations Function.

### Advanced Settings

**Aggregation event limit**: The maximum number of events to include in any given aggregation event. Defaults to unlimited. Must be at least `1`.

**Aggregation memory limit**: The maximum memory usage for aggregations within each Worker Process. This limit is applied per process, not globally. By default, it's unlimited, allowing the use of all available system memory. Accepts numerals with multiple-byte units like KB, MB, GB (for example, `4GB`).

**Flush on stream close**: Toggle on (default) if you want aggregations to flush when an input stream is closed. If toggled off, the [Time Window Settings](#tws) will control flush behavior; this can be preferable in cases like the following: 
- Your input data consists of many small files.
- You are sending data to Prometheus. Enabling **Flush on stream close** can send Prometheus multiple aggregations from the same Worker Process for the same time period. Prometheus cannot tell the multiple aggregations apart, and will ingest only the first one.

## Aggregation Functions

- `avg(expr:FieldExpression)`: Returns the average `expr` values.
- `count(expr:FieldExpression)`: If `expr` is omitted, returns the number of events received over the time window. If `expr` is given, returns the number of events seen where `expr` evaluates to a value other than null or undefined. For example, with these three events in the same time window:
   - `{ _time: 20, _val: 5 }`
   - `{ _time: 22 }`
   - `{ _time: 24, _val: null }`
   
   `count()` would return `3`, while `count(_val)` would return `1`.
- `dc(expr: FieldExpression, errorRate: number = 0.01)`: Returns the estimated number of distinct values of `expr`, within a relative error rate. Lower error rates increase the accuracy of the result, at the cost of using more memory. For example, with these three events in the same time window:
   - `{ _time: 20, _name: 'Alice' }`
   - `{ _time: 22, _name: 'Bob' }`
   - `{ _time: 24, _name: 'Alice' }`
      
   `dc(_name)` would return `2`.
- `distinct_count(expr: FieldExpression, errorRate: number = 0.01)`: Identical to `dc(expr)`.
- `earliest(expr:FieldExpression)`: Returns the earliest (based on `_time`) observed `expr` value.
- `first(expr:FieldExpression)`: Returns the first observed `expr` value.
- `histogram(expr:FieldExpression, buckets: number[])`. Returns the average of the values of `expr` and generates a field with the same name as the aggregated output field, suffixed with `_data`.
  - `buckets` must contain at least one numeric value.
  
  The `_data` field in the output contains three pieces of information:
  - `_sum` – the sum of all values of `expr`.
  - `_count` –  the number of events seen where `expr` evaluates to a value other than null or undefined.
  - `_buckets` – an object whose keys are the buckets defined in the function, and whose values are the number of values that fall in that histogram bucket. In addition to the buckets defined by the user, the object will include a bucket labeled `Infinity`, into which all values will be placed.
  A value of `x` falls into a histogram bucket if it is less than or equal to the value of the bucket. A value of `45` would fall into a bucket with value `50`, but not one with value `40`. Counts are cumulative; all values counted in a bucket will also be counted in every bucket larger than it.
  For example, with these three events in the same time window:
      - `{ _time: 20, age: 20 }`
      - `{ _time: 22, age: 25 }`
      - `{ _time: 24, age: 30 }`

      `histogram(age, [10, 25, 40])` would result in the following on the output event:
      ```json
      {
        age_histogram: 25
        age_histogram_data: {
          _count: 3,
          _sum: 75
          _buckets: {
            10: 0,
            25: 2,
            40: 3,
            Infinity: 3
          }
        }
      }
      ```
- `last(expr:FieldExpression)`: Returns the last observed `expr` value.
- `latest(expr:FieldExpression)`: Returns the latest (based on `_time`) observed `expr` value.
- `list(expr:FieldExpression[, max:number, excludeNulls: boolean = true])`: Returns a list of the observed values of `expr`.
   - Optional `max` parameter limits the number of values returned. If omitted, the default is `100`. If set to `0`, will return all values.
   - Optional `excludeNulls` boolean excludes null and undefined values from results. If included, defaults to `true`.
- `max(expr:FieldExpression)`: Returns the maximum `expr` value.
- `median(expr:FieldExpression)`: Returns the middle value of the sorted parameter.
- `min(expr:FieldExpression)`: Returns the minimum `expr` value.
- `mode(expr: FieldExpression[, excludeNulls: boolean = true])`: Returns the single most frequently encountered `expr` value.
   - Optional `excludeNulls` boolean excludes null and undefined values from results. If included, defaults to `true`.
- `per_second(expr:FieldExpression)`: Returns the rate of change of the values of `expr` over the aggregate time window. This is equivalent to `rate(expr, '1s')`. For example, with these three events in the same time window:
   - `{ _time: 20, _val: 5 }`
   - `{ _time: 22, _val: 15 }`
   - `{ _time: 24, _val: 35 }`
   
   `per_second(_val)` would give back `(35 - 5) / (24 - 20) = 7.5`, meaning that `_val` increased by 7.5 every second over the time window.
- `perc(level: number, expr: FieldExpression)`: Returns `<level>` percentile value of the numeric `expr` values.
- `rate(expr:FieldExpression, timeString: string = '1s')`: Returns the rate of change of the values of `expr` over the aggregate time window. Calculated as (latest value - earliest value) / (latest timestamp - earliest timestamp) * number of seconds in `timeString`. For example, with these three events in the same time window:
   - `{ _time: 20, _val: 5 }`
   - `{ _time: 22, _val: 15 }`
   - `{ _time: 24, _val: 35 }`
      
   `rate(_val, '2s')` would give back `(35 - 5) / (24 - 20) * 2 = 15`, meaning that `_val` increased by 15 every 2 seconds over the time window.
- `stdev(expr:FieldExpression)`: Returns the sample standard deviation of the `expr` values.
- `stdevp(expr:FieldExpression)`: Returns the population standard deviation of the `expr` values.
- `sum(expr:FieldExpression)`: Returns the sum of the `expr` values.
- `summary(expr:FieldExpression)[, quantiles: number[]])`: Returns the average of the `expr` values and generates a field with the same name as the aggregate output field, suffixed with `_data`. 
   - Optional: `quantiles` values must be between `0` and `1` inclusive.
   
   The `_data` field in the output contains three pieces of information:
   - `_sum` – the sum of all `expr` values.
   - `_count` – the number of events seen where `expr` evaluates to a value other than null or undefined.
   - `_quantiles` – an object whose keys are the quantiles defined in the function, and whose values are the quantile value across the `expr` values. For example, a quantile key `0.5` and value `500` would indicate that the 50th percentile (or median) of the values seen was 500.
   - For example, with these three events in the same time window:
      - `{ foo: 100 }`
      - `{ foo: 200 }`
      - `{ foo: 300 }`
         
      `summary(foo, [0.25, 0.5, 0.75])` would result in the following on the output event:
      ```json
      {
        foo_summary: 25
        foo_summary_data: {
          _count: 3,
          _sum: 75
          _quantiles: {
            0.25: 100,
            0.5: 150,
            0.75: 225
          }
        }
      }
      ```
- `sumsq(expr:FieldExpression)`: Returns the sum of squares of the `expr` values.
- `top(expr: FieldExpression, count: number[, excludeNulls: boolean = true])`: Returns the most frequently encountered `expr` values, up to  `count` number of results.
   - `count` parameter must be a positive integer.
   - Optional `excludeNulls` boolean excludes null and undefined values from results. If included, defaults to `true`.
- `values(expr:FieldExpression[, max: number, excludeNulls: boolean = true, errorRate: number])`: Returns a list of distinct `expr` values.
   - Optional `max` parameter limits the number of values returned; if omitted, the default is `0`, meaning return all distinct values.
   - Optional `excludeNulls` boolean excludes null and undefined values from results. If included, defaults to `true`.
   - Optional `errorRate` parameter controls how accurately the function counts “distinct” values. Range is `0`–`1`; if omitted, the default value is `0.01`. Higher values allow higher error rates (fewer unique values recognized), with the offsetting benefit of less memory usage.
- `variance(expr:FieldExpression)`: Returns the sample variance of the `expr` values.
- `variancep(expr:FieldExpression)`: Returns the population variance of the `expr` values.

## Safeguarding Data

Upon shutdown, Cribl Edge will attempt to flush the buffers that hold aggregated data, to avoid data loss. If you set a **Time window** greater than 1 hour, Cribl recommends adjusting the **Aggregation memory limit** and/or **Aggregation event limit** to prevent the system from running out of memory. 

This is especially necessary for high-cardinality data. (Both settings default to unlimited, but we recommend setting defined limits based on testing.)

## How Do Time Window Settings Work?

### Lag Tolerance

As events are aggregated into windows, there is a good chance that most will arrive later than their event time. For instance, given a `10s` window (`10:42:00 - 10:42:10`), an event with timestamp `10:42:03` might come in 2 seconds later at `10:42:05`. 

In several cases, there will also be late, or lagging, events that will arrive **after** the latest time window boundary. For example, an event with timestamp `10:42:04` might arrive at `10:42:12`. Lag Tolerance is the setting that governs how long to wait – after the latest window boundary – and still accept late events.

![](cribl-aggregations-lag.8cc785d1c6.png)
{scale="60%"}

The "bucket" of events is said to be in Stage 1, where it's still accepting new events, but it's not yet finalized. Notice how in the third case, an event with event time `10:42:09` arrives 1 second past the window boundary at `10:42:11`, but it's still accepted because it happens before the lag time expires.  
 
After the lag time expires, the bucket moves to Stage 2. 

![](cribl-aggregations-idle.4d570aa19b.png)
{scale="60%"}

If the bucket is created from a historic stream, then the bucket is initiated in Stage 2. Lag time is not considered. A "historic" stream is one where the latest time of a bucket is before `now()`. E.g., if the window size is 10s, and `now()=10:42:42`, an event with `event_time=10` will be placed in a Stage 2  bucket with range `10:42:10 - 10:42:20`.

### Idle Bucket Time Limit

While Lag Tolerance works with event time, Idle Bucket Time Limit works on arrival time (i.e., real time). It is defined as the amount of time to wait before flushing a bucket that has not received events.

![](cribl-aggregations-stage3.50fb31e960.png)
{scale="60%"}

After the Idle Time limit is reached, the bucket is "flushed" and sent out of the system.

## Examples

Assume we're working with VPC Flowlog events that have the following structure: 

`version account_id interface_id srcaddr dstaddr srcport dstport protocol packets bytes start end action log_status`

For example: 

`2 99999XXXXX eni-02f03c2880e4aaa3 10.0.1.70 10.0.1.11 9999 63030 6 6556 262256 1554562460 1554562475 ACCEPT OK`
`2 496698360409 eni-08e66c4525538d10b 37.23.15.38 10.0.2.232 4373 8108 6 1 52 1554562456 1554562466 REJECT OK`

### Scenario A:

Every 10s, compute sum of `bytes` and output it in a field called `TotalBytes`.

Time Window:  `10s`
Aggregations: `sum(bytes).as(TotalBytes)`

### Scenario B: 

Every 10s, compute sum of `bytes`, output it in a field called `TotalBytes`, group by `srcaddr`.

Time Window:  `10s`
Aggregations: `sum(bytes).as(TotalBytes)`
Group by fields: `srcaddr`

### Scenario C: 

Every 10s, compute sum of `bytes` but only where action is `REJECT`, output it in a field called `TotalBytes`, group by `srcaddr`.

Time Window:  `10s`
Aggregations: `sum(bytes).where(action=='REJECT').as(TotalBytes)`
Group by Fields: `srcaddr`

### Scenario D: 

Every 10s, compute sum of `bytes` but only where action is `REJECT`, output it in a field called `TotalBytes`. Also, compute distinct count of `srcaddr`.

Time Window:  `10s`
Aggregations:   
 `sum(bytes).where(action=='REJECT').as(TotalBytes)`  
  `distinct_count(srcaddr).where(action=='REJECT')`


> For further examples, see [Engineering Deep Dive: Streaming Aggregations Part 2 – Memory Optimization](https://cribl.io/blog/engineering-deep-dive-streaming-aggregations-part-2-memory-optimization).
>
{.box .info}
