# Prometheus Remote Write


Cribl Edge supports receiving metric data from Prometheus instances that are configured to send data via the [remote write](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#remote_write) protocol.

> Type: **Push**  |  TLS Support: **YES** | Event Breaker Support: **No**
> 
> This Source assumes that incoming data is snappy-compressed.
>
{.box .info}

## Configuring Cribl Edge to Receive Metrics from Prometheus Remote Write Sources

From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**Push** > ] **Prometheus** > **Remote Write**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Push** > ] **Prometheus** > **Remote Write**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this Source definition. 

**Address**: Enter the hostname/IP to listen to. Defaults to `0.0.0.0`.

**Port**: Enter the port number to listen on..

**Remote Write API endpoint**: Enter the absolute path on which to listen for Prometheus requests. Defaults to `/write`, which will (in this example) expand as: `http://<your‑upstream‑URL>:<your‑port>/write`.

### Optional Settings

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Authentication

Select one of the following options for authentication:

- **None**: Don't use authentication.

- **Auth token**: Use HTTP token authentication. In the resulting **Token** field, enter the bearer token that must be included in the HTTP authorization header, or click **Generate** if you need a new token.

- **Auth token (text secret)**: Provide an HTTP token referenced by a secret. Select a stored text secret in the resulting drop-down, or click **Create** to configure a new secret.

- **Basic**: Displays **Username** and **Password** fields for you to enter HTTP Basic authentication credentials. Click **Generate** if you need a new password.

- **Basic (credentials secret)**: Provide username and password credentials referenced by a secret. Select a stored text secret in the resulting **Credentials secret** drop-down, or click **Create** to configure a new secret.

### TLS Settings (Server Side)

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

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](cloud-enterprise)), this tab exposes only the **Enable Persistent Queue** toggle. If enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB disk space allocated per PQ‑enabled Source, per Worker Process. 
> 
> The 1 GB limit is on uncompressed inbound data, and no compression is applied to the queue. This limit is not configurable. For configurable queue size, compression, mode, and other options below, use a [hybrid Group](cloud-enterprise#hybrid).
>
{.box .cloud}

**Enable Persistent Queue**: Defaults to `No`. When toggled to `Yes`:

**Mode**: Select a condition for engaging persistent queues.
- `Always On`: This default option will always write events to the persistent queue, before forwarding them to Cribl Edge's data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from Cribl Edge's data processing engine.

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the sender and writing the queue to disk. Defaults to `1000`. (This buffer is per connection, not just per Worker Process – and this can dramatically expand memory usage.)

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, etc. If not specified, Cribl Edge applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume on each Worker Process. Once this limit is reached, this Source will stop queueing data and block incoming data. Required, and defaults to `5` GB. Accepts positive numbers with units of `KB`, `MB`, `GB`, etc. Can be set as high as `1 TB`, unless you've [configured](settings-group-fleet#storage) a different **Max PQ size per Worker Process** in Fleet Settings.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Edge will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file is closed. Defaults to `None`; `Gzip` is also available.

> In Cribl Edge 4.1 and later, Source-side PQ's default **Mode** is `Always on`, to best ensure events' delivery. For details on optimizing this selection, see [Always On versus Smart Mode](persistent-queues-configuring#source-pq-busy).
> 
> You can optimize Workers' startup connections and CPU load at **Fleet Settings** > [Worker Processes](settings-group-fleet#processes).
>
{.box .info}

### Processing Settings

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality.

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Enable proxy protocol**: Toggle to `Yes` if the connection is proxied by a device that supports [Proxy Protocol](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) v1 or v2. This setting affects how the Source handles the [`__srcIpPort` field](#src-ip-port).

**Capture request headers**: Toggle this to `Yes` to add request headers to events, in the `__headers` field.

**Max active requests**: Maximum number of active requests allowed for this Source, per Worker Process. Defaults to `256`. Enter `0` for unlimited.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Socket timeout (seconds)**: How long Cribl Edge should wait before assuming that an inactive socket has timed out. The default `0` value means wait forever.

**Request timeout (seconds)**: How long to wait for an incoming request to complete before aborting it. The default `0` value means wait indefinitely.

**Keep-alive timeout (seconds)**: After the last response is sent, Cribl Edge will wait this long for additional data before closing the socket connection. Defaults to `5` seconds; minimum is `1` second; maximum is `600` seconds (10 minutes).

> The longer the **Keep‑alive timeout**, the more Cribl Edge will reuse connections. The shorter the timeout, the closer Cribl Edge gets to creating a new connection for every request. When request frequency is high, you can use longer timeouts to reduce the number of connections created, which mitigates the associated cost.
>
{.box .success}

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Edge uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

* `__criblMetrics`
* `__final`
* `__headers` – Added only when **Advanced Settings** > **Capture request headers** is set to `Yes`.
* `__inputId`
* `__srcIpPort` – See details [below](#src-ip-port).
* `_time`
* `_value`


### Overriding `__srcIpPort` with Client IP/Port {#src-ip-port}

The `__srcIpPort` field's value contains the IP address and (optionally) port of the Prometheus Remote Write client sending data to this Source.

When any proxies (including load balancers) lie between the Prometheus Remote Write client and the Source, the last proxy adds an `X‑Forwarded‑For` header whose value is the IP/port of the original client. With multiple proxies, this header's value will be an array, whose first item is the original client IP/port.

If `X‑Forwarded‑For` is present, and **Advanced Settings** > **Enable proxy protocol** is set to `No`, the original client IP/port in this header will override the value of `__srcIpPort`. 

If **Enable proxy protocol** is set to `Yes`, the `X‑Forwarded‑For` header's contents will **not** override the `__srcIpPort` value. (Here, the upstream proxy can convey the client IP/port without using this header.)

## Detecting Metrics' Types

Because Prometheus remote write requests don’t specify metrics' types, Cribl Edge applies the following rules to determine the type as we ingest them:

*   If the metric’s name ends with `_total`, `_sum`, `_count`, or `_bucket`, the type is set to `counter`.
    
*   Otherwise, the metric's type is set to `gauge`.
    
This is consistent with the type detection practiced by other services implementing the remote write protocol. See, for example, [New Relic](https://docs.newrelic.com/docs/integrations/prometheus-integrations/install-configure-remote-write/set-your-prometheus-remote-write-integration/#mapping)'s and [Elastic](https://www.elastic.co/guide/en/beats/metricbeat/master/metricbeat-metricset-prometheus-remote_write.html#_types_patterns)'s documentation.
        
Note that Cribl Edge supports the `timer` type in addition to `counter` and `gauge`.


