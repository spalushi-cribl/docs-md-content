# Metrics Source


Cribl Stream supports receiving metrics in these wire formats/protocols: [StatsD](#statsd), [StatsD Extended](#statsd-extended), and [Graphite](#graphite). Automatic protocol detection happens on the first line received over a TCP connection or a UDP packet. Lines not matching the detected protocol are dropped.

> Type: **Push**  |  TLS Support: **No** | Event Breaker Support: **No**
>
> Cribl Edge Workers support System Metrics only when running on Linux, not on Windows.
>
{.box .info}

## Configure Cribl Stream to Receive Metrics

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
     - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**.
     - To configure via the [Routes](routes), select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
    - **Input ID**: Enter a unique name to identify this Source definition. If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**.
    - **Description**: Optionally, enter a description.
    - **Address**: Enter the hostname/IP to listen to. Defaults to `0.0.0.0`.
    - **UDP port**: Enter the UDP port number to listen on. Not required if listening on TCP.For detail, see [UDP Port Considerations](#consider).
    - **TCP port**: Enter the TCP port number to listen on. Not required if listening on UDP.
3. Next, you can configure the following **Optional Settings**:
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [TLS](#tls), [Persistent Queue Settings](#pq), [Processing](#processing) and [Advanced](#advanced-settings) settings, or [Connected Destinations](#connected) outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**.

### UDP Port Considerations {#consider}

Sending large numbers of UDP events per second can cause Cribl.Cloud to drop some of the data. This results from restrictions of the UDP protocol. To minimize the risk of data loss, deploy a customer-managed (hybrid) Stream Worker Group with Worker Nodes as close as possible to the UDP senders. Cribl also recommends tuning the OS UDP buffer size.

### TLS Settings (TCP Only) {#tls}

**Enabled**: Defaults to toggled off. When toggled on:

**Certificate name**: Name of the predefined certificate.

**Private key path**: Server path containing the private key (in PEM format) to use. Path can reference `$ENV_VARS`.

**Passphrase**: Passphrase to use to decrypt private key.

**Certificate path**: Server path containing certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**CA certificate path**: Server path containing CA certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**Authenticate client (mutual auth)**: Require clients to present their certificates. Used to perform mutual authentication using SSL certs. Default is toggled off. When toggled on:

- **Validate client certificates**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (for example, the system's CA). Default is toggled on.

- **Common name**: Regex that a peer certificate's subject attribute must match in order to connect. Defaults to `.*`. Matches on the substring after `CN=`. As needed, escape regex tokens to match literal characters. (For example, to match the subject `CN=worker.cribl.local`, you would enter: `worker\.cribl\.local`.) If the subject attribute contains Subject Alternative Name (SAN) entries, the Source will check the regex against all of those but ignore the Common Name (CN) entry (if any). If the certificate has no SAN extension, the Source will check the regex against the single name in the CN.

**Minimum TLS version**: Optionally, select the minimum TLS version to accept from connections.

**Maximum TLS version**: Optionally, select the maximum TLS version to accept from connections.

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.13/snippets/_sources-persistent-queue-settings.md]

### Processing Settings {#processing}

#### Fields

[Snippet not found: content/shared/4.13/snippets/_sources-add-fields.md]

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

### Advanced Settings {#advanced-settings}

**Enable Proxy Protocol**: Toggle on if the connection is proxied by a device that supports [Proxy Protocol](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) v1 or v2.

**IP allowlist regex**: Regex matching IP addresses that are allowed to send data. Defaults to `.*` (i.e., all IPs.)

**Buffer size limit (events)**: Maximum number of events to buffer when downstream is blocking. Defaults to `1000`.

**UDP socket buffer size (bytes)**: Optionally, set the SO_RCVBUF socket option for the UDP socket. This value tells the operating system how many bytes can be buffered in the kernel before events are dropped. Leave blank to use the OS default. Min: `256`. Max: `4294967295`.

It may also be necessary to increase the size of the buffer available to the `SO_RCVBUF` socket option. Consult the documentation for your operating system for a specific procedure.

> Setting this value will affect OS memory utilization.
>
{.box .warning}

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

  * `__srcIpPort`
  * `__metricsInType`

## Metric Event Schema and Destination Support

Metric data is read into the following event schema:

```
_metric - the metric name
_metric_type - the type of the metric (gauge, counter, timer)
_value - the value of the metric
_time - metric_time or Date.now()/1000
dim1 - value of dimension1
dim2 - value of dimension2
....
```

Cribl Stream places sufficient information into a field called  `__criblMetric` to enable these events to be properly serialized out to any metric outputs (independent of the input type).

The following Destinations natively support the `__criblMetric` field:

* Splunk
* Splunk HEC
* InfluxDB
* Statsd
* Statsd Extended
* Graphite

## Data Format/​Protocol Examples

#### StatsD

Format: `MetricName:value|type`

```shell {title="StatsD Example"}
metric1:100|g
metric2:200|ms
metric.dot.3:300.16|c
```

See the StatsD [repo](https://github.com/statsd/statsd).

#### StatsD Extended

Format: `MetricName:value|type|#dim=value,dim2=value`

```shell {title="StatsD Extended Example"}
metric1:100|g|#dim1:val1,dim2:val2,dim3:val3
metric2:200|ms|#dim1:val1,dim2:val2,dim3:val3
metric.dot.3:300.16|c|#dim1:val1,dim2:val2,dim3:val3
```

#### Graphite

Format: `MetricName[;dim1=val1[;dim2=val2]] value time`

```shell {title="Graphite Example with Dimensions"}
metric1;dim1=val1;dim2=val2 100 9999
metric2;dim1=val1;dim2=val2 200 9999
metric.dot.3;dim1=val1;dim2=val2 300.16 9999.16
```

```shell {title="Graphite Example without Dimensions"}
metric1 100 9999
metric2 200 9999
metric.dot.3 300.16 9999.16
```

See the Graphite (also known as Carbon) [plaintext protocol](https://graphite.readthedocs.io/en/latest/feeding-carbon.html#the-plaintext-protocol).

## Troubleshooting

[Snippet not found: content/shared/4.13/snippets/_sources-troubleshooting.md]
