# Syslog


Cribl Stream supports receiving syslog data, whether structured according to [RFC 3164](https://tools.ietf.org/html/rfc3164) or [RFC 5424](https://tools.ietf.org/html/rfc5424). This Source supports message-length prefixes according to [RFC 5425](https://datatracker.ietf.org/doc/html/rfc5425#section-4.3.1) or [RFC 6587](https://datatracker.ietf.org/doc/html/rfc6587#section-3.4.1).

> Type: **Push**  |  TLS Support: **YES** | Event Breaker Support: **No**
> 
> For details on how to replace your syslog server with Cribl Stream, see [Syslog Best Practices](/stream/usecase-syslog). 
>
{.box .info}

## Configuring Cribl Stream to Receive Data over Syslog

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**Push** > ] **Syslog**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Push** > ] **Syslog**. Next, click **New Source** to open a **New Source** modal that provides the options below.

> Cribl Stream ships with a Syslog Source preconfigured to listen for both UDP and TCP traffic on Port 9514. You can clone or directly modify this Source to further configure it, and then enable it.
>
{.box .success}

### General Settings

**Input ID**: Enter a unique name to identify this Syslog Source definition. 

**Address**: Enter the hostname/IP on which to listen for data., E.g. `localhost` or `0.0.0.0`.

**UDP port**: Enter the UDP port number to listen on. Not required if listening on TCP.

> The maximum supported inbound UDP message size is 16,384 bytes.
>
{.box .warning}

**TCP port**: Enter the TCP port number to listen on. Not required if listening on UDP.

> Sending large numbers of UDP events per second can cause Cribl.Cloud to drop some of the data.
> This results from restrictions of the UDP protocol.
> 
> To minimize the risk of data loss, deploy a hybrid Stream Worker Group
> with customer-managed Worker Nodes as close as possible to the UDP senders.
> Cribl also recommends tuning the OS UDP buffer size.
>
{.box .cloud}

#### Optional Settings

**Fields to keep**: List of fields from source data to retain and pass through. Supports wildcards. Defaults to `*` wildcard, meaning keep all fields. Fields **not** specified here (by wildcard or specific name) will be removed from the event.

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Stream. Use a tab or hard return between (arbitrary) tag names.

### TLS Settings (TCP Only)

**Enabled** defaults to `No`. When toggled to `Yes`:

**Certificate name**: Name of the predefined certificate.

**Private key path**: Server path containing the private key (in PEM format) to use. Path can reference `$ENV_VARS`.

**Passphrase**: Passphrase to use to decrypt private key.  

**Certificate path**: Server path containing certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**CA certificate path**: Server path containing CA certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**Authenticate client (mutual auth)**: Require clients to present their certificates. Used to perform mutual authentication using SSL certs. Defaults to `No`. When toggled to `Yes`:

- **Validate client certs**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (e.g., the system's CA). Defaults to `No`.

- **Common name**: Regex matching subject common names in peer certificates allowed to connect. Defaults to `.*`. Matches on the substring after `CN=`. As needed, escape regex tokens to match literal characters. E.g., to match the subject `CN=worker.cribl.local`, you would enter: `worker\.cribl\.local`.

**Minimum TLS version**: Optionally, select the minimum TLS version to accept from connections.

**Maximum TLS version**: Optionally, select the maximum TLS version to accept from connections.

### Persistent Queue Settings {#pq}

In this section, you can optionally specify [persistent queue](persistent-queues) storage, using the following controls. This will buffer and preserve incoming events when a downstream Destination is down, or exhibiting backpressure.

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](deploy-cloud#enterprise)), this tab exposes only the **Enable Persistent Queue** toggle. If enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB disk space allocated per Worker Process.
>
{.box .cloud}

**Enable Persistent Queue**: Defaults to `No`. When toggled to `Yes`:

**Mode**: Select a condition for engaging persistent queues.
- `Always On`: This default option will always write events to the persistent queue, before forwarding them to Cribl Stream's data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from Cribl Stream's data processing engine.

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the Source. Defaults to `1000`.

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, etc. If not specified, Cribl Stream applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume, on each Worker Process. Once this limit is reached, Cribl Stream will stop queueing data, and will apply the **Queue‑full behavior**. Enter a numeral with units of KB, MB, etc. If not specified, the implicit `0` default will enable Cribl Stream to fill all available disk space on the volume.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Stream will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file is closed. Defaults to `None`; `Gzip` is also available.

> As of Cribl Stream 4.1, Source-side PQ's default **Mode** changed from `Smart` to `Always on`. This option more reliably ensures events' delivery, and the change does not affect existing Sources' configurations. However:
> - If you create Stream Sources programmatically, and you want to enforce the previous `Smart` mode, you'll need to update your existing code.
> - If you enable `Always on`, this can reduce data throughput. As a trade-off for data durability, you might need to either accept slower throughput, or provision more machines/faster disks.
> - You can optimize Workers' startup connections and CPU load at **Group Settings** > [Worker Processes](settings-group-fleet#processes).
>
{.box .warning}

### Processing Settings

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Enable Proxy Protocol**: Toggle to `Yes` if the connection is proxied by a device that supports [Proxy Protocol](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) v1 or v2.

**Single msg per UDP**: Enable this to treat received UDP packet data as a full syslog message. With the `No` default, Cribl Stream will treat newlines within the packet as event delimiters.

**Octet count framing**: Toggle to `Yes` if messages are prefixed with a byte length, according to RFC 5425 or RFC 6587. The default setting (`No`) applies non-transparent framing using `\n` as the delimiter. The framing method utilized by this input will be applied to all events received by this input. Therefore, if your syslog devices use a mixture of framing types (non-transparent vs. octet count), you will need to use a separate Syslog Source for each framing type. Additional inputs will necessitate separate ports.

**Allow non-standard app name**: Toggle to `Yes` to allow hyphens to appear in an RFC 3164–formatted Syslog message's `TAG` section. For details, see [TAG Section Processing](#tag).

**IP whitelist regex**: Regex matching IP addresses that are allowed to send data. Defaults to `.*` (i.e., all IPs).

**Max buffer size (events)**: Maximum number of events to buffer when downstream is blocking. The buffer is only in memory. (This setting is applicable only to UDP syslog.)

**Default timezone**: Timezone to assign to timestamps that omit timezone info. Accept the default  `Local` value, or use the drop-down list to select a specific timezone by city name or GMT/UTC offset.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source's data to one or more Destinations via independent, direct connections.

## `TAG` Section Processing {#tag}

Cribl Stream will try to parse `appname` out of a syslog message's `TAG` section, even when the message is not RFC 5424–formatted. 

This practice means that the `TAG` section can contain any alphanumeric character or an underscore (`_`). When the **Allow–non‑standard app name** option is enabled, Cribl Stream will also process hyphens that appear in an RFC 3164–formatted Syslog message's `TAG` section. If Cribl Stream encounters a hyphenated `appname`, it will continue processing to find `procid`. (This setting has no effect on RFC 5424–formatted messages.)

If the `TAG` section contains any non-alphanumeric character, Cribl Stream will treat that character as the termination of the `TAG` section, and as the starting character of the `CONTENT` section.

## `CONTENT` Section Processing {#content}

Cribl Stream will try to parse `procid` from the beginning of the `CONTENT` section if this section is directly after the `TAG` section, and if it either follows a `:` or is surrounded by `[]`. This process occurs regardless of the **Allow–non‑standard app name** setting.

## What Fields to Expect

To create fields, the Syslog Source requires messages to comply with RFC 3164 or RFC 5424. If messages sent to the Source comply with neither of these RFCs, you'll see the following fields:

- `_raw`: Contains the whole message, but no other fields broken out.
- `_time`: Contains the time the message was received by Cribl Stream.
- `__srcIpPort <udp|tcp>:<port>`: See [Internal Fields](#internal-fields).
- `__syslogFail`: See [Internal Fields](#internal-fields).
- `host`: IP address of the device sending the incoming message.

If messages sent to the Source comply with either RFC 3164 or RFC 5424, fields that the RFC deems guaranteed will always be there, but fields deemed optional might or might not be. Once Cribl Stream parses the required fields and any optional fields, what remains is the actual message.

To see this in real life, install the `cribl-syslog-input` Pack and preview the `RFC5424-RFC3164.log` sample file.

RFC Name for Field | Cribl Name for Field | Guaranteed? | Notes
--- | --- | --- | --- 
`PRI` | `facility`, `facilityName`, `severity`, `severityName` | Yes | `PRI` is a bitwise representation of Facility and Severity, which is how Cribl Stream can derive values for all four of its fields.
`TIMESTAMP` | `_time` | Yes | This field's format differs depending on which RFC the messages adhere to. Cribl Stream converts it to UNIX epoch time.  
`HOSTNAME` | `host` | Yes |  
`APP-NAME` | `appname` | No | 
`PROCID` | `procid` | No |
`MSGID` | `msgid` | No | 
`STRUCTURED-DATA` | `structuredData` | No |
`MSG` | `message` | Yes 

## Internal Fields {#internal-fields}

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but are accessible and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

* `__inputId`
* `__srcIpPort <udp|tcp>:<port>`: Identifies the port of the sending socket.
* `__syslogFail`: `true` for data that fails RFC 3164/5424 validation as syslog format.



## Troubleshooting Resources {#troubleshooting}

[Cribl University](https://cribl.io/university/) offers an Advanced Troubleshooting > [Source Integrations: Syslog](https://university.cribl.io/#/online-courses/c1741eed-c361-4095-b66f-c8592e27da28) short course. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses and [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0).
