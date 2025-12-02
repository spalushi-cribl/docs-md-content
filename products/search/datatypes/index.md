# Datatypes


Make searching easier by processing data into discrete events and defining fields.

---

Datatypes are sets of rules that Cribl Search follows to break data (bytes) from [Datasets](basic-concepts#datasets) into discrete
events, timestamp them, and parse them as needed. This helps with categorizing events and makes searching easier via
addressing through a `datatype` field.

Cribl Search ships with a number of Datatype definitions, and you can author, test, and validate custom Datatype rules
via an intuitive interface. A few built-in Datatype Rulesets are:

| Rule name         | Datatype value           |
| ----------------- | ------------------------ |
| AWS VPC Flow      | `aws_vpcflow`            |
| AWS ALB           | `aws_alb_accesslogs`     |
| AWS ELB           | `aws_elb_accesslogs`     |
| Apache Common     | `apache_access`          |
| Apache Combined   | `apache_access_combined` |
| Palo Alto Traffic | `pan_traffic`            |


> For details about who can access and modify this resource, see [Search Member Permissions](cloud-managing#member-roles). With appropriate Permissions, you can export, share, and import Datatypes in [Packs](packs).
>
{.box .info}

## Using Datatypes {#using}

To search a Dataset with a specific rule's results, use the `Datatype` field. For example,
`dataset=myDataset Datatype=aws_vpcflow`. Note that the value is not the `ID` of a Datatype – rather, it is the
`Datatype` of the rule.

Datatyping is managed via a collection of rules called Datatype Rulesets. A Dataset can be associated with one or more
Rulesets. Rulesets are evaluated top‑down, and they consist of an ordered list of rules that are also evaluated
top‑down.

Cribl Search will attempt to match the data against all the ordered rules, and it will process the search results using
the first rule that matches.

When you write data to an object store for later analysis by Cribl Search, ensure that events are homogeneously formatted within each file. This consistent structure is necessary in order for Cribl Search to accurately apply its Event Breakers and Parsers. Cribl Search determines a file’s Datatype by evaluating the file’s initial data against Datatype filter conditions, and then applies the first-matched Datatype to the remainder of the file.

Here’s an example of a Datatype Ruleset called “AWS Datatypes”:


![AWS Datatypes](search-aws-datatypes.png)
{scale="100%"}

Datatyping is applied before search terms - meaning that if a field is added, for example through parsing, you can
reference it in the query, such as in filter or projected terms.

Here’s an example of a Dataset configured with multiple Datatype Rulesets:


![Dataset with wildcarded Datatype Rulesets](search-dataset-with-multiple-datatypes.png)
{scale="100%"}

Use wildcards `*` to specify multiple Datatype Rulesets with similar names. Matching is case sensitive. For example,

- `A*` matches Datatype IDs starting with the letter A.
- `*A` matches Datatype IDs ending with the letter A.
- `*A*` matches Datatype IDs with the letter A.

## Order of Operations

The process of datatyping is as follows:

- Event Breaking – Breaks raw bytes into discrete events.
- Timestamping – Assigns timestamps to events.
- Parsing – Parses fields from events.
- Add Fields – Adds additional fields.

## Create a Rule {#add}

On the **Datatypes** page, select an existing Datatype ruleset, or select **Add Datatype** to create a new ruleset. Within a Datatype configuration modal, select **Add Rule** to open the **Rules** modal shown below:


![Adding a new rule](search-new-breaker-rule.png)
{scale="100%"}

Each rule includes the following components.

### Filter Condition

As data is processed, a rule's filter expression (JavaScript) is applied. If the expression evaluates to `true`, the
rule configurations are engaged on the Dataset. Else, the next rule down the line is evaluated.

### Event Breaker Settings

Event Breakers configure how Cribl Search converts data into discrete events. Cribl Search provides several different
formats for Event Breakers. See below for specific information on different [Event Breaker Types](#event-breaker-types).

### Timestamp Settings

After data is processed into events, Cribl Search will attempt timestamping. First, a timestamp anchor will be located
inside the event. Next, starting there, the engine will try to do one of the following:

- Scan up to a configurable depth into the event and autotimestamp, **or**
- Timestamp using a manually supplied `strptime` format, **or**
- Timestamp the event with the current time.

The closer an anchor is to the timestamp pattern, the better the performance and accuracy – especially if multiple
timestamps exist within an event. For the manually supplied option, the anchor must lead the engine **right before** the
timestamp pattern begins.


![Anchors preceding timestamps](breaker-timestamp-performance.f6c46f6c82.png)
{scale="100%"}

### Parser Settings

The Parser associated with a rule can be used to extract fields out of events that you can reference in searches. Each
field in the parsed event contains a key-value pair, where the field name is the key.

Parsers are very similar to the [Parser function](/stream/parser-function) that can be configured in Cribl Stream
pipelines.

You can configure a new Parser or select one from the library to configure. Based on the **Type** selected, you'll have
the following configurations:

- **Type**: Select the format of your data, `CSV`, `Extended Log File Format`, `Common Log Format`, `Key=Value Pairs`,
  `JSON Object`, `Delimited values`, `Regular Expression`, or `Grok`.
- **Library**: Select a Parser defined in the library. The library is located at **Knowledge** > **Parsers**.
- **Source field**: Define the field the Parser will use to extract from. Defaults to `_raw`.
- **Destination field**: Provide a name for the field to which the extracted data will be assigned.
- **List of fields**: The fields to be extracted in the desired order. If you don't specify any fields, the Parser will
  generate them automatically.
- **Fields to keep**: The fields to keep. Supports wildcards `*`. Takes precedence over **Fields to remove**.
- **Fields to remove**: The fields to remove. Supports wildcards `*`. You cannot remove fields that match
  **Fields to keep**.
- **Fields filter expression**: Expression evaluated against `{index, name, value}` context. Return `truthy` to keep a
  field, or `falsy` to remove it.

The following configuration lists are categorized by **Type**:

#### Key=Value Pairs

- **Clean fields**: Whether to clean field names by replacing non-alphanumeric characters `[a-zA-Z0-9]` with an
  underscore `_`.

**Advanced Settings**

- **Allowed key characters**: A list of characters that can appear in a key name, even though they're normally separator
  or control characters.
- **Allowed value characters**: A list of characters that can appear in a value name, even though they're normally
  separator or control characters.

#### Delimited Values

- **Delimiter**: Delimiter character to use to split values.
- **Quote character**: Character used to quote values. Required if values contain the delimiter character.
- **Escape character**: Character used to escape characters within a value.
- **Null value**: String value that should be treated as null or undefined.

#### Regular Expression

- **Regex**: Regex literal with named capturing groups, for example, `(?<foo>bar)`. Or with `_NAME_` and `_VALUE_`
  capturing groups, for example, `(?<_NAME_0>[^ =]+)=(?<_VALUE_0>[^,]+)`.
- **Additional regex**: Add another regular expression to match against other data.
- **Max exec**: The maximum number of times to apply the regex to the source field when the global flag is set, or when
  using named capturing groups.
- **Field name format expression**: JavaScript expression to format field names when `_NAME_n` and `_VALUE_n` capturing
  groups are used. The original field name is in the global variable `name`. For example, to append `XX` to all field
  names: `${name}_XX` (backticks are literal). If empty, names will be sanitized using this regex:
  `/^[_0-9]+|[^a-zA-Z0-9_]+/g`. You can access other field values via `__e.<fieldName>`.
- **Overwrite existing fields**: Overwrite existing event fields with extracted values. If set to **No**, existing
  fields will be converted to an array.

#### Grok

- **Pattern**: Grok pattern to extract fields. Syntax supported: `%{PATTERN_NAME:FIELD_NAME}`.
- **Additional Grok patterns**: Add another pattern to match other data.

### Add Fields to Events

After events have been timestamped, one or more fields can be added here as key-value pairs. In each field's
**Value Expression**, you can fully evaluate the field value using JavaScript expressions. Each rule should be
configured to add one field named `datatype` and its corresponding value. This way, it will be possible to determine
which Datatype each event belongs to by looking at that field.

## Event Breaker Types {#event-breaker-types}

Several types of Event Breakers can be applied to data:

- [Regex](#regex)
- [JSON New Line Delimited](#newline)
- [JSON Array](#array)
- [File Header](#header)
- [Timestamp](#timestamp)
- [CSV](#csv)
- [AWS CloudTrail](#aws-cloudtrail-breaker)
- [AWS VPC Flow Log](#aws-vpc-flow-log-breaker)


> For details about who can access and modify these resources, see [Search Member Permissions](cloud-managing#member-roles).
>
{.box .info}

### Regex Event Breaker {#regex}

The **Regex** breaker uses regular expressions to find breaking points in data.

Breaking will occur at the beginning of the match, and the matched content will be consumed/thrown away. If necessary,
you can use a positive lookahead regex to keep the content – for example: `(?=pattern)`

Capturing groups are **not allowed** to be used anywhere in the Event Breaker pattern, as they will further break the
data – which is often undesirable. Breaking will also occur if **Max Event Bytes** has been reached.


> The highest **Max Event Bytes** value that you can set is about 128 MB (`134217728` bytes). Events exceeding the maximum will be split into separate events, but left unbroken. Cribl Search will set these events' `__isBroken` internal field to `false`.
>
{.box .warning}

#### Example

With the following configurations, let's see how this sample data is datatyped:

- **Event Breaker:** – `[\n\r]+`
- **Parser** – `AWS VPC Flow Logs` from the Library.
- **Add Field** – `"datatype": "aws_vpcflow"`

```
--input--
2 123456789012 eni-0b2fc5457066bc156 10.0.0.164 54.239.152.25 41050 443 6 8 1297 1679339697 1679340000 ACCEPT OK
2 123456789013 eni-0b2fc5457066bc157 10.0.0.164 54.239.152.25 41050 443 6 8 1297 1679339698 1679340000 REJECT OK

--- output event 1 ---
{
 "_raw": "2 123456789012 eni-0b2fc5457066bc156 10.0.0.164 54.239.152.25 41050 443 6 8 1297 1679339697 1679340000 ACCEPT OK",
 "_time": 1679339697,
 "datatype": "aws_vpcflow",
 "version": "2",
 "account_id": "123456789012",
 "interface_id": "eni-0b2fc5457066bc156",
 "srcaddr": "10.0.0.164",
 "dstaddr": "54.239.152.25",
 "srcport": "41050",
 "dstport": "443",
 "protocol": "6",
 "packets": "8",
 "bytes": "1297",
 "start": "1679339697",
 "end": "1679340000",
 "action": "ACCEPT",
 "log_status": "OK",
 "dataset": "s3_vpcflowlogs",
}

--- output event 2 ---
{
 "_raw": "2 123456789013 eni-0b2fc5457066bc157 10.0.0.164 54.239.152.25 41050 443 6 8 1297 1679339698 1679340000 REJECT OK",
 "_time": 1679339698,
 "datatype": "aws_vpcflow",
 "version": "2",
 "account_id": "123456789013",
 "interface_id": "eni-0b2fc5457066bc156",
 "srcaddr": "10.0.0.164",
 "dstaddr": "54.239.152.25",
 "srcport": "41050",
 "dstport": "443",
 "protocol": "6",
 "packets": "8",
 "bytes": "1297",
 "start": "1679339698",
 "end": "1679340000",
 "action": "REJECT",
 "log_status": "OK",
 "dataset": "s3_vpcflowlogs",
}
```

### JSON New Line Delimited Event Breaker {#newline}

You can use the **JSON New Line Delimited** breaker to break and extract fields in newline-delimited JSON data.

#### Example

Using default values, let's see how this sample data breaks up:


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

### JSON Array Event Breaker {#array}

You can use the **JSON Array** breaker to extract events from an array in a JSON document (for example, an Amazon CloudTrail file).

- **Array Field**: Optional path to array in a JSON event with records to extract. For example, `Records`.
- **Timestamp Field**: Optional path to timestamp field in extracted events. For example, `eventTime` or
  `level1.level2.eventTime`.
- **JSON Extract Fields**: Enable this slider to auto-extract fields from JSON events. If disabled, only `_raw` and
  `time` will be defined on extracted events.
- **Timestamp Format**: If **JSON Extract Fields** is set to **No**, you **must** set this to **Autotimestamp** or
  **Current Time**. If **JSON Extract Fields** is set to **Yes**, you can select any option here.

#### Example

Using the values above, let's see how this sample data breaks up:


``` {title="Sample Event - JSON Document (Array)"}
--- input ---
{"Records":[{"eventVersion":"1.05","eventTime":"2020-04-08T01:35:55Z","eventSource":"ec2.amazonaws.com","eventName":"DescribeVolumes", "more_fields":"..."}, 
{"eventVersion":"1.05","eventTime":"2020-04-08T01:35:56Z","eventSource":"ec2.amazonaws.com","eventName":"DescribeInstanceAttribute", "more_fields":"..."}]}

--- output event 1 ---
{
  "_raw": "{\"eventVersion\":\"1.05\",\"eventTime\":\"2020-04-08T01:35:55Z\",\"eventSource\":\"ec2.amazonaws.com\",\"eventName\":\"DescribeVolumes\", \"more_fields\":\"...\"}",
  "_time": 1586309755,
  "cribl_breaker": "j-array"
}

--- output event 2 ---
{
  "_raw": "{\"eventVersion\":\"1.05\",\"eventTime\":\"2020-04-08T01:35:56Z\",\"eventSource\":\"ec2.amazonaws.com\",\"eventName\":\"DescribeInstanceAttribute\", \"more_fields\":\"...\"}",
  "_time": 1586309756,
  "cribl_breaker": "j-array"
}
```

### File Header Event Breaker {#header}

You can use the **File Header** breaker to break files with headers, such as IIS or Bro logs. This type of breaker relies on
a header section that lists field names. The header section is typically present at the top of the file, and can be
single-line or greater.

After the file has been broken into events, fields will also be extracted, as follows:

- **Header Line**: Regex matching a file header line. For example, `^#`.
- **Field Delimiter**: Field delimiter regex. For example, `\s+`.
- **Field Regex**: Regex with one capturing group, capturing all the fields to be broken by field delimiter. For
  example, `^{{< id `Ff` >}}ields[:]?\s+(.*)`
- **Null Values**: Representation of a null value. Null fields are not added to events.
- **Clean Fields**: Whether to clean up field names by replacing non-alphanumeric characters `[a-zA-Z0-9]` with an
  underscore `_`.

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

### Timestamp Event Breaker {#timestamp}

You can use the **Timestamp** breaker to break events at the beginning of any line in which Cribl Search finds a timestamp.
This type enables breaking on lines whose timestamp pattern is not known ahead of time.

#### Example

Using default values, let's see how this sample data breaks up:


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

### CSV Event Breaker {#csv}

The **CSV** breaker extracts fields in CSV data that include a header line. Selecting this type exposes these extra fields:

- **Delimiter**: Delimiter character to use to split values. Defaults to: `,`.
- **Quote Char**: Character used to quote literal values. Defaults to: `"`.
- **Escape Char**: Character used to escape the quote character in field values. Defaults to: `"`.

#### Example

Using default values, let's see how this sample data breaks up:


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

### AWS CloudTrail Event Breaker

Use the **AWS CloudTrail** breaker to break and parse
[AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) events much
faster.

See the example of input and output events below.


``` {title="Sample Event - AWS CloudTrail Breaker"}
--- input ---
{"Records":[{"eventVersion":"1.0","userIdentity":{"type":"IAMUser","principalId":"EX_PRINCIPAL_ID","arn":"arn:aws:iam::123456789012:user/Alice","accessKeyId":"EXAMPLE_KEY_ID","accountId":"123456789012","userName":"Alice"},"eventTime":"2014-03-06T21:22:54Z","eventSource":"ec2.amazonaws.com","eventName":"StartInstances","awsRegion":"us-east-2","sourceIPAddress":"205.251.233.176","userAgent":"ec2-api-tools 1.6.12.2","requestParameters":{"instancesSet":{"items":[{"instanceId":"i-ebeaf9e2"}]}},"responseElements":{"instancesSet":{"items":[{"instanceId":"i-ebeaf9e2","currentState":{"code":0,"name":"pending"},"previousState":{"code":80,"name":"stopped"}}]}}},{"eventVersion":"1.0","userIdentity":{"type":"IAMUser","principalId":"EX_PRINCIPAL_ID","arn":"arn:aws:iam::123456789012:user/Rabbit","accessKeyId":"EXAMPLE_KEY_ID","accountId":"123456789012","userName":"Rabbit"},"eventTime":"2014-03-06T21:22:54Z","eventSource":"ec2.amazonaws.com","eventName":"StartInstances","awsRegion":"us-east-2","sourceIPAddress":"205.251.233.176","userAgent":"ec2-api-tools 1.6.12.2","requestParameters":{"instancesSet":{"items":[{"instanceId":"i-ebeaf9e2"}]}},"responseElements":{"instancesSet":{"items":[{"instanceId":"i-ebeaf9e2","currentState":{"code":0,"name":"pending"},"previousState":{"code":80,"name":"stopped"}}]}}},{"eventVersion":"1.0","userIdentity":{"type":"IAMUser","principalId":"EX_PRINCIPAL_ID","arn":"arn:aws:iam::123456789012:user/Hatter","accessKeyId":"EXAMPLE_KEY_ID","accountId":"123456789012","userName":"Hatter"},"eventTime":"2014-03-06T21:22:54Z","eventSource":"ec2.amazonaws.com","eventName":"StartInstances","awsRegion":"us-east-2","sourceIPAddress":"205.251.233.176","userAgent":"ec2-api-tools 1.6.12.2","requestParameters":{"instancesSet":{"items":[{"instanceId":"i-ebeaf9e2"}]}},"responseElements":{"instancesSet":{"items":[{"instanceId":"i-ebeaf9e2","currentState":{"code":0,"name":"pending"},"previousState":{"code":80,"name":"stopped"}}]}}}]}

--- output event 1 ---
{
  "_raw": "{\"eventVersion\":\"1.0\",\"userIdentity\":{\"type\":\"IAMUser\",\"principalId\":\"EX_PRINCIPAL_ID\",\"arn\":\"arn:aws:iam::123456789012:user/Alice\",\"accessKeyId\":\"EXAMPLE_KEY_ID\",\"accountId\":\"123456789012\",\"userName\":\"Alice\"},\"eventTime\":\"2014-03-06T21:22:54Z\",\"eventSource\":\"ec2.amazonaws.com\",\"eventName\":\"StartInstances\",\"awsRegion\":\"us-east-2\",\"sourceIPAddress\":\"205.251.233.176\",\"userAgent\":\"ec2-api-tools 1.6.12.2\",\"requestParameters\":{\"instancesSet\":{\"items\":[{\"instanceId\":\"i-ebeaf9e2\"}]}},\"responseElements\":{\"instancesSet\":{\"items\":[{\"instanceId\":\"i-ebeaf9e2\",\"currentState\":{\"code\":0,\"name\":\"pending\"},\"previousState\":{\"code\":80,\"name\":\"stopped\"}}]}}}",
  "_time": 1711112256.322
}

--- output event 2 ---
{
  "_raw": "{\"eventVersion\":\"1.0\",\"userIdentity\":{\"type\":\"IAMUser\",\"principalId\":\"EX_PRINCIPAL_ID\",\"arn\":\"arn:aws:iam::123456789012:user/Rabbit\",\"accessKeyId\":\"EXAMPLE_KEY_ID\",\"accountId\":\"123456789012\",\"userName\":\"Rabbit\"},\"eventTime\":\"2014-03-06T21:22:54Z\",\"eventSource\":\"ec2.amazonaws.com\",\"eventName\":\"StartInstances\",\"awsRegion\":\"us-east-2\",\"sourceIPAddress\":\"205.251.233.176\",\"userAgent\":\"ec2-api-tools 1.6.12.2\",\"requestParameters\":{\"instancesSet\":{\"items\":[{\"instanceId\":\"i-ebeaf9e2\"}]}},\"responseElements\":{\"instancesSet\":{\"items\":[{\"instanceId\":\"i-ebeaf9e2\",\"currentState\":{\"code\":0,\"name\":\"pending\"},\"previousState\":{\"code\":80,\"name\":\"stopped\"}}]}}}",
  "_time": 1711112355.478
}

--- output event 3 ---
{
  "_raw": "{\"eventVersion\":\"1.0\",\"userIdentity\":{\"type\":\"IAMUser\",\"principalId\":\"EX_PRINCIPAL_ID\",\"arn\":\"arn:aws:iam::123456789012:user/Hatter\",\"accessKeyId\":\"EXAMPLE_KEY_ID\",\"accountId\":\"123456789012\",\"userName\":\"Hatter\"},\"eventTime\":\"2014-03-06T21:22:54Z\",\"eventSource\":\"ec2.amazonaws.com\",\"eventName\":\"StartInstances\",\"awsRegion\":\"us-east-2\",\"sourceIPAddress\":\"205.251.233.176\",\"userAgent\":\"ec2-api-tools 1.6.12.2\",\"requestParameters\":{\"instancesSet\":{\"items\":[{\"instanceId\":\"i-ebeaf9e2\"}]}},\"responseElements\":{\"instancesSet\":{\"items\":[{\"instanceId\":\"i-ebeaf9e2\",\"currentState\":{\"code\":0,\"name\":\"pending\"},\"previousState\":{\"code\":80,\"name\":\"stopped\"}}]}}}",
  "_time": 1711112355.478
}
```

### AWS VPC Flow Log Event Breaker

Use the **AWS VPC Flow Log** breaker to break and parse
[AWS VPC Flow Log](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html) events as fast as possible.

This breaker works properly with files formatted as follows:

- Each file starts with a header. That header is in the same format as other lines but describes the column names.
- Every line has the same number of columns.
- Each column is separated by the space (` `) character. There is no space before the first or after the last column.
- Each line ends with the newline character (`\n`).
- A single dash (`-`) indicates a null value for the column where it appears.

See the example of input and output events below.


``` {title="Sample Event - AWS VPC Flow Log Breaker"}
--- input ---
version account_id interface_id srcaddr dstaddr srcport dstport protocol packets bytes start end action log_status
2 123456789014 eni-07e5d76940fbaa5c6 231.234.233.57 83.70.34.149 19856 15726 17 3048 380 1262332800 1262332900 ACCEPT NODATA
2 123456789010 eni-0858304751c757b2e 31.200.171.164 22.109.125.129 58551 456 17 10824 352 1262332801 1262333301 ACCEPT OK
2 123456789013 eni-07e5d76940fbaa5c6 78.128.152.196 67.42.235.136 25164 29139 17 5380 160 1262332802 1262332902 REJECT OK

--- output event 1 ---
{
  "_raw": "2 123456789014 eni-07e5d76940fbaa5c6 231.234.233.57 83.70.34.149 19856 15726 17 3048 380 1262332800 1262332900 ACCEPT NODATA",
  "version": "2",
  "account_id": "123456789014",
  "interface_id": "eni-07e5d76940fbaa5c6",
  "srcaddr": "231.234.233.57",
  "dstaddr": "83.70.34.149",
  "srcport": "19856",
  "dstport": "15726",
  "protocol": "17",
  "packets": "3048",
  "bytes": "380",
  "start": "1262332800",
  "end": "1262332900",
  "action": "ACCEPT",
  "log_status": "NODATA",
  "_time": 1611768713
}

--- output event 2 ---
{
  "_raw": "2 123456789010 eni-0858304751c757b2e 31.200.171.164 22.109.125.129 58551 456 17 10824 352 1262332801 1262333301 ACCEPT OK",
  "version": "2",
  "account_id": "123456789010",
  "interface_id": "eni-0858304751c757b2e",
  "srcaddr": "31.200.171.164",
  "dstaddr": "22.109.125.129",
  "srcport": "58551",
  "dstport": "456",
  "protocol": "17",
  "packets": "10824",
  "bytes": "352",
  "start": "1262332801",
  "end": "1262333301",
  "action": "ACCEPT",
  "log_status": "OK",
  "_time": 1611768714
}

--- output event 3 ---
{
  "_raw": "2 123456789013 eni-07e5d76940fbaa5c6 78.128.152.196 67.42.235.136 25164 29139 17 5380 160 1262332802 1262332902 REJECT OK",
  "version": "2",
  "account_id": "123456789013",
  "interface_id": "eni-07e5d76940fbaa5c6",
  "srcaddr": "78.128.152.196",
  "dstaddr": "67.42.235.136",
  "srcport": "25164",
  "dstport": "29139",
  "protocol": "17",
  "packets": "5380",
  "bytes": "160",
  "start": "1262332802",
  "end": "1262332902",
  "action": "REJECT",
  "log_status": "OK",
  "_time": 1611768715
}
```


> With **Type: AWS VPC Flow Log** selected, an Event Breaker will properly add quotes around all values, regardless of their initial state.
>
{.box .success}

## Cribl Rulesets versus Custom Rulesets

Datatype rulesets shipped by Cribl will be listed under the **Cribl** tag, while the user-built ones will be listed
under **Custom**. Over time, Cribl will ship more patterns, so this distinction allows for both sets to grow
independently. In the case of an ID/Name conflict, the Custom pattern takes priority in listings and search.

## Exporting and Importing Datatype Rulesets {#import-custom}

You can export and import Custom (or Cribl) rulesets as JSON files. This facilitates sharing across Cribl.Cloud
organizations.

To export a Datatype Ruleset:

1. Select to open an existing ruleset, or [create](#add) a new ruleset.
1. In the resulting modal, select **Manage as JSON** to open the JSON editor.
1. You can now modify the ruleset directly in JSON, if you choose.
1. Select **Export**, select a destination path, and name the file.

To import any Dataset Ruleset that has been exported as a valid JSON file:

1. [Create](#add) a new ruleset.
1. In the resulting modal, select **Manage as JSON** to open the JSON editor.
1. Select **Import**, and choose the file you want.
1. Select **OK** to get back to the **New Ruleset** modal.
1. Select **Save**.


> Every Dataset Ruleset must have a unique value in its top `id` key. Importing a JSON file with a duplicate `id` value will fail at the final **Save** step, with a message that the ruleset already exists. You can remedy this by giving the file a unique `id` value.
>
{.box .warning}

