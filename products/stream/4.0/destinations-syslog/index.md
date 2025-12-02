# Syslog


Cribl Stream supports sending out data over syslog via TCP or UDP.  

> Type: Streaming | TLS Support: Configurable | PQ Support: Yes  
> 
> This Syslog Destination supports [RFC 3164](https://tools.ietf.org/html/rfc3164) and [RFC 5424](https://tools.ietf.org/html/rfc5424). Before you configure this Destination, review [Understanding Syslog Format Options](#structure) below, to ensure that your configuration will structure compliant outbound events that downstream services can read.
>
{.box .info} 

## Configuring Cribl Stream to Output Data in Syslog Format

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **+ Add Destination** at right. From the resulting drawer's tiles, select **Syslog**. Next, click either **+ Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.

Or, to configure via the [Routing](routes) UI, click **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). From the resulting page's tiles or the **Destinations** left nav, select **Syslog**. Next, click **New Destination** to open a **New Destination** modal that provides the options below.

### General Settings

**Output ID**: Enter a unique name to identify this Syslog definition.

**Protocol**: The network protocol to use for sending out syslog messages. Defaults to `TCP`; `UDP` is also available.

**Load balancing**: This option is displayed only when the **Protocol** is set to `TCP`. When toggled to `Yes`, see [Load Balancing Settings](#lb) below. With the default `No` setting, if you notice that Cribl Stream is not sending data to all possible IP addresses, enable **Advanced Settings** > **Round-robin DNS**.

**Address**: Address/hostname of the receiver.

**Port**: Port number to connect to on the host.

**Max record size**: Displayed when **Protocol** is set to `UDP`. Sets the maximum size of syslog messages. Defaults to `1500`, and must be ≤ `2048`. To avoid packet fragmentation, keep this value <= the MTU (maximum transmission unit for the network path to the Destination system). When the message length exceeds the size limit, it is truncated and followed with an ellipsis ("...").

### Optional Settings

**Facility**: Default value for message facility. If set, will be overwritten by the value of `__facility`. Defaults to `user`.

**Severity**: Default value for message severity. If set, will be overwritten by the value of `__severity`. Defaults to `notice`.

**App name**: Default value for application name. If set, will be overwritten by the value of `__appname`. Defaults to `Cribl`.

**Message format**: The syslog message format supported by the receiver. Defaults to `RFC3164`.

**Timestamp format**: The timestamp format to use when serializing an event's time field. Options include `Syslog` (default) and `ISO8601`.

For `Syslog`, the format is `Mmm dd hh:mm:ss`, using the timezone of the syslog device. For example: `Jan 18 16:56:36`.

For `ISO8601`, the basic format is `YYYY‑MM‑DD`. For example: `2023‑01‑18`. For extended formats, add hours, minutes, seconds, decimal fractions (optional), and timezone. For example: `2023‑01‑18T16:56:36.996‑08:00`.

**Throttling**: Throttle rate, in bytes per second. Defaults to `0`, meaning no throttling. Multiple-byte units such as KB, MB, GB etc. are also allowed, e.g., `42 MB`. When throttling is engaged, your **Backpressure behavior** selection determines whether Cribl Stream will handle excess data by blocking it, dropping it, or queueing it to disk.

**Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`.

**Tags**: Optionally, add tags that you can use for filtering and grouping at the final destination. Use a tab or hard return between (arbitrary) tag names.

### Load Balancing Settings {#lb}

> Load balancing is available only when the **Protocol** is set to `TCP`.
>
{.box .info}

Enabling the **Load balancing** slider replaces the static **General Settings** > **Address** and **Port** fields with the following controls:

#### Exclude Current Host IPs

This slider determines whether to exclude all IPs of the current host from the list of any resolved hostnames. Defaults to `No`.

#### Destinations {#destinations}

The **Destinations** table is where you specify a known set of receivers on which to load-balance data. Click **+ Add Destination** to specify more receivers on new rows. Each row provides the following fields:

**Address**: Hostname of the receiver. Optionally, you can paste in a comma-separated list, in `<host>:<port>` format.

**Port**: Port number to send data to on this host.

**TLS**: Whether to inherit TLS configs from group setting, or disable TLS. Defaults to `inherit`.

**TLS servername**: Servername to use if establishing a TLS connection. If not specified, defaults to connection host (if not an IP). Otherwise, uses the global TLS settings. 

**Load weight**: The weight to apply to this receiver for load-balancing purposes.

The final column provides an `X` button to delete any row from the table.

### Persistent Queue Settings

> This section is displayed only for **TCP**, and only when **Backpressure behavior** is set to **Persistent Queue**.
>
{.box .info}

**Max file size**: The maximum data volume to store in each queue file before closing it. Enter a numeral with units of KB, MB, etc. Defaults to `1 MB`.

**Max queue size**: The maximum amount of disk space the queue is allowed to consume. Once this limit is reached, Cribl Stream stops queueing and applies the fallback **Queue‑full behavior**. Enter a numeral with units of KB, MB, etc.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this value, Cribl Stream will append `/<worker‑id>/<output‑id>`.

**Compression**: Codec to use to compress the persisted data, once a file is closed. Defaults to `None`; `Gzip` is also available.

**Queue-full behavior**: Whether to block or drop events when the queue is exerting backpressure (because disk is low or at full capacity). **Block** is the same behavior as non-PQ blocking, corresponding to the **Block** option on the **Backpressure behavior** drop-down. **Drop new data** throws away incoming data, while leaving the contents of the PQ unchanged.

**Clear persistent queue**: Click this button if you want to flush out files that are currently queued for delivery to this Destination. A confirmation modal will appear. (Appears only after **Output ID** has been defined.)

### TLS Settings (Client Side)

**Enabled** defaults to `No`. When toggled to `Yes`:


**Validate server certs**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (e.g., the system's CA). Defaults to `No`.

**Server name (SNI)**: Server name for the SNI (Server Name Indication) TLS extension. This must be a host name, not an IP address.

**Minimum TLS version**: Optionally, select the minimum TLS version to use when connecting.

**Maximum TLS version**: Optionally, select the maximum TLS version to use when connecting.

**Certificate name**: The name of the predefined certificate.
 
**CA certificate path**: Path on client containing CA certificates (in PEM format) to use to verify the server's cert. Path can reference `$ENV_VARS`.
 
**Private key path (mutual auth)**: Path on client containing the private key (in PEM format) to use.  Path can reference `$ENV_VARS`. **Use only if mutual auth is required**.
 
**Certificate path (mutual auth)**: Path on client containing certificates in (PEM format) to use. Path can reference `$ENV_VARS`. **Use only if mutual auth is required**. 
 
**Passphrase**: Passphrase to use to decrypt private key.

### Timeout Settings

> These timeout settings apply only to the TCP protocol.
>
{.box .info}

**Connection timeout**: Amount of time (in milliseconds) to wait for the connection to establish, before retrying. Defaults to `10000`.

**Write timeout**: Amount of time (milliseconds) to wait for a write to complete, before assuming connection is dead. Defaults to `60000`.

### Processing Settings

#### Post‑Processing

**Pipeline**: Pipeline to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.

### Advanced Settings

The first two options are always displayed:

**Octet count framing**: When enabled, Cribl Stream prefixes messages with their byte count, regardless of whether the messages are constructed from `message`, `__syslogout`, or `_raw`. When disabled, Cribl Stream omits prefixes, and instead appends a `\n` to messages.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

The following options are added if you enable the [General Settings](#general-settings) tab's **Load balancing** option:

**DNS resolution period (seconds)**: Re-resolve any hostnames after each interval of this many seconds, and pick up destinations from A records. Defaults to `600` seconds.

**Load balance stats period (seconds)**: Lookback traffic history period. Defaults to `300` seconds.  (Note that If multiple receivers are behind a hostname – i.e., multiple A records – all resolved IPs will inherit the weight of the host, unless each IP is specified separately. In Cribl Stream load balancing, IP settings take priority over those from hostnames.)

**Max connections**: Constrains the number of concurrent receiver connections, per Worker Process, to limit memory utilization. If set to a number &gt; `0`, then on every DNS resolution period, Cribl Stream will randomly select this subset of discovered IPs to connect to. Cribl Stream will rotate IPs in future resolution periods – monitoring weight and historical data, to ensure fair load balancing of events among IPs.

### How Does Load Balancing Work {#lb-how}

Cribl Stream will attempt to load-balance outbound data as fairly as possibly across all receivers (listed as Destinations in the GUI). If FQDNs/hostnames are used as the Destination address and each resolves to, for example, 5 (unique) IPs, then each Worker Process will have its # of outbound connections = {# of IPs x # of FQDNs} for purposes of the Destination. Data is sent by all Worker Processes to all receivers simultaneously, and the amount sent to each receiver depends on these parameters: 

1. Respective destination **weight**.
2. Respective destination **historical data**.

By default, historical data is tracked for 300s. Cribl Stream uses this data to influence the traffic sent to each destination, to ensure that differences decay over time, and that total ratios converge towards configured weights. 

#### Example

Suppose we have two receivers, A and B, each with weight of `1` (i.e., they are configured to receive equal amounts of data). Suppose further that the load-balance stats period is set at the default `300s` and – to make things easy – for each period, there are 200 events of equal size (Bytes) that need to be balanced. 

Interval | Time Range | Events to be dispensed
--- | --- | ---
1 | *time=0s* ---> *time=300s* | **200** 

   Both A and B start this interval with 0 historical stats each.

   Let's assume that, due to various circumstances, 200 events are "balanced" as follows:
   `A = 120 events` and `B = 80 events` – a difference of **40 events** and a ratio of **1.5:1**.

Interval | Time Range | Events to be dispensed
--- | --- | ---
2 | *time=300s* ---> *time=600s* | **200**

At the beginning of interval 2, the load-balancing algorithm will look back to the previous interval stats and carry **half** of the receiving stats forward. I.e., receiver A will start the interval with **60** and receiver B with **40**. To determine how many events A and B will receive during this next interval, Cribl Stream will use their weights and their stats as follows:

Total number of events: `events to be dispensed + stats carried forward = 200 + 60 + 40 = 300`.
Number of events per each destination (weighed): `300/2 = 150` (they're equal, due to equal weight).
Number of events to send to each destination `A: 150 - 60 = 90` and `B: 150 - 40 = 110`.

Totals at end of interval 2: `A=120+90=210`, `B=80+110=190`, a difference of **20 events** and a ratio of **1.1:1**.

Over the subsequent intervals, the difference becomes exponentially less pronounced, and eventually insignificant. Thus, the load gets balanced fairly.

## Understanding Syslog Format Options {#structure}

Unlike other Cribl Stream Destinations, Syslog applies an additional post-processing step after the Pipeline(s) transform events. This additional step structures the data for compliance with the syslog transport protocol (RFC 3164 and/or RFC 5424) before it is transmitted to downstream services.

The Syslog Destination's **General Settings** page offers several settings to format the timestamps, to format the message delivering the event, and to set the syslog-specific default settings for Facility, Severity, and App Name.  

Below are two examples of RFC-compliant syslog events: 
- `<13>Jul 11 10:34:35 testbox testing[42]`
- `<214>1 2022-07-11T18:58:45.000Z testbox testing[42]: foo=bar this=that base=ball gizmo=sprocket`

Cribl Stream offers three different output approaches in the Syslog Destination. The flowchart below summarizes how Cribl Stream determines which approach to use:

- [`message`](#message): For ease of use, Cribl recommends using this option. When you define `message`, Cribl Stream discards `_raw`, and composes a new payload using the other syslog-related fields. Cribl Stream automatically processes the values of the `message`, `_time`, `host`, and other fields, and creates an ISO-compliant timestamp and a properly formatted event. To use this method, you must configure `message` and must **not** set `__syslogout`. 

- [`__syslogout`](#syslogout): When you define `__syslogout`, Cribl Stream sends it as the entire syslog message, discards `_raw`, and ignores all the other fields.

- [`_raw`](#using_raw) (with or without a header): If you didn't define `message` or `__syslogout`, then Cribl Stream uses the existing `_raw` field as the `message` field, and prepends all the other syslog-related fields to the event. 

![Syslog output flow](syslog_destination_flow.0e821e963a.png)
{border="true"}

The subsections below walk you through some considerations for each of these options, before you configure this Destination. First, let's take a look at internal fields, since they play a critical role in formatting. 

### Internal Fields {#internal}

Cribl Stream uses a set of internal fields to assist in forwarding data to downstream services. Fields for this Destination:

  * `__priority`
  * `__facility`
  * `__severity`
  * `__procid`
  * `__appname`
  * `__msgid`
  * `__host`
  * `__syslogout`

### Defining `message`{#message}

This approach is easiest and least error-prone, because Cribl Stream creates the payload for you. To define the `message`, all you need to supply are the following required fields:

  * `__facility`
  * `__severity`
  * `__host`

Or, to make this even simpler, you can substitute `__priority` for `__facility` and  `__severity`. Either way, Cribl Stream will create a new payload using a combination of these required fields. For details on the fields assembled here, see [Important Fields](#important) below. 

### Exporting `__syslogout` {#syslogout}

As a second approach, if you choose to send `__syslogout` to downstream services, it is exclusive – it becomes the entire syslog message sent. Neither `_raw`, nor any other metadata, will be sent downstream. Also, Cribl Stream will not check to ensure that the value of `__syslogout` is RFC-compliant. 

The result will be a proper syslog message only if you hand-build the event yourself. You must manually construct the `__syslogout` payload, starting with `_time`, for all the fields that Cribl Stream would automatically handle with the above `message` option. In particular:

> When defining `__syslogout`, you must follow the syslog protocol's RFCs. Otherwise, downstream services will misinterpret or completely ignore the message. Some syslog receivers might drop non-compliant events entirely, or try to “fix” the format by supplying missing fields.
>
{.box .warning} 
 
If you have **Octet count framing** enabled for this Destination, Cribl Stream will prepend the number of bytes of the constructed `__syslogout` field to the message before sending it.
 
The most common uses for the `__syslogout` method are:
- When the value of `_raw` is already in syslog format as it comes in, and minimal processing is necessary.
- When sending raw TCP data to a destination that doesn’t enforce syslog RFCs, such as a raw TCP listener. Cribl Stream currently does not provide a native "raw TCP" Destination. However, in some environments, configuring a Syslog Destination with the TCP protocol and `__syslogout` is an effective method for delivering raw data over TCP.

#### Constructing `__syslogout`

You'll need to add `_time` to the payload. For example, in an [Eval Function](eval-function), you could use **Evaluate fields** to build up `__syslogout` in the expression below. Here, the `priority` encodes the `severity` + `facility`, according to the syslog protocol.

| Name | Value Expression |
| --- | --- |
| `priority` | `(8*facility)+severity` |
| `__syslogout` | `` `<${priority}>${C.Time.strftime(_time,"%Y-%m-%dT%H:%M:%S.%f%z")} ${host} ${appname}[${procid}]: ${_raw}` `` |

Here's that example expression in a full Function:

![Adding `__syslogout`, `_time` to construct a valid syslog message](syslogout-add-time-how.1c5e34d44c.png)
{border="true"}

### Enhancing `_raw` with syslog {#using_raw}

A third approach is to add the message content in `_raw`, and construct the syslog "envelope" around `_raw` by including the `severity`, `priority`, `facility`, `procid`, `msgid`, and `appname` fields, as required. 

Here's an alternative Eval Function that illustrates this:

![Adding syslog decorations to `_raw`](syslog-raw-out.91cbd13554.png)
{border="true"}

Both Eval Functions are provided in this example Pipeline:

```json {title="syslog_loop.json"}
{
  "id": "syslog_loop",
  "conf": {
    "output": "default",
    "groups": {},
    "asyncFuncTimeout": 1000,
    "functions": [
      {
        "filter": "true",
        "conf": {
          "mode": "reserialize",
          "type": "json",
          "srcField": "_raw",
          "dstField": "_raw",
          "keep": [
            "resource",
            "path",
            "httpMethod"
          ],
          "remove": []
        },
        "id": "serde",
        "disabled": false
      },
      {
        "filter": "true",
        "conf": {
          "clones": [
            {
              "__syslog_test": "true"
            }
          ]
        },
        "id": "clone",
        "disabled": false
      },
      {
        "filter": "__syslog_test",
        "conf": {
          "add": [
            {
              "name": "appname",
              "value": "'using_syslogout'"
            },
            {
              "name": "severity",
              "value": "1"
            },
            {
              "name": "facility",
              "value": "3"
            },
            {
              "name": "pri",
              "value": "(8 * facility) + severity"
            },
            {
              "name": "procid",
              "value": "'7777'"
            },
            {
              "name": "__syslogout",
              "value": "`<${pri}>${C.Time.strftime(_time,\"%b %d %H:%M:%S\")} ${host} ${appname}[${procid}]: ${_raw}`"
            }
          ],
          "keep": [],
          "remove": []
        },
        "id": "eval",
        "disabled": false
      },
      {
        "filter": "! __syslog_test",
        "conf": {
          "add": [
            {
              "name": "severity",
              "value": "1"
            },
            {
              "name": "facility",
              "value": "3"
            },
            {
              "name": "procid",
              "value": "8889"
            },
            {
              "name": "appname",
              "value": "'using_raw'"
            }
          ],
          "keep": [
            "_raw",
            "*severity",
            "*facility",
            "*procid",
            "_time",
            "*appname"
          ],
          "remove": [
            "*"
          ]
        },
        "id": "eval",
        "disabled": false
      }
    ]
  }
}
```

In this method, Cribl Stream takes the value of  `_raw` verbatim, and then prepends a human-readable timestamp, host, and other information. 

If you are using `_raw` as the `message` field, make sure you explicitly set the `priority` and `facility` whenever possible. Otherwise, Cribl Stream will use the default values. The acceptable values are defined in the RFCs.

> When using `_raw`, you might end up with duplicate fields in the event. For example, even if your event already contains a `timestamp`, Cribl Stream might prepend another `timestamp` to the beginning of `_raw`, without checking whether the data is already present. This can increase event sizes with redundant data. If you use this method, make sure you first strip the prepended (duplicate) fields. 
> 
> Also, when using `_raw`, this Destination does not check whether there's a valid header defined in the event – it always adds one. Also, because this Destination reformats the data, you might not see the header information when you preview it in Live Capture. 
>
{.box .warning} 

 For details on the fields assembled here, see [Important Fields](#important) below.

### Defined `message` and `_raw` Fields {#double_definitions}

If Cribl Stream's Destination stage receives an event that contains both `message` and `_raw` fields, it will build the syslog package using the `message` field, discarding `_raw`. 

The `message` (or `_raw`) field must be an ASCII string in order to be put on the syslog wire. This Destination does not handle JSON objects. Avoid mismatching these types, or else no data might be sent out.

If your intermediate processing – such as a `JSON.parse(_raw)` transformation – has converted the `_raw` field's contents into a JSON object literal, you will need to stringify the result, using a method like `JSON.stringify(_raw)`. You can do so in a [post‑processing Pipeline](pipelines#output-conditioning-pipelines) attached to the Syslog Destination.

### Important Fields {#important}

The `Message` and `_raw` prepend methods use additional fields to create the final payload. When using `__syslogout`, Cribl Stream ignores these fields. 

The fields below appear in the order generated by a syslog-formatted event.

**Sample syslog-formatted event**: `<38>1 2022-07-11T22:04:46.000Z testbox app01 [4321] AC-123 - foo=bar this=that base=ball gizmo=sprocket`

- `__facility` or **facility**: Cribl Stream uses this field to calculate priority. The RFC protocol dictates Facility levels. For details, see [Facility](https://en.wikipedia.org/wiki/Syslog#Facility).

- `__severity` or **severity**: Cribl Stream also uses this field to calculate priority. The RFC protocol dictates Severity levels. For details, see [Severity](https://en.wikipedia.org/wiki/Syslog#Severity_level).

- `__priority`: If you configure this field, Cribl Stream will use it and override the `severity` and `facility` values. The priority displays at the beginning of a syslog event, `<38>` in the example above.  If you don't configure this field, then Cribl Stream calculates it using the formula: `priority = (8*facility + severity)`. 

    For example, if the `facility` is `13` (Security) and the `severity` is `2`(Critical), the `priority` will be `13*8 + 2 = 106`. The `priority` of `<38> `in the example above is `8*4(Facility of auth) + 6 (Severity of info)`.

- `_time`: The value of `_time` in Cribl events is in epoch format, but the syslog RFCs dictate that each event’s timestamp is must be in human-readable format.  When defining a Syslog Destination, you can have an option to use the ISO8601 (recommended) or the syslog format. ISO8601 defines the method for specifying time zone and year, whereas the older syslog format lacks this information. The example above shows the ISO8601 format, listed after the `priority` (`<13>`) and `version`.

- `__host` OR **host**: the value for the required host field in a syslog event, following the timestamp.`testbox` in the example above.

- `__appname` or **appname**: The required application name. `app01` in the example above. This is typically the name of the daemon or process that is logging any given event.

- `__procid` or **procid**: This optional field is the Process ID.`4321` in the example above. Use a numeric value for this field, optionally this may be surrounded with brackets [ ]. Cribl Stream will automatically adjust the spaces and syntax to ensure RFC-compliant formatting.

- `__msgid` or **msgid**: This optional field is the Message ID. `AC-123` in the example above. 
 
For each pair of attribute names (above),  Cribl Stream uses the values as specified here:

1. The event contains both `__<key>` and `<key>`: Cribl Stream uses the value of `__<key>` and ignores `<key>` if also present. For example, if the event contains `__host`, the value of **host** will be ignored if also present. 

2. The event contains `<key>`: Cribl Stream uses the value of `<key>`. For example, if **host** is set, the value is applied. 

3. The event contains neither `__<key>` nor `<key>`,  Cribl Stream uses the default values configured in the Syslog Destination.


