# Event Breakers


Event Breakers help break incoming streams of data into discrete events. To see how Event Breakers interact with the rest of Cribl Edge's data flow, see [Event Processing Order](event-processing-order).

## Access Event Breakers {#intro}

To access the Event Breakers management interface navigate to **Products** > **Cribl Edge** > **Fleets**. Select a Fleet, then go to **More** > **Knowledge** > **Event Breaker Rulesets**. On the resulting **Event Breaker Rulesets** page, you can edit, add, delete, search, and tag Event Breaker rules and rulesets, as necessary.

![Event Breaker Rulesets page](cribl-breakers.png)
{border="true" scale="90%"}

Event Breakers are also supported in [Packs](packs).

## Limitations

Event Breakers are directly accessible only on Sources that require incoming events to be broken into a better-defined format. (Check individual Cribl Edge Sources' documentation for Event Breaker support.)

However, you can instead use the [Event Breaker Function](event-breaker-function) in Pipelines that process data from unsupported Sources.

> Validate event breaking before turning on a Source.
>
{.box .info}

## Event Breaker Rulesets

Rulesets are **collections of Event Breaker rules** that are associated with [Sources](sources). Rules define configurations needed to break down a stream of data into events.

![Rules within an example (AWS) ruleset that ships with Cribl Edge](breaker-rules-4.0.ed106b6cbf.png)
{border="true"}

Rules within a ruleset are ordered and evaluated top‑>down. One or more rulesets can be associated with a Source, and these rulesets are also evaluated top‑>down. For a stream from a given Source, the first matching rule goes into effect.

``` {title="Rulesets and Rules - Ordered"}
Ruleset A
  Rule 1
  Rule 2
  ...
  Rule n

...

Ruleset B
  Rule Foo
  Rule Bar
  ...
  Rule FooBar
```

An example of multiple rulesets associated with a Source:

![Three Event Breaker rulesets added to a Source](cribl-breakers-on-sources.2e6b68605d.png)
{border="true"}

### Rule Example

This rule breaks on newlines and uses Manual timestamping **after** the sixth comma, as indicated by this pattern: `^(?:[^,]*,){6}`.

![An Event Breaker rule](se-cribl-breaker-rule-3.5-4101.png)
{border="true"}

### System Default Rule

The system default rule functionally sits at the bottom of the ruleset/rule hierarchy (but is built-in and not displayed on the Event Breakers page), and goes into effect if there are no matching rules:

- Filter Condition defaults to `true`
- Event Breaker to `[\n\r]+(?!\s)`
- Timestamp anchor to `^`
- Timestamp format to `Auto` and a scan depth of `150` bytes
- Max Event Bytes to `51200`
- Default Timezone to `Local`

## How Do Event Breakers Work  {#add}

On the **Event Breaker Rulesets** page (see screenshot [above](#intro)), select **Add Ruleset** to create a new Event Breaker ruleset. Select **Add Rule** within a ruleset to add a new Event Breaker.

![Adding a new Event Breaker rule](se-event-breaker-rule-modal-3.5-4101.png)
{border="true"}

Each Event Breaker includes the following components, which you configure from top to bottom in the above **Event Breaker Rules** modal:

### Filter Condition

When data enters the system, the engine checks it against each filter expression in sequence. If the expression evaluates to `true`, the rule configurations engage for the entire duration of that stream. Otherwise, the engine evaluates the next rule down the line. Different Source types define a "stream" differently:

- For TCP Sources, a stream equates to a single incoming TCP connection. The stream begins when the socket connection is established and ends when that connection closes.
- For HTTP Sources, a stream equates to a single HTTP request, with connections potentially being reused across different requests instead of corresponding to the entire connection lifecycle.
- For file-based sources like File Monitor or Journal Files, each ingested file is treated as a distinct and complete stream, evaluated independently against filters.
- For Sources that support multiplexed data (like Splunk S2S protocol), Cribl Edge demultiplexes based on channels. Each channel (typically representing a unique source+sourcetype+host combination in Splunk Sources) will have its own event breaker applied consistently throughout its lifecycle.

The "stickiness" of the first `true` expression can lead to some unintended behavior. For example, if an [S3 Source](/stream/sources-s3) ingests an archive file that contains multiple files in different formats – where each file requires its own unique Event Breaker – events will not be broken correctly.

### Event Breaker Type

After a breaker pattern has been selected, it will apply on the stream **continuously**. See below for specific information on different [Event Breaker Types](#types).

### Timestamp Settings {#timestamp-settings}

Cribl Edge timestamps events after they are synthesized from streams, ensuring accurate time representation for event processing and analysis.

- If an Event Breaker has set an event's `_time` to the current time – rather than extracting the value from the event itself – it will mark this by adding the internal field `__timestampExtracted: false` to the event.
- Cribl Edge truncates timestamps to three-digit (milliseconds) resolution, omitting trailing zeros.

**Anchor**: Cribl Edge first identifies a timestamp anchor within the event. This anchor serves as the starting point for timestamp extraction. The end of the first regex match is the start of the timestamp.
  > The closer an anchor is to the timestamp pattern, the better the performance and accuracy – especially if multiple timestamps exist within an event.
  {.box .success}

  ![Anchors preceding timestamps](cribl-streams-breakers.f6c46f6c82.png)
  {border="true"}

**Format**: Starting from the anchor, the engine attempts the following methods, stopping after a successful timestamp extraction:

- **Autotimestamp**: Scans up to a configurable depth within the event, attempting to automatically identify and parse a timestamp. This uses the same algorithm as the [Auto Timestamp](auto-timestamp-function) Function and the [`C.Time.timestampFinder()`](expressions-time#time-finder) native method.
- **Manual**: Select a `strptime` format to parse the timestamp immediately following the anchor. The anchor must lead the engine **right before** the timestamp pattern begins.
- **Current time**: The event is timestamped with the current time.

**Default timezone**: Assigns a timezone to timestamps that lack timezone information.

**Earliest timestamp allowed** and **Future timestamp allowed**: Constrain the parsed timestamp to prevent unrealistic values, setting timestamps outside these bounds to the default time.

### Add Fields to Events {#fields}

After events have been timestamped, one or more fields can be added here as key-value pairs. In each field's **Value Expression**, you can fully evaluate the field value using JavaScript expressions.

> Event Breakers **always** add the `cribl_breaker` field to output events. Its value is the name of the chosen ruleset. (Some examples below omit the `cribl_breaker` field for brevity, but in real life the field is always added.)
>
{.box .info}

### Max Event Bytes {#bytes}

If an event's final broken chunk reaches a matched rule's **Max event bytes** length, it will be broken again.

> The highest **Max Event Bytes** value that you can set is about 128 MB (`134217728` bytes). Events exceeding that size will be split into separate events, but left unbroken. Cribl Edge will set these events' `__isBroken` internal field to `false`.
>
{.box .warning}

## Event Breaker Types {#types}

Several types of Event Breaker can be applied to incoming data streams:

- [Regex](#regex)
- [File Header](#header)
- [JSON Array](#array)
- [JSON New Line Delimited](#newline)
- [Timestamp](#timestamp)
- [CSV](#csv)

### Regex {#regex}

The Regex breaker uses regular expressions to find breaking points in data streams.

After a breaker regex pattern has been selected, it will apply on the stream **continuously**. Breaking will occur at the beginning of the match, and the matched content will be consumed/thrown away. If necessary, you can use a positive lookahead regex to keep the content, such as `(?=pattern)`

Capturing groups are **not allowed** to be used anywhere in the Event Breaker pattern, as they will further break the stream – which is often undesirable. Breaking will also occur if the final broken event's length reaches the rule's **Max event bytes**.

#### Example

Break after a newline or carriage return, but only if followed by a timestamp pattern: <br/>
   **Event Breaker:** `[\n\r]+(?=\d+-\d+-\d+\s\d+:\d+:\d+)`

``` {title="Sample Event - Multiline"}
--- input ---
2020-05-19 16:32:12 moen3628 ipsum[5213]: Use the mobile TCP feed, then you can program the auxiliary card!
   Try to connect the FTP sensor, maybe it will connect the digital bus!
   Try to navigate the AGP panel, maybe it will quantify the mobile alarm!
2020-05-19 16:32:12 moen3628 ipsum[5213]: Use the mobile TCP feed, then you can program the auxiliary card!
   Try to connect the FTP sensor, maybe it will connect the digital bus!
   Try to navigate the AGP panel, maybe it will quantify the mobile alarm!


--- output event 1 ---
{
  "_raw": "2020-05-19 16:32:12 moen3628 ipsum[5213]: Use the mobile TCP feed, then you can program the auxiliary card! \n   Try to connect the FTP sensor, maybe it will connect the digital bus!\n   Try to navigate the AGP panel, maybe it will quantify the mobile alarm!",
  "_time": 1589920332
}

--- output event 2 ---
{
  "_raw": "2020-05-19 16:32:12 moen3628 ipsum[5213]: Use the mobile TCP feed, then you can program the auxiliary card!\n   Try to connect the FTP sensor, maybe it will connect the digital bus!\n   Try to navigate the AGP panel, maybe it will quantify the mobile alarm!",
  "_time": 1589920332
}
```

### File Header {#header}

You can use the File Header breaker to break files with headers, such as IIS or Bro logs. This type of breaker relies on a header section that lists field names. The header section is typically present at the top of the file, and can be single-line or greater.

After the file has been broken into events, fields will also be extracted, as follows:

- **Header Line**: Regex matching a file header line. For example, `^#`.
- **Field Delimiter**: Field delimiter regex. For example, `\s+`.
- **Field Regex**: Regex with one capturing group, capturing all the fields to be broken by field delimiter. For example, `^#[Ff]ields[:]?\s+(.*)`
- **Null Values**: Representation of a null value. Null fields are not added to events.
- **Clean Fields**: Whether to clean up field names by replacing non `[a-zA-Z0-9]` characters with `_`.

#### Example

Using the values above, let's see how this sample file breaks up:

``` {title="Sample Event - File Header"}
--- input ---
#fields ts      uid     id.orig_h       id.orig_p       id.resp_h       id.resp_p       proto
#types  time    string  addr    port    addr    port    enum
1331904608.080000       -     192.168.204.59  137     192.168.204.255 137     udp
1331904609.190000       -     192.168.202.83  48516   192.168.207.4   53      udp


--- output event 1 ---
{
  "_raw": "1331904608.080000       -     192.168.204.59  137     192.168.204.255 137     udp",
  "ts": "1331904608.080000",
  "id_orig_h": "192.168.204.59",
  "id_orig_p": "137",
  "id_resp_h": "192.168.204.255",
  "id_resp_p": "137",
  "proto": "udp",
  "_time": 1331904608.08
}

--- output event 2 ---
{
  "_raw": "1331904609.190000       -     192.168.202.83  48516   192.168.207.4   53      udp",
  "ts": "1331904609.190000",
  "id_orig_h": "192.168.202.83",
  "id_orig_p": "48516",
  "id_resp_h": "192.168.207.4",
  "id_resp_p": "53",
  "proto": "udp",
  "_time": 1331904609.19
}
```

### JSON Array {#array}

Use the JSON Array Event Breaker to split a single, large JSON object containing a nested array of records into multiple, individual events. This is particularly useful for logs from services like AWS CloudTrail, EKS CloudWatch, or Google Cloud Audit Logs, where a single log entry contains an array of event records.

> If a Source supports Event Breakers (such as AWS Sources), it is much more efficient to [unroll JSON](json-unroll-function) in an Event Breaker rather than a Pipeline Function.
>
{.box .info}

The JSON Array Event Breaker has these settings:

- **Array field:** Optional path to array in a JSON event with records to extract. For example, `Records`.
- **Parent fields to copy:** This setting allows you to retain key metadata from the original parent event and include it with each of the extracted child events. By default, only the extracted events from the array are passed down the Pipeline. For example, you can use this to copy `host`, `source`, `region`, or other contextual information that is not contained within the array itself.<br><br>

  To specify which fields to copy, enter a comma-separated list of top-level field names. You can also use `*` as a wildcard to copy all top-level fields. Be aware that:

  - This setting only supports copying fields from the top level of the JSON document. Nested fields are not supported.
  - The `Array field` you specify to be unrolled is always excluded from the copied fields. This prevents redundant data.
  - Enclose field names containing special characters (such as `my-field`) in single or double quotes.

- **JSON extract fields:** This toggle controls whether Cribl Edge parses the JSON array and automatically extracts key-value pairs from each event:

  - When enabled, Cribl Edge will fully parse each JSON object and extract all fields (such as `user_id`, `status`, `event_type`). This is the ideal setting for sending data to Destinations that need structured data for analysis like Splunk or Elasticsearch for searching, filtering, and dashboard-building. When this toggle is on, it exposes the **Timestamp field** setting.
  - When disabled, Cribl Edge will not extract any fields from the JSON objects. Only the `_raw` (the full event text) and `time` will be available. Choose this option if your primary goal is to send raw data for archival purposes with the highest possible throughput.

  > The **JSON extract fields** toggle impacts the **Timestamp format** setting in the [Timestamp Settings](#timestamp-settings). If the **JSON extract fields** toggle is off, do not select the **Manual format** option. If the toggle is on, you can select any option.
  >
  {.box .warning}

- **Timestamp field:** This optional setting appears when **JSON extract fields** is toggled on. This allows you to specify a path to the timestamp field within each extracted event. For example, `eventTime` or `level1.level2.eventTime`. Cribl Edge will use this value to set the event's `_time` field.

- **Max event bytes**: The maximum size (in number of bytes) that an event can be before being flushed to the Pipelines.

- **Event byte limit**: The maximum size (in number of bytes) that an event can be before being flushed to the Pipelines.

#### Configuration Examples {#raw-json-array}

The following is an example of data input before the JSON Array Event Breaker processes it:

```json {title="Example raw input - JSON Document (Array)"}
{
  "someField": 42,
  "someArray": [1, 2, 3],
  "myObjectArray": [
    {"one": 1, "two": 2},
    {"three": 3, "four": 4}
  ],
  "Records": [
    { "eventVersion": "1.0" },
    { "eventVersion": "2.0" }
  ],
  "myObject": {
    "someField": {
      "test1": 1,
      "test2": "two"
    },
    anotherArray: [
      {"id": 1},
      {"id": 2}
    ]
  }
}
```

**Example 1 - Copy All Top-Level Fields**

This first example shows how to use the wildcard `*` to copy all top-level fields from the original JSON event to each of the new, extracted events.

{{% tabs "json" "json" "JSON Configuration" "ui" "User Interface Configuration"%}}
{{% tab-item "json" %}}
```json
{
  "parentFieldsToCopy": ["*"],
  "jsonArrayField": "Records"
}
```
{{% /tab-item %}}
{{% tab-item "ui" %}}
- **Parent fields to copy:** `*`
- **Array field:** `Records`
{{% /tab-item %}}
{{% /tabs %}}

<br>

With these configurations, the [original raw data](#raw-json-array) will have this output:

```json {title="Example 1 - Copy All Top-Level Fields"}
[
  {
    _raw: "{ "eventVersion": "1.0" }",
    "someField": 42,
    "someArray": [1, 2, 3],
    "myObjectArray": [
      {"one": 1, "two": 2},
      {"three": 3, "four": 4}
    ],
    "myObject": {
      "someField": {
        "test1": 1,
        "test2": "two"
      },
      anotherArray: [
        {"id": 1},
        {"id": 2}
      ]
    }
  },
  {
    _raw: "{ "eventVersion": "2.0" }",
    "someField": 42,
    "someArray": [1, 2, 3],
    "myObjectArray": [
      {"one": 1, "two": 2},
      {"three": 3, "four": 4}
    ],
    "myObject": {
      "someField": {
        "test1": 1,
        "test2": "two"
      },
      anotherArray: [
        {"id": 1},
        {"id": 2}
      ]
    }
  }
]
```

**Example 2 - Copy Specific Fields**

This example shows how to select only specific top-level fields to copy. By providing a comma-separated list of field names, you can control which metadata carries over to the extracted events, resulting in smaller events and reduced data volume.

{{% tabs "json" "json" "JSON Configuration" "ui" "User Interface Configuration"%}}
{{% tab-item "json" %}}
```json
{
  "parentFieldsToCopy": ["someField", "myObject"],
  "jsonArrayField": "Records"
}
```
{{% /tab-item %}}
{{% tab-item "ui" %}}
- **Parent fields to copy:** `"someField"`, `"myObject"`
- **Array field:** `Records`
{{% /tab-item %}}
{{% /tabs %}}

<br>

With these configurations, the [original raw data](#raw-json-array) will have this output:

```json {title="Example 2 - Copy Specific Fields"}
[
  {
    _raw: "{ "eventVersion": "1.0" }",
    "someField": 42,
    "myObject": {
      "someField": {
        "test1": 1,
        "test2": "two"
      },
      anotherArray: [
        {"id": 1},
        {"id": 2}
      ]
    }
  },
  {
    _raw: "{ "eventVersion": "2.0" }",
    "someField": 42,
    "myObject": {
      "someField": {
        "test1": 1,
        "test2": "two"
      },
      anotherArray: [
        {"id": 1},
        {"id": 2}
      ]
    }
  }
]
```

**Example 3 - Use a Wildcard with a Prefix**

You can also use a wildcard with a prefix to copy fields that share a common name. This example uses `some*` to copy any top-level fields that start with "some," such as `someField` and `someArray`. This is useful when you have a consistent naming convention for related fields.

{{% tabs "json" "json" "JSON Configuration" "ui" "User Interface Configuration"%}}
{{% tab-item "json" %}}
```json
{
  "parentFieldsToCopy": ["some*"],
  "jsonArrayField": "Records"
}
```
{{% /tab-item %}}
{{% tab-item "ui" %}}
- **Parent fields to copy:** `"some*"`
- **Array field:** `Records`
{{% /tab-item %}}
{{% /tabs %}}

<br>

With these configurations, the [original raw data](#raw-json-array) will have this output:

```json {title="Example 3 - Use a Wildcard with a Prefix"}
[
  {
    _raw: "{ "eventVersion": "1.0" }",
    "someField": 42,
    "someArray": [1, 2, 3]
  },
  {
    _raw: "{ "eventVersion": "2.0" }",
    "someField": 42,
    "someArray": [1, 2, 3]
  }
]
```

**Example 4 - Unroll a Nested Array**

This example demonstrates how to extract events from an array nested inside another object, as defined by the `jsonArrayField` path `myObject.anotherArray`. In this scenario, Cribl Edge excludes the entire top-level parent object (`myObject`) from the extracted events. The other top-level fields are copied to the new events.

{{% tabs "json" "json" "JSON Configuration" "ui" "User Interface Configuration"%}}
{{% tab-item "json" %}}
```json
{
  "parentFieldsToCopy": ["*"],
  "jsonArrayField": "myObject.anotherArray"
}
```
{{% /tab-item %}}
{{% tab-item "ui" %}}
- **Parent fields to copy:** `*`
- **Array field:** `myObject.anotherArray`
{{% /tab-item %}}
{{% /tabs %}}

<br>

With these configurations, the [original raw data](#raw-json-array) will have this output:

```json {title="Example 4 - Unroll a Nested Array"}
[
  {
    "_raw": {"id": 1},
    "someField": 42,
    "someArray": [1, 2, 3],
    "myObjectArray": [
      {"one": 1, "two": 2},
      {"three": 3, "four": 4}
    ],
    "Records": [
      { "eventVersion": "1.0" },
      { "eventVersion": "2.0" }
    ]
  },
  {
    "_raw": {"id": 2},
    "someField": 42,
    "someArray": [1, 2, 3],
    "myObjectArray": [
      {"one": 1, "two": 2},
      {"three": 3, "four": 4}
    ],
    "Records": [
      { "eventVersion": "1.0" },
      { "eventVersion": "2.0" }
    ]
  }
]
```

### JSON New Line Delimited {#newline}

You can use the JSON  New Line Delimited breaker to break and extract fields in newline-delimited JSON streams.

Example

Using default values, let's see how this sample stream breaks up:

``` {title="Sample Event - Newline Delimited JSON Breaker"}
--- input ---
{"time":"2020-05-25T18:00:54.201Z","cid":"w1","channel":"clustercomm","level":"info","message":"metric sender","total":720,"dropped":0}
{"time":"2020-05-25T18:00:54.246Z","cid":"w0","channel":"clustercomm","level":"info","message":"metric sender","total":720,"dropped":0}


--- output event 1 ---
{
  "_raw": "{\"time\":\"2020-05-25T18:00:54.201Z\",\"cid\":\"w1\",\"channel\":\"clustercomm\",\"level\":\"info\",\"message\":\"metric sender\",\"total\":720,\"dropped\":0}",
  "time": "2020-05-25T18:00:54.201Z",
  "cid": "w1",
  "channel": "clustercomm",
  "level": "info",
  "message": "metric sender",
  "total": 720,
  "dropped": 0,
  "_time": 1590429654.201,
}

--- output event 2 ---
{
  "_raw": "{\"time\":\"2020-05-25T18:00:54.246Z\",\"cid\":\"w0\",\"channel\":\"clustercomm\",\"level\":\"info\",\"message\":\"metric sender\",\"total\":720,\"dropped\":0}",
  "time": "2020-05-25T18:00:54.246Z",
  "cid": "w0",
  "channel": "clustercomm",
  "level": "info",
  "message": "metric sender",
  "total": 720,
  "dropped": 0,
  "_time": 1590429654.246,
}
```

### Timestamp {#timestamp}

You can use the Timestamp breaker to break events at the beginning of any line in which Cribl Edge finds a timestamp. This type enables breaking on lines whose timestamp pattern is not known ahead of time.

Example

Using default values, let's see how this sample stream breaks up:

``` {title="Sample Event - Timestamp Based Breaker"}
--- input ---
{"level":"debug","ts":"2021-02-02T10:38:46.365Z","caller":"sdk/sync.go:42","msg":"Handle ENIConfig Add/Update: us-west-2a, [sg-426fdac8e5c22542], subnet-42658cf14a98b42"}
{"level":"debug","ts":"2021-02-02T10:38:56.365Z","caller":"sdk/sync.go:42","msg":"Handle ENIConfig Add/Update: us-west-2a, [sg-426fdac8e5c22542], subnet-42658cf14a98b42"}


--- output event 1 ---
{
  "_raw": "{\"level\":\"debug\",\"ts\":\"2021-02-02T10:38:46.365Z\",\"caller\":\"sdk/sync.go:42\",\"msg\":\"Handle ENIConfig Add/Update: us-west-2a, [sg-426fdac8e5c22542], subnet-42658cf14a98b42\"}",
  "_time": 1612262326.365
}

--- output event 2 ---
{
  "_raw": "{\"level\":\"debug\",\"ts\":\"2021-02-02T10:38:56.365Z\",\"caller\":\"sdk/sync.go:42\",\"msg\":\"Handle ENIConfig Add/Update: us-west-2a, [sg-426fdac8e5c22542], subnet-42658cf14a98b42\"}",
  "_time": 1612262336.365
}
```

### CSV {#csv}

The CSV breaker extracts fields in CSV streams that include a header line. Selecting this type exposes these extra fields:

- **Delimiter**: Delimiter character to use to split values. Defaults to: `,`
- **Quote Char**: Character used to quote literal values. Defaults to: `"`
- **Escape Char**: Character used to escape the quote character in field values. Defaults to: `"`

  **Example**: Using default values, let's see how this sample stream breaks up:

#### Example

Using default values, let's see how this sample stream breaks up:

``` {title="Sample Event - CSV Breaker"}
--- input ---
time,host,source,model,serial,bytes_in,bytes_out,cpu
1611768713,"myHost1","anet","cisco","ASN4204269",11430,43322,0.78
1611768714,"myHost2","anet","cisco","ASN420423",345062,143433,0.28


--- output event 1 ---
{
  "_raw": "\"1611768713\",\"myHost1\",\"anet\",\"cisco\",\"ASN4204269\",\"11430\",\"43322\",\"0.78\"",
  "time": "1611768713",
  "host": "myHost1",
  "source": "anet",
  "model": "cisco",
  "serial": "ASN4204269",
  "bytes_in": "11430",
  "bytes_out": "43322",
  "cpu": "0.78",
  "_time": 1611768713
}

--- output event 2 ---
{
  "_raw": "\"1611768714\",\"myHost2\",\"anet\",\"cisco\",\"ASN420423\",\"345062\",\"143433\",\"0.28\"",
  "time": "1611768714",
  "host": "myHost2",
  "source": "anet",
  "model": "cisco",
  "serial": "ASN420423",
  "bytes_in": "345062",
  "bytes_out": "143433",
  "cpu": "0.28",
  "_time": 1611768714
}
```

> With **Type: CSV** selected, an Event Breaker will properly add quotes around all values, regardless of their initial state.
>
{.box .success}

## Cribl Versus Custom Rulesets

Event Breaker rulesets shipped by Cribl will be listed under the **Cribl** tag, while user-built rulesets will be listed under **Custom**. Over time, Cribl will ship more patterns, so this distinction allows for both sets to grow independently. In the case of an ID/Name conflict, the Custom pattern takes priority in listings and search.

## Export and Import Rulesets {#import-custom}

You can export and import Custom (or Cribl) rulesets as JSON files. This facilitates sharing rulesets among Worker Groups or Cribl Edge deployments.

To export a ruleset:

1. Select to open an existing ruleset, or [create](#add) a new ruleset.

1. In the resulting modal, select **Advanced Mode** to open the JSON editor.

1. You can now modify the ruleset directly in JSON, if you choose.

1. Select **Export**, select a destination path, and name the file.

To import any ruleset that has been exported as a valid JSON file:

1. [Create](#add) a new ruleset.

1. In the resulting modal, select **Advanced Mode** to open the JSON editor.

1. Select **Import**, and choose the file you want.

1. Select **OK** to get back to the **New Ruleset** modal.

1. Select **Save**.

> Every ruleset must have a unique value in its top `id` key. Importing a JSON file with a duplicate `id` value will fail at the final **Save** step, with a message that the ruleset already exists. You can remedy this by giving the file a unique `id` value.
>
{.box .warning}

## Edge: Build Breakers from Files

You can use [Cribl Edge](/edge/about) to apply and author Event Breakers while exploring files – even files that you have not saved as sample files. This option includes remote files that are viewable only from Edge. See [Exploring Cribl Edge on Linux](/edge/explore-edge-linux) or [Exploring Cribl Edge on Windows](/edge/explore-edge-win).

## Troubleshooting

If you notice fragmented events, check whether Cribl Edge has added a `__timeoutFlush` internal field to them. This diagnostic field's presence indicates that the events were flushed because the Event Breaker buffer timed out while processing them. These timeouts can be due to large incoming events, backpressure, or other causes.

### Specify Minimum `_raw` Length

If you notice that the same Collector or Source is applying inconsistent Event Breakers to different datasets – such as intermittently falling back to the **System Default Rule** – you can address this by adjusting Breakers' **Min Raw Length** setting.

When determining which Event Breaker to use, Cribl Edge creates a substring from the incoming `_raw` field, at the length set by rules' **Max Event Bytes** parameters. If Cribl Edge does not get a match, it checks whether the substring of `_raw` has at least the threshold number of characters set by **Min Raw Length**. If so, Cribl Edge does not wait for more data.

You can tune this **Min Raw Length** threshold to account for varying numbers of inapplicable characters at the beginning of events. The default is `256` characters, but you can set this as low as `50`, or as high as `100000` (100 KB).
