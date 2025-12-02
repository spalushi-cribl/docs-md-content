# Syslog Destination


Cribl Edge supports sending out data over syslog via TCP or UDP.  

> Type: Streaming | TLS Support: Configurable | PQ Support: Yes  
> 
> This Syslog Destination supports [RFC 3164](https://tools.ietf.org/html/rfc3164) and [RFC 5424](https://tools.ietf.org/html/rfc5424). **Before** you configure this Destination, review [Syslog Format Options](#format) and [Structure Syslog Output](#approaches) below. This will help you ensure that your configuration will structure RFC-compliant outbound events that downstream services can read.
>
{.box .info}

## Configure Cribl Edge to Output Data in Syslog Format {#configuring}

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Syslog definition. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**.
   - **Description**: Optionally, enter a description.
   - **Protocol**: The network protocol to use for sending out syslog messages. Defaults to `TCP`; `UDP` is also available.
   - **Load balancing**: Cribl Edge displays this option only when the **Protocol** is set to `TCP`. Toggle on to specify multiple destinations. See [Load Balancing Settings](#lb) below. 
   
   <br> Turning **Load balancing** off displays the following two additional fields: </br>

     - **Address**: Address/hostname of the receiver.

     - **Port**: Port number to connect to on the host.

    <br>If **Load balancing** is disabled and you notice that Cribl Edge is not sending data to all possible IP addresses, enable **Advanced Settings > Round-robin DNS**. </br>

     - **Record size limit**: Displayed when **Protocol** is set to `UDP`. Sets the maximum size of syslog messages. Defaults to `1500`, and must be ≤ `2048`. To avoid packet fragmentation, keep this value less than or equal to the MTU (maximum transmission unit) for the network path to the downstream receiver. When the message length exceeds the size limit, it is truncated and followed with an ellipsis ("...").

3. Next, you can configure the following **Optional Settings**:
     - **Facility**: Default value for message facility. If set, will be overwritten by the value of `__facility`. Defaults to `user`.
     - **Severity**: Default value for message severity. If set, will be overwritten by the value of `__severity`. Defaults to `notice`.
     - **App name**: Default value for application name. If set, will be overwritten by the value of `__appname`. Defaults to `Cribl`.
     - **Message format**: The syslog message format supported by the receiver. Defaults to `RFC3164`.
     - **Timestamp format**: The timestamp format to use when serializing an event's time field. Options include `Syslog` (default) and `ISO8601`. For details, see [Timestamp format](#format) below.
    - **Throttling**: Throttle rate, in bytes per second. Defaults to `0`, meaning no throttling. Multiple-byte units such as KB, MB, or GB are also allowed – for example, `42 MB`. When throttling is engaged, your **Backpressure behavior** selection determines whether Cribl Edge will handle excess data by blocking it, dropping it, or queueing it to disk. For details, see [Throttling](#throttle) below. 
   - **Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`.

   - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [Persistent Queue](#pq), [TLS](#tls), [Processing](#processing), [Timeout](#timeout), and [Advanced](#advanced-tab) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 

### Load Balancing Settings {#lb}

> Load balancing is available only when the **Protocol** is set to `TCP`.
>
{.box .info}

Toggling **Load balancing** on replaces the static **General Settings** > **Address** and **Port** fields with the following controls:

**Exclude current host IPs**: Appears when **Load balancing** is toggled on. It determines whether to exclude all IPs of the current host from the list of any resolved hostnames. Default is toggled off, which keeps the current host available for load balancing.

#### Destinations {#destinations}

Use the **Destinations** table to specify a known set of receivers on which to load-balance data. To specify more receivers on new rows, click **Add Destination**. Each row provides the following fields:

**Address**: Hostname of the receiver. Optionally, you can paste in a comma-separated list, in `<host>:<port>` format.

**Port**: Port number to send data to on this host.

**TLS**: Whether to inherit TLS configs from group setting, or disable TLS. Defaults to `inherit`.

**TLS servername**: Servername to use if establishing a TLS connection. If not specified, defaults to connection host (if not an IP). Otherwise, uses the global TLS settings. 

**Load weight**: Set the relative traffic-handling capability for each connection by assigning a weight (> `0`). This column accepts arbitrary values, but for best results, assign weights in the same order of magnitude to all connections. Cribl Edge will attempt to distribute traffic to the connections according to their relative weights.

The final column provides an `X` button to delete any row from the table.

For details on configuring all these options, see [About Load Balancing](load-balancing). 


### Timestamp Format {#format}

For `Syslog`, the timestamp format is `Mmm dd hh:mm:ss`, using the timezone of the syslog device. For example: `Jan 18 16:56:36`. </br>

For `ISO8601`, the basic timestamp format is `YYYY‑MM‑DD`. For example: `2023‑01‑18`. For extended formats, add hours, minutes, seconds, decimal fractions (optional), and timezone. For example: `2023‑01‑18T16:56:36.996‑08:00`. </br>

### Throttling {#throttle}

 When **Throttling** is used in conjunction with **Connection limit**, the throttling rate is applied per connection. For instance, if **Connection limit** is set to `2` and throttling is set to `3 MB`, each of the two connections can send data at a rate of up to 3 MB per second, resulting in a total potential throughput of 6 MB per second across both connections. 


### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.14/snippets/_destinations-persistent-queue-settings.md]

### TLS Settings (Client Side) {#tls}

**Enabled**: Defaults to toggled off. When toggled on:


**Validate server certs**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA – for example, the system's CA. Defaults to toggled on.

**Server name (SNI)**: Server name for the SNI (Server Name Indication) TLS extension. This must be a host name, not an IP address.

**Minimum TLS version**: Optionally, select the minimum TLS version to use when connecting.

**Maximum TLS version**: Optionally, select the maximum TLS version to use when connecting.

**Certificate name**: The name of the predefined certificate.
 
**CA certificate path**: Path on client containing CA certificates (in PEM format) to use to verify the server's cert. Path can reference `$ENV_VARS`.
 
**Private key path (mutual auth)**: Path on client containing the private key (in PEM format) to use.  Path can reference `$ENV_VARS`. **Use only if mutual auth is required**.
 
**Certificate path (mutual auth)**: Path on client containing certificates in (PEM format) to use. Path can reference `$ENV_VARS`. **Use only if mutual auth is required**. 
 
**Passphrase**: Passphrase to use to decrypt private key.

### Timeout Settings {#timeout}

> These timeout settings apply only to the TCP protocol.
>
{.box .info}

**Connection timeout**: Amount of time (in milliseconds) to wait for the connection to establish, before retrying. Defaults to `10000`.

**Write timeout**: Amount of time (milliseconds) to wait for a write to complete, before assuming connection is dead. Defaults to `60000`.

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Edge Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

### Advanced Settings {#advanced-tab}

The first three options are always displayed:

**Octet count framing**: When enabled, Cribl Edge prefixes messages with their byte count, regardless of whether the messages are constructed from `message` or `__syslogout`. When disabled, Cribl Edge omits prefixes, and instead appends a `\n` to messages. RFCs [5425](https://datatracker.ietf.org/doc/html/rfc5425) and [6587](https://datatracker.ietf.org/doc/html/rfc6587) describe how octet counting works in syslog.

**Log failed requests to disk**: Toggle on to make the payload of the most recently failed request available for inspection. See [Inspect Payload to Troubleshoot Closed Connections](#log-payload) below.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

The following options are added if you enable the [General Settings](#general-settings) tab's **Load balancing** option:

**DNS resolution period (seconds)**: Re-resolve any hostnames after each interval of this many seconds, and pick up destinations from A records.
- For TCP: This helps maintain up-to-date IP addresses for your TCP connections. Defaults to `600` seconds.
- For UDP: This reduces the frequency of DNS lookups for UDP destinations, improving performance. Defaults to `0` seconds, meaning DNS lookups occur for every outgoing message.

**Load balance stats period (seconds)**: Lookback traffic history period. Defaults to `300` seconds. If multiple receivers – that is, multiple A records – are behind a hostname, all resolved IPs will inherit the weight of the host unless each IP is specified separately. In Cribl Edge load balancing, IP settings take priority over those from hostnames.

**Connection limit**: Constrains the number of concurrent receiver connections per Worker Process, to limit memory utilization. If you set this to a number greater than `0`, Cribl Edge will randomly select that many discovered IPs to connect to on every DNS resolution period. Cribl Edge will rotate IPs in future resolution periods – monitoring weight and historical data, to ensure fair load balancing of events among IPs.

#### Inspect Payload to Troubleshoot Closed Connections {#log-payload}

When a downstream receiver closes connections from this Destination (or just stops responding), inspecting the payload of the most recently failed request can help you find the cause. For example:

* Suppose you send an event whose size is larger than the downstream receiver can handle. 
* Suppose you send an event that has a `number` field, but the value exceeds the highest number that the downstream receiver can handle.

When **Log failed requests to disk** is enabled, you can inspect the last failed request payload. Here is how:

1. In the Destination UI, navigate to the **Logs** tab.
2. Find a log entry with a `connection error` message. 
3. Expand the log entry. 
4. If the message includes the phrase `See payload file for more info`, note the path in the `file` field on the next line.

Now you have the path to the directory where Cribl Edge is storing the payload from the last failed request.

## Internal Fields {#internal}

Cribl Edge uses a set of internal fields to assist in forwarding data to downstream services. Fields for this Destination:

  * `__priority`
  * `__facility`
  * `__severity`
  * `__procid`
  * `__appname`
  * `__msgid`
  * `__syslogout`

## Syslog Format Options {#format}

Unlike other Cribl Edge Destinations, the Syslog Destination applies an additional post-processing step after the Pipeline(s) transform events. This additional step structures the data for compliance with the syslog transport protocol (RFC 3164 and/or RFC 5424) before it is transmitted to downstream services.

- With [RFC 3164](https://tools.ietf.org/html/rfc3164)-compliant messages, you’ll get `priority`, `timestamp`, `host`, and [`message`](#message), such that `message` includes `appname` and `procid`. You will not get `msgid` or `STRUCTURED-DATA`.
- With [RFC 5424](https://tools.ietf.org/html/rfc5424)-compliant messages – that is, structured data – Cribl Edge will send the contents of `event.structuredData`, whether or not that content is valid according to the RFC. Cribl Edge will make no attempt to frame, enclose, or validate the data. If present, structured data **should** be in the form `[ SD-ID *PARAM-NAME=PARAM-VALUE]` but Cribl Edge will not enforce this. Cribl Edge will send whatever is in that field, or, if the field is missing, send a dash (`-`).

In [**General Settings**](#general-settings), you can format the timestamps, format the message delivering the event, and set defaults for **Facility**, **Severity**, and **App name** (in RFC 5424, these are Facility, Severity, and APP-NAME, respectively).  

Here is an example of an RFC 3164-compliant syslog event:
- `<13>Jul 11 10:34:35 testbox testing[42]`

Here is an example of an RFC 5424-compliant syslog event:
- `<214>1 2022-07-11T18:58:45.000Z testbox testapp 42 - [exampleSDID@32473 iut="3" foo="bar" this="that"] Here is a message`

## Structure Syslog Output {#approaches}

Two approaches are available for specifying how the Syslog Destination will structure its output. One relies on the `message` field, and the other relies on the `__syslogout` field. The Syslog Destination determines what to do based on whether or not those fields are set, as the flowchart below makes clear. 

- [**Use `message` (Simplest Approach)**](#message): Cribl recommends using this option, which requires you to configure `message` and to leave `__syslogout` unset. When `message` is defined, Cribl Edge discards `_raw` and composes a new payload using the other syslog-related fields. Cribl Edge automatically processes the values of the `message`, `_time`, `host`, and other fields, and creates an ISO-compliant timestamp and a properly formatted event. In the flowchart, the rounded rectangle that reads "Use message + other fields ..." represents this approach. 

- **[Export `__syslogout` (Alternative Approach)](#syslogout)**: When you define `__syslogout`, Cribl Edge sends this field and its value as the entire syslog message, discards `_raw`, and ignores all the other fields. In the flowchart, the rounded rectangle that reads "Send __syslogout ..." represents this approach.

As the flowchart shows, if **neither** `message` nor `__syslogout` are set, Cribl Edge uses the existing `_raw` field as the `message` field
 (with or without a header), and prepends all the other syslog-related fields to the event. While this could be considered a third approach, Cribl does not recommend it, and we will not describe it further in this topic.

![Syslog output flow](syslog_destination_flow.0e821e963a.png)
{border="false"}

Before you configure this Destination, review the subsection below that explains the approach that you prefer.

### Use `message` (Simplest Approach) {#message}

> Cribl recommends this approach for environments where events enter Cribl Edge from the Syslog Source, because that Source supplies all required fields.
{.box .info}

This approach is the simplest and least error-prone because Cribl Edge creates the payload for you. The following fields are required:

  * `facility`
  * `severity`
  * `_time`
  * `host`
  * `message`

Cribl Edge will create a new syslog-adherent payload using these required fields. See [Field Logic and Behavior](#logic) below. To specify which RFC your events should adhere to, use **General Settings > Optional Settings > Message format**.

If these optional fields are included in the event, Cribl Edge will use them appropriately when constructing the payload:

  * `appname`
  * `host`
  * `procid`
  * `msgid` (RFC 5424 only)
  * `structuredData` (RFC 5424 only)

#### Field Logic and Behavior {#logic}

Let's examine the fields in a syslog-formatted event. Keep in mind that if Severity, Facility, or APP-NAME are not set on the event, Cribl Edge defaults to the corresponding value(s) [configured](#optionalsettings) in the Syslog Destination (**Severity**, **Facility**, and **App name**, respectively).  

- `__priority`: If this field is present Cribl Edge will use it and ignore `facility` and `severity`.
  
- The version (used for RFC 5424 only, and not a field that you can set) follows priority (with no spaces in between).

- `__facility` or `facility`: Cribl Edge uses this field to calculate priority. The RFCs define [Facility codes](https://en.wikipedia.org/wiki/Syslog#Facility).

- `__severity` or `severity`: Cribl Edge also uses this field to calculate priority. The RFCs define [Severity levels](https://en.wikipedia.org/wiki/Syslog#Severity_level).

Use either (a) `__facility` and `__severity` or (b) `facility` and `severity`. For these fields, avoid mixing double-underscore naming with no-underscore naming, because if you mix them, Cribl Edge will ignore one of them and you will get unintended results.

> If `__priority` is not set, Cribl Edge calculates the Priority from the values of Severity and Facility. Cribl Edge uses the formula: Priority = ((8 * Facility) + Severity). 
> - For example, if the `facility` is `13` (Security) and the `severity` is `2` (Critical), the `priority` will be `(13 * 8) + 2 = 106`.
{.box .info}

Now let's look at the rest of the fields. The list below shows field names in priority order: if both `__appname` and `appname` are present, Cribl Edge uses `__appname`.

- `_time`: The value of `_time` in Cribl events is in epoch format, but the syslog RFCs dictate that each event’s timestamp must be in human-readable format (see TIMESTAMP in [RFC 3164](https://datatracker.ietf.org/doc/html/rfc3164#section-4.1.2) or [RFC 5424](https://datatracker.ietf.org/doc/html/rfc5424#section-6.2.3)). When defining a Syslog Destination, you configure this with **General Settings** > **Timestamp format**. The `ISO8601` option ([ISO 8601 format](https://en.wikipedia.org/wiki/ISO_8601)) defines the method for specifying time zone and year, while the older `Syslog` format lacks this information. For this reason, Cribl recommends `ISO8601`.

- `host`: the value for the required host field in a syslog event (see HOSTNAME in [RFC 3164](https://datatracker.ietf.org/doc/html/rfc3164#section-4.1.2) or [RFC 5424](https://datatracker.ietf.org/doc/html/rfc5424#section-6.2.4)), following the timestamp.

- `__appname` or `appname`: The required application name (APP-NAME in [RFC 5424](https://datatracker.ietf.org/doc/html/rfc5424#section-6.2.5)). This is typically the name of the daemon or process that is logging any given event.

- `__procid` or `procid`: This optional field (PROCID in [RFC 5424](https://datatracker.ietf.org/doc/html/rfc5424#section-6.3)) is the Process ID. Use a numeric value, optionally surrounded with brackets – for example, `[4321]`. Cribl Edge will automatically adjust the spaces and syntax to ensure RFC-compliant formatting.

- `__msgid` or `msgid`: This optional field (MSGID in [RFC 5424](https://datatracker.ietf.org/doc/html/rfc5424#section-6.2.7) only) is the Message ID. 
- `structuredData`: This optional field (STRUCTURED-DATA in [RFC 5424](https://datatracker.ietf.org/doc/html/rfc5424#section-6.3) only) contains a namespace and zero or more key-value pairs. Cribl Edge does not validate whether it adheres to the RFC.

### Export `__syslogout` (Alternative Approach) {#syslogout}

> The most common use of this approach is to send to a non-syslog downstream receiver such as a raw TCP listener. While there is no native "raw TCP" Destination  in Cribl Edge, this approach – where you configure a Syslog Destination with the TCP protocol and `__syslogout` – can be an effective method for delivering raw data over TCP to a downstream receiver that does not enforce syslog RFCs. 
{.box .info}

When you set `__syslogout`, that field becomes the entire syslog message sent. Neither `_raw` nor any other metadata are sent downstream. 

If you enable [**Octet count framing**](#advanced-settings), Cribl Edge will prepend the number of bytes of the constructed `__syslogout` field to the message before sending it.

Cribl Edge does not check whether the value of `__syslogout` is RFC-compliant. This means that to send proper, RFC-compliant syslog messages, you must manually construct the `__syslogout` payload, starting with `_time`, for all the fields that Cribl Edge would automatically handle with the [first approach](#message) above.

> If the value of `__syslogout` fails to adhere to the syslog RFCs, a downstream syslog server may misinterpret the event; drop non-compliant events entirely; or, try to “fix” the format by supplying missing fields.
>
{.box .warning} 

## Troubleshooting

[Snippet not found: content/shared/4.14/snippets/_destinations-troubleshooting.md]