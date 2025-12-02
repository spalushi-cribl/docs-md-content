# ServiceNow Cloud Observability


The ServiceNow Cloud Observability Destination enables seamless integration with [ServiceNow's Cloud Observability platform](https://www.servicenow.com/products/observability.html). This Destination routes observability data, including logs, metrics, and traces, to ServiceNow for enhanced monitoring and analysis. To ensure your data conforms to the expected format, refer to the [Protocol section](#protocol) below.

To get started, connecting to ServiceNow Cloud Observability is simple. Grab your ServiceNow [Cloud Observability access token](https://docs.lightstep.com/docs/create-and-manage-access-tokens#create-access-tokens), pick your preferred protocol and endpoint, then add the token to your Destination settings. Need more detailed instructions or configuration options? See below!

> Type: Streaming | TLS Support: Configurable | PQ Support: Yes 
>
{.box .info} 

## Configuring a ServiceNow Cloud Observability Destination {#configuring}

In Cribl Edge, set up a ServiceNow Cloud Observability Destination.

1. From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:
   * To configure via [QuickConnect](quickconnect), click **Routing** then **QuickConnect** (Cribl Stream) or **Collect** (Cribl Edge). Next, click **Add Destination** and from the resulting drawer's tiles, select **ServiceNow Cloud Observability**. Next, click either **Add Destination** or (if displayed) **Select Existing**.
   * To configure via the [Routes](routes), click **Data** then **Destinations** (Cribl Stream) or **More** then **Destinations** (Cribl Edge). Select **ServiceNow Cloud Observability** from the list of tiles or the **Destinations** list. Next, click **Add Destination** to open a **New Destination** modal.

1. In the Destination modal, configure the following under **General Settings**:
    * **Output ID**: Enter a unique name to identify this output definition. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**.
    * **OTLP version**: Already set to `1.3.1` and cannot be changed.
    * **Protocol**: Use the drop-down to choose the protocol to use when sending data:
      * `gRPC` (default): Uses Protocol Buffers (Protobuf) for serialization.
      * `HTTP`: Uses Binary Protobuf encoding (does not support JSON Protobuf).
    * **Endpoint**: The endpoint to send data to ServiceNow. Default options are `ingest.lightstep.com:443` (United States) and `ingest.eu.lightstep.com:443` (Europe).
      * You can specify a custom endpoint using either a valid URL or an IP address (IPv4 or IPv6). Enclose IPv6 addresses in square brackets (for example, `[2001:db8:85a3:0000:0000:8a2e:0370:7334]`). 
      * To proxy outbound HTTP/S requests, see [System Proxy Configuration](proxy-config).
    * **Auth token (text secret)**: Create or select a Cribl [stored text secret](/stream/securing-secrets) that contains your ServiceNow [Cloud Observability access token](https://docs.lightstep.com/docs/create-and-manage-access-tokens#create-access-tokens), ensuring secure authentication for data transmission.
    * **Optional Settings**:
      * When using `HTTP`, you can [customize the path](#custom-path) instead of using the default cloud endpoints. This is useful when integrating with a hosted Lightstep satellite.
      * **Backpressure behavior**: Whether to `Block`, `Drop`, or [`Persistent Queue`](#pq) events when all receivers are exerting backpressure.
      * **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

1. Depending on the protocol you've selected, you have the following options:
   * **gRPC** provides [TLS](#tls) settings.
   * **HTTP** provides [retry](#retries) options, in addition to [custom paths](#custom-path).

1. The **Processing Settings** and **Advanced Settings** provide granular control over how data is handled and transmitted.
   * [Processing Settings](#processing) allow you to define pre-processing and post-processing Pipelines, ensuring that events conform to the relevant Protobuf specifications for traces, metrics, and logs. This includes adding metadata, applying transformations, and conditioning the data.
   * [Advanced Settings](#advanced-settings) offer additional configuration options that enhance the flexibility and performance of data transmission. 
     * For `gRPC`, this includes connection timeout, keep-alive time, and metadata headers.
     * For `HTTP`, this includes options like custom path overrides for traces, metrics, and logs, as well as settings for validating server certificates and using round-robin DNS. HTTP is simpler to set up and more widely compatible with existing tools and platforms.

1. Click **Save**, then **Commit & Deploy**.

1. Use the **Test** tab in the Destination’s configuration modal to validate the configuration and connectivity of your Destination.

#### Custom HTTP Path {#custom-path}

To send data to a custom location, override the default endpoint paths. When the **Protocol** is set to `HTTP`, this Destination sends data to the following endpoints by default:

* Traces: `<endpoint>/traces/otlp/v0.9`
* Metrics: `<endpoint>/metrics/otlp/v0.9`
* Logs: `<endpoint>/v1/logs`

`<endpoint>` refers to the base URL you specified in the **Endpoint** setting above.

To override a default path, provide only the desired path in the corresponding field: **Traces endpoint override**, **Metrics endpoint override**, or **Logs endpoint override**.

### TLS Settings (Client Side) {#tls}

TLS is available only when **General Settings** > **Protocol** is set to `gRPC`.

**Use TLS** Default is toggled off. When toggled on:

**Validate server certs**: Toggle on (default) to reject certificates that are **not** authorized by a CA in the **CA certificate path**, nor by another trusted CA (for example, the system's CA).

**Minimum TLS version**: Optionally, select the minimum TLS version to use when connecting.

**Maximum TLS version**: Optionally, select the maximum TLS version to use when connecting.

**Certificate name**: The name of the predefined certificate.
 
**CA certificate path**: Path on client containing CA certificates (in PEM format) to use to verify the server's cert. Path can reference `$ENV_VARS`.
 
**Private key path (mutual auth)**: Path on client containing the private key (in PEM format) to use.  Path can reference `$ENV_VARS`. **Use only if mutual auth is required**.
 
**Certificate path (mutual auth)**: Path on client containing certificates in (PEM format) to use. Path can reference `$ENV_VARS`. **Use only if mutual auth is required**. 
 
**Passphrase**: Passphrase to use to decrypt private key.

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.12/snippets/_destinations-persistent-queue-settings.md]

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output – both metric events, as dimensions; and log events, as labels. Supports wildcards. 

By default, includes `cribl_pipe` (Cribl Edge Pipeline that processed the event). 

Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

### Retries {#retries}

> This tab is displayed when **General Settings** > **Protocol** is set to `HTTP`. 
>
{.box .info}

[Snippet not found: content/shared/4.12/snippets/_destinations-http-retries.md]

### Advanced Settings  {#advanced-settings}

This tab's options depend on whether **General Settings** > **Protocol** is set to the `gRPC` or `HTTP` transport.

**Compression**: Compression type to apply to messages sent to the OpenTelemetry endpoint. `Gzip` (the default) and `None` are available for both protocols; `Deflate` is available for gRPC only.

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30` sec.

**Request concurrency**: Maximum number of concurrent requests before blocking. This is set per Worker Process. Defaults to `5`.

**Body size limit (KB)**: Maximum size of the request body before compression. Defaults to `2048` KB. The actual request body size might exceed the specified value because the Destination adds bytes when it writes to the downstream receiver. Cribl recommends that you experiment with the **Body size limit** value until downstream receivers reliably accept all events.

**Flush period (sec)**: Maximum time between requests. Low values could cause the payload size to be smaller than the configured **Max body size**. Defaults to `1` sec.

**Auth token name**: Name of the header or metadata key that will carry your ServiceNow Cloud Observability access token during data transmission. The default is `lightstep-access-token` and is customizable to match your specific requirements.

**Environment**: Optionally, specify a single Git branch on which to enable this configuration. If this field is empty, the config will be enabled everywhere.

#### Additional Settings for gRPC {#grpc}

When **General Settings** > **Protocol** is set to `gRPC`, the **Advanced Settings** tab adds the following options.

**Connection timeout**: Amount of time (milliseconds) to wait for the connection to establish before retrying. Defaults to `10000` (10 sec.).

**Keep alive time (seconds)**: How often the sender should ping the peer to keep the connection alive. Defaults to `30`.

**Metadata**: Extra information to send with each gRPC request. Click **Add Metadata** to add each item as a **Key**-**Value** pair. The **Key** field is arbitrary. The **Value** field is a JavaScript expression that is evaluated just once, when this Destination is initialized. If you pass credentials as metadata, Cribl recommends using [C.Secret()](cribl-reference#secret).

#### Additional Settings for HTTP {#http}

When **General Settings** > **Protocol** is set to `HTTP`, the **Advanced Settings** tab adds the following options.

**Validate server certs**: Toggle on to reject certificates that are **not** authorized by a CA in the **CA certificate path**, nor by another trusted CA (e.g., the system's CA).

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Edge will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.

**Keep alive**: By default, Cribl Edge sends `Keep-Alive` headers to the remote server and preserves the connection from the client side up to a maximum of 120 seconds. Toggle this off if you want Cribl Edge to close the connection immediately after sending a request.

**Extra HTTP headers**: Click **Add Header** to define additional HTTP headers to pass to all events. Each row is a **Name**‑**Value** pair. Values will be sent encrypted. You can also add headers dynamicall,y on a per-event basis, in the `__headers` field.

**Safe headers**: Add headers here to declare them as safe to log in plaintext. (Sensitive headers such as `authorization` will always be redacted, even if listed here.) Use a tab or hard return to separate header names.

## Protocol and Transport Support {#protocol}

This Destination leverages the OpenTelemetry Protocol (OTLP) [v1.3.1](https://github.com/open-telemetry/opentelemetry-proto/tree/v1.3.1) to send observability data to ServiceNow. It supports both gRPC and HTTP protocols, ensuring flexibility in data transmission. OTLP defines Protocol buffer (Protobuf) schemas for its payloads (requests and responses). With the HTTP transport, this Destination supports Binary Protobuf payload encoding, but currently does not support JSON Protobuf.

When configuring Pipelines (including pre-processing and post-processing Pipelines), you need to ensure that events sent to this Destination conform to the relevant Protobuf specification.

- For traces, [opentelemetry/proto/trace/v1/trace.proto](https://github.com/open-telemetry/opentelemetry-proto/blob/v1.3.1/opentelemetry/proto/trace/v1/trace.proto)
- For metrics, [opentelemetry/proto/metrics/v1/metrics.proto](https://github.com/open-telemetry/opentelemetry-proto/blob/v1.3.1/opentelemetry/proto/metrics/v1/metrics.proto)
- For logs, [opentelemetry/proto/logs/v1/logs.proto](https://github.com/open-telemetry/opentelemetry-proto/blob/v1.3.1/opentelemetry/proto/logs/v1/logs.proto)

This Destination will drop non-conforming events. If the Destination encounters a parsing error when trying to convert an event to OTLP, it discards the event and Cribl Edge logs the error.

## Troubleshooting

[Snippet not found: content/shared/4.12/snippets/_destinations-troubleshooting.md]