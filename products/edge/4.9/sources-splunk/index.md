# Splunk TCP Source



Cribl Edge supports receiving Splunk data from [Universal or Heavy Forwarders](http://docs.splunk.com/Documentation/Splunk/latest/Forwarding/Typesofforwarders).

> Type: **Push**  |  TLS Support: **YES** | Event Breaker Support: **YES**
>
{.box .info}

For examples of optimizing incoming Splunk events for downstream services, see our [Splunk Stream to Cribl Stream](/stream/usecase-splunk-stream/), [Splunk to Elasticsearch](/stream/usecase-splunk-elasticsearch/), and [Splunk to Exabeam](/stream/usecase-exabeam/) guides.

Also examine several [Splunk Forwarder/UF Packs](https://packs.cribl.io/?query=Splunk%20Forwarder) on the Cribl Packs Dispensary. These provide processing Pipelines to optimize Splunk Windows events and Universal Forwarder internal events. You can directly import these and adapt them to your needs.

## Configure Cribl Edge to Receive Splunk TCP Data

> Cribl Edge ships with a Splunk TCP Source preconfigured to listen on Port 9997. You can clone or directly modify this Source to further configure it, and then enable it.
>
{.box .success}

In Cribl Edge, configure Splunk TCP:

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
    - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**.
    - To configure via the [Routes](routes), select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). Select the Source you want. Next, select **Add Source**.
2. Configure the following under **General Settings**:
    - **Input ID**: Enter a unique name. The default Source is prefilled with the value `in_splunk_tcp`, which can't be changed via the UI. If you clone this Source, Cribl Edge will add `-CLONE` to the original **Input ID**.
    - **Description**: Optionally, enter a description.
    - **Address**: Enter hostname/IP to listen for Splunk data. For example, `localhost` or `0.0.0.0`.
    - **Port**: Enter port number.
3. Next, you can configure the following **Optional Settings**:
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [TLS](#tls), [Persistent Queue Settings](#pq), [Processing](#processing), [Auth tokens](#auth) and [Advanced](#advanced-settings) settings, or [Connected Destinations](#connected) outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**.


### TLS Settings (Server Side) {#tls}

**Enabled** defaults to `No`. When toggled to `Yes`:

**Certificate name**: Name of the predefined certificate.

**Private key path**: Server path containing the private key (in PEM format) to use. Path can reference `$ENV_VARS`.

**Passphrase**: Passphrase to use to decrypt private key.

**Certificate path**: Server path containing certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**CA certificate path**: Server path containing CA certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**Authenticate client (mutual auth)**: Require clients to present their certificates. Used to perform mutual authentication using SSL certs. Defaults to `No`. When toggled to `Yes`:

- **Validate client certs**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (for example, the system's CA). Defaults to `Yes`.

- **Common Name**: Regex that a peer certificate's subject attribute must match in order to connect. Defaults to `.*`. Matches on the substring after `CN=`. As needed, escape regex tokens to match literal characters. (For example, to match the subject `CN=worker.cribl.local`, you would enter: `worker\.cribl\.local`.) If the subject attribute contains Subject Alternative Name (SAN) entries, the Source will check the regex against all of those but ignore the Common Name (CN) entry (if any). If the certificate has no SAN extension, the Source will check the regex against the single name in the CN.

**Minimum TLS version**: Optionally, select the minimum TLS version to accept from connections.

**Maximum TLS version**: Optionally, select the maximum TLS version to accept from connections.

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.9/snippets/_sources-persistent-queue-settings.md]

### Processing Settings {#processing}



#### Event Breakers

Event Breakers are applied only to raw, unparsed data. Data originating from Splunk Heavy Forwarders or Universal Forwarders with sourcetypes configured for event breaking or indexed extractions bypasses Event Breakers, as it is already segmented into discrete events.

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied to the input data stream before the data is sent through the Routes. Defaults to `System Default Rule`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality.

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Auth Tokens {#auth}

**Add Token** : Click to add authorization tokens. Each token's section provides the fields listed below. If no tokens are specified, unauthorized access **will be permitted**.

**Token**: Shared secrets to be provided by any Splunk forwarder (`Authorization: <token>`). Click **Generate** to create a new secret.



**Description**: Optional description for this token.

### Advanced Settings {#advanced-settings}

**Enable Proxy Protocol**: Toggle to `Yes` if the connection is proxied by a device that supports [Proxy Protocol](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) v1 or v2.

**IP allowlist regex**: Regex matching IP addresses that are allowed to establish a connection. Defaults to `.*` (such as, all IPs).

**Max active connections**: Maximum number of active connections allowed per Worker Process. Defaults to `1000`. Set a lower value if connection storms are causing the Source to hang. Set `0` for unlimited connections.

**Max S2S version**: The highest version of the Splunk-to-Splunk protocol to expose during handshake. Defaults to `v4`; `v3` is also available.

**Use Universal Forwarder time zone**: Displayed (and enabled by default) only when **Max S2S version** is set to `v4`. Provides Event Breakers with a `__TZ` field, which derives events' time zone from UF-provided metadata. See [Using the UF Time Zone](#uf-time-zone) and [Configuring a Splunk Forwarder](#maxs2-warning), below.

**Drop control fields**: When set to `Yes`, this setting drops Splunk control fields such as `crcSalt` and `savedPort`. Defaults to `Yes`. Toggle to `No` if you want to retain these fields and store the data in the `__ctrlFields` internal field.

**Extract metrics**: Toggle to `Yes` if you're using Splunk's S2S protocol. When enabled, Cribl will identify and handle Splunk metric events, ensuring that metric events generated by Splunk are processed and forwarded as metrics.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

### Using the UF Time Zone {#uf-time-zone}

Under [Advanced Settings](#advanced-settings), the **Use Universal Forwarder time zone** toggle mitigates cases where incoming events have timestamp strings but no time zone information. For example:

```
12-15-2022 14:57:22.080 WARN TcpOutputFd [1607 TcpOutEloop] - Connect to 172.17.0.1:9997 failed. Connection refused
```

This gap can be problematic, especially if the originating Universal Forwarder is in a different time zone from the processing Edge Node.

The `__TZ` field is the solution. Event Breakers use the `__TZ` field to derive time zone information, enabling them to set the `_time` field correctly. Derived time zone information will appear in Cribl Edge's own logs as shown below:

![Time zone information in logs](time-zone-info-in-logs.90b76f1e1e.png)
{border="true"}

### Setting the Log Level for Connection Messages {#loglevel}

When a Splunk forwarder connects to Cribl Edge, Cribl Edge logs the following message at the `debug` level: `Connection with forwarder has been established successfully`.

To see this message, set the `:forwarders` level to `debug`.

Each message contains details specific to the forwarder, such as the protocol, Splunk version, or remote host, to name a few.

In some situations, logging each incoming connection can produce many messages, which can make it hard to find other messages.

You can adjust the level of these connection messages. To do so, follow these steps:

1. Select **Settings** > **Global Settings** > **Logging** > **Levels**.

1. Search for the channel that logs the connection messages. It will have a name in the form `input:<source-id>:forwarders`. For example: `input:in_splunk_tcp:forwarders`.

1. Set the channel to the log level you prefer, such as `debug` or `silly`.

1. Click **Save** to save your setting.

1. Commit and deploy the change.

## Internal Fields {#internal-fields}

Cribl Edge uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

* `__inputId`
* `__ctrlFields`
* `__s2sVersion` – value can be either `v3` or `v4`
* `__source`
* `__srcIpPort`
* `__TZ` – see [above](#uf-time-zone)

## Configuring a Splunk Forwarder {#config-splunk-fwd}

To configure a Splunk forwarder (UF, HF) use the following sample [outputs.conf](https://docs.splunk.com/Documentation/Splunk/latest/Admin/outputsconf) stanzas:

{{% tabs "logstream" "logstream" "outputs.conf (on-prem)" "cloud" "outputs.conf (Cribl Cloud)" %}}
{{% tab-item "logstream" %}}

```
[tcpout]
disabled = false
defaultGroup = cribl, <optional_clone_target_group>, ...
enableOldS2SProtocol = true

[tcpout:cribl]
server = [<cribl_ip>|<cribl_host>]:<port>, [<cribl_ip>|<cribl_host>]:<port>, ...
compressed = false
sendCookedData = true
# As of Splunk 6.5, using forceTimebasedAutoLB is no longer recommended. Ensure this is left at default for UFs
# forceTimebasedAutoLB = false
```

{{% /tab-item %}}
{{% tab-item "cloud" %}}

```
[tcpout]
disabled = false
defaultGroup = cribl
enableOldS2SProtocol = true

[tcpout:cribl]
server = default.main.<Your-Org-ID>.cribl.cloud:9997
# sslVerifyServerCert = true
sslRootCAPath = $SPLUNK_HOME/etc/auth/cacert.pem
compressed = false
useSSL = true
sendCookedData = true
```

{{% /tab-item %}}
{{% /tabs %}}

With a Cribl.Cloud Enterprise plan, generalize the above URL's `default.main` substring to `<group-name>.main` when sending to other Fleets.

If your use case requires compression, use [useClientSSLCompression vs compressed on forwarder](https://community.splunk.com/t5/Getting-Data-In/useClientSSLCompression-vs-compressed-on-forwarder/m-p/105481) to compress the data stream.



> ##### Preventing Data Loss with v3 {{< id `maxs2-warning` >}}
>
> If you set **Max S2S version** to `v3` and are using Splunk 9.1.0 or later, Cribl recommends that you use the `enableOldS2SProtocol = true` setting shown above to avoid data loss. If you are working with `v3` and a Splunk version earlier than 9.1.0, you should use `negotiateProtocolLevel = 0`. Depending on your environment, enabling `negotiateProtocolLevel` with a non-`0` value could cause Cribl Edge to not accept data from the forwarder.
>
> If you set **Max S2S version** to `v4`, these settings are not necessary. The Splunk receiver will detect which version is in use and automatically use the correct handler.
>
> See [Internal Fields](#internal-fields) for information on the `__s2sVersion` field.
>
{.box .warning}

## Troubleshooting

[Snippet not found: content/shared/4.9/snippets/_sources-troubleshooting.md]

### Splunk Forwarder Performance Issues

If you encounter performance issues with a Splunk Forwarder, Cribl recommends increasing the number of parallel ingestion Pipelines or increasing forwarder throughput. You can experiment with either or both of these settings.

To increase the number of parallel ingestion Pipelines, adjust the setting for `parallelIngestionPipelines` in `server.conf`. Experiment with values ranging from `2`–`4`.

To adjust forwarder throughput, increase the `maxKBps` value in `limits.conf`. The default value is `256`. A value of `0` removes all throttling from the forwarder.

### "Connection rejected" with a `reason` of "Too many connections"

The maximum number of active Splunk TCP connections has been exceeded per Worker Process. The default is 1000.

##### Recommendation

In the Splunk TCP input's Advanced Settings configuration, increase the Max Active Connections value, set it to 0 for unlimited, and/or increase the # of Worker Processes the Worker Node(s) are using.
