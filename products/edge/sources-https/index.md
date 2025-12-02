# HTTP/S (Bulk API) Source


Cribl Edge supports receiving data over HTTP/S from Cribl Bulk API, [Splunk HEC](https://dev.splunk.com/enterprise/docs/dataapps/httpeventcollector/), and [Elastic Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html) endpoints.

> Type: **Push**  |  TLS Support: **YES** | Event Breaker Support: **No**
>
> This Source supports gzip-compressed inbound data when the `Content‑Encoding: gzip` connection header is set.
>
{.box .info}

## Configure Cribl Edge to Receive Data over HTTP(S)

> Cribl Edge ships with an HTTP Source preconfigured to listen on Port 10080, and on several default endpoints. You can clone or directly modify this Source to further configure it, and then enable it.
>
{.box .success}

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
     - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**.
     - To configure via the [Routes](routes), select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
    - **Input ID**: Enter a unique name. The default Source is prefilled with the value `http`, which can't be changed via the UI. If you clone this Source, Cribl Edge will add `-CLONE` to the original **Input ID**.
    - **Description**: Optionally, enter a description.
    - **Address**: Enter the hostname/IP on which to listen for HTTP(S) data. (For example, `localhost` or `0.0.0.0`.)
    - **Port**: Enter the port number to listen on.
3. Under **Authentication**, enter the **Auth tokens**:
     - **Token**: Shared secrets to be provided by any client (`Authorization: <token>`). Click **Generate** to create a new secret. If empty, unauthorized access **will be permitted**.
     - **Description**: Optionally, enter a description.
     - **Fields**: Fields to add to events referencing this token. Each field is a **Name/Value** pair, where **Value** is a JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.) For details, see [Authentication Fields](#fields).
4. Next, you can configure the following **Optional Settings**:
    - **Cribl HTTP event API**: Base path on which to listen for Cribl HTTP API requests. To construct the actual endpoint, Cribl Edge will append `/_bulk` to this path. For example, with the default value of `/cribl`, your senders should send events to a `/cribl/_bulk` path.  Use an empty string to disable.
    - **Elastic API endpoint** (for [Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html)): Base path on which to listen for Elasticsearch API requests. For details, see [Elastic API Endpiont](#api).
    - **Splunk HEC endpoint**: Absolute path on which to listen for Splunk HTTP Event Collector (HEC) API requests. Use an empty string to disable. Default entry is `/services/collector`. For details, see [Splunk HEC Endpoint](#hec).
    - **Enable Splunk HEC acknowledgements**: Whether to enable Splunk HEC acknowledgements. Default is toggled off.
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

5. Optionally, you can adjust the [TLS](#tls), [Persistent Queue Settings](#pq), [Processing](#processing) and [Advanced](#advanced-settings) settings, or [Connected Destinations](#connected) outlined in the sections below.
6. Select **Save**, then **Commit & Deploy**.

### Elastic API Endpoint {#api}
 Currently, the only supported option is the default `/elastic`, to which Cribl Edge will append `/_bulk`. So, your senders should send events to an `/elastic/_bulk` path. Other entries are faked as success. Use an empty string to disable.

Cribl generally recommends that you use the dedicated [Elasticsearch API](sources-elastic) Source instead of this endpoint. The Elastic API implementation here is provided for backward compatibility, and for users who want to ingest multiple inputs on one HTTP/S port.

### Splunk HEC Endpoint {#hec}

This Splunk HEC implementation is an **event** (such as, not **raw**) endpoint. For details, see [Splunk's documentation](https://docs.splunk.com/Documentation/Splunk/latest/Data/UsetheHTTPEventCollector). To send data to it from a HEC client, use either `/services/collector` or `/services/collector/event`. (See the [examples](#examples) below.)

The [system default](event-breakers#system-default-rule) Event Breaker is automatically applied, which enforces a limit of 51,200 bytes per event.

Cribl generally recommends that you use the dedicated [Splunk HEC](sources-splunk-hec) Source instead of this endpoint. The Splunk HEC implementation here is provided for backward compatibility, and for users who want to ingest multiple inputs on one HTTP/S port.

### Authentication Fields {#fields}

By default, fields specified here will override fields of the same name in incoming events. However, you can customize this behavior to allow event fields to take precedence. For example, to allow the event's `index` field to take precedence, you could use this expression: `` `event.index || 'defaultIndex'` ``.

Fields add context to events based on the token used. This can be useful for tagging events with metadata such as source or environment, enhancing security by avoiding direct use of authorization values in Routes and Pipelines.

See also [Periodic Logging](#logging) for information on how auth tokens affect product logging.

### TLS Settings (Server Side) {#tls}

**Enabled**: Defaults to toggled off. When toggled on:

**Certificate**: Name of the predefined certificate.

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

In the **Persistent Queue Settings** tab, you can optionally specify persistent queue storage, using the following controls. Persistent queue buffers and preserves incoming events when a downstream Destination has an outage or experiences backpressure.

Before enabling persistent queue, learn more about persistent queue behavior and how to optimize it with your system:

- [About Persistent Queues](/persistent-queues)
- [Optimize Source Persistent Queues (sPQ)](/persistent-queues-sources)

> On Cribl-managed [Cloud](deploy-cloud) Workers (with an [Enterprise plan](cloud-enterprise)), this tab exposes only the **Enable persistent queue** toggle. If enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB disk space allocated per PQ‑enabled Source, per Worker Process.
>
> The 1 GB limit is on uncompressed inbound data, and the queue does not perform any compression. This limit is not configurable. For configurable queue size, compression, mode, and other options below, use a [hybrid Group](cloud-enterprise#hybrid).
>
{.box .cloud}

**Enable persistent queue**: Default is toggled off. When toggled on:

**Mode**: Select a condition for engaging persistent queues.

- `Always On`: This default option will always write events to the persistent queue, before forwarding them to the Cribl Stream data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from the Cribl Stream data processing engine.

> `Smart` mode only engages when necessary, such as when a downstream Destination becomes blocked *and* the **Buffer size limit** reaches its limit. When persistent queue is set to `Smart` mode, Cribl attempts to flush the queue when every new event arrives. The only time events stay in the buffer is when a downstream Destination becomes blocked.
>
{.box .info}

**Buffer size limit**: The maximum number of events to hold in memory before reporting backpressure to the sender and writing the queue to disk. Defaults to `1000`. This buffer is for all connections, not just per Worker Process. For that reason, this can dramatically expand memory usage. Connections share this limit, which may result in slightly lower throughput for higher numbers of connections. For higher numbers of connections, consider increasing the limit.

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.

**File size limit**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, and so forth. If not specified, Cribl Stream applies the default `1 MB`.

**Queue size limit**: The maximum amount of disk space that the queue is allowed to consume on each Worker Process. Once this limit is reached, this Source will stop queueing data and block incoming data. Required, and defaults to `5` GB. Accepts positive numbers with units of `KB`, `MB`, `GB`, and so forth. Can be set as high as `1 TB`, unless you've [configured](group-settings#storage) a different **Worker Process PQ size limit** in Group or Fleet settings.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Stream will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file closes. Defaults to `None`; `Gzip` is also available.

> In Cribl Stream 4.1 and newer, the Source persistent queue default **Mode** is `Always on`, to best ensure events' delivery. For details on optimizing this selection, see [Optimize Source Persistent Queues (sPQ)](persistent-queues-sources).
>
> You can optimize Workers' startup connections and CPU load at **Group/Fleet settings** > [Worker Processes](group-settings#processes).
>
{.box .info}

### Processing Settings {#processing}

#### Fields

In this section, you can define new fields or modify existing ones using JavaScript expressions, similar to the [Eval](eval-function) function. 

* The **Field Name** can either be a new field (unique within the event) or an existing field name to modify its value.
* The **Value** is a JavaScript expression (enclosed in quotes or backticks) to compute the field's value (can be a constant). Select this field's advanced mode icon (far right) if you'd like to open a modal where you can work with sample data and iterate on results.

This flexibility means you can:

* Add new fields to enrich the event.
* Modify existing fields by overwriting their values.
* Compute logic or transformations using JavaScript expressions.

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

### Advanced Settings {#advanced-settings}

**Show originating IP**: Toggle on when clients are connecting through a proxy that supports the `X-Forwarded-For` header to keep the client's original IP address on the event instead of the proxy's IP address. This setting affects how the Source handles the [`__srcIpPort` field](#src-ip-port).

**Capture request headers**: Toggle on to add request headers to events, in the `__headers` field.

**Health check endpoint**: Toggle on to enable a health check endpoint specific to this Source, `http(s)://<host>:<port>/cribl_health`. A `200` HTTP response code is returned when the Source is healthy. Otherwise, two errors you could receive are:
  * `ECONNRESET` where the Source failed to initialize due to not having listeners on the port.
  * `503` or `Server is busy, max active connections reached` indicate there are too many connections per Worker Process.

**Active request limit**: Maximum number of active requests allowed for this Source, per Worker Process. Defaults to `256`. Enter `0` for unlimited.

> Raising this limit can increase throughput by allowing more concurrent data requests, but increases resource usage and load on both your Cribl Edge infrastructure and on downstream Destinations. Before raising this limit, ensure:
>
> - Your Cribl Edge deployment has sufficient capacity to support higher request concurrency, including CPU, memory, or number of Worker Processes.
> - Downstream Destinations are correctly sized and tuned to accept the higher data ingest rate, preventing backpressure. See [Manage Backpressure](manage-backpressure) for more information.
>
> Improper sizing on either side can result in dropped events, delayed processing, or overall system instability.
{.box .warning}

**Activity log sample rate**: Determines how often request activity is logged at the `info` level. The default `100` value logs every 100th value; a `1` value would log every request; a `10` value would log every 10th request; and so forth.

**Requests-per-socket limit**: The maximum number of requests Cribl Edge should allow on one socket before instructing the client to close the connection. Defaults to `0` (unlimited). See [Balancing Connection Reuse Against Request Distribution](#max-requests-per-socket) below.

**Socket timeout (seconds)**: How long Cribl Edge should wait before assuming that an inactive socket has timed out. The default `0` value means wait forever.

**Request timeout (seconds)**: How long to wait for an incoming request to complete before aborting it. The default `0` value means wait indefinitely.

**Keep-alive timeout (seconds)**: After the last response is sent, Cribl Edge will wait this long for additional data before closing the socket connection. Defaults to `5` seconds; minimum is `1` second; maximum is `600` seconds (10 minutes).

> The longer the **Keep‑alive timeout**, the more Cribl Edge will reuse connections. The shorter the timeout, the closer Cribl Edge gets to creating a new connection for every request. When request frequency is high, you can use longer timeouts to reduce the number of connections created, which mitigates the associated cost.
>
{.box .success}

**IP allowlist regex**: Grants access to requests originating from specific IP addresses that match a defined pattern. Unmatched requests are rejected with a 403 (Forbidden) status code. Defaults to `.*` (allow all).

**IP denylist regex**: Blocks requests originating from specific IP addresses that match a defined pattern, even if they would be allowed by default. Rejected requests receive a 403 (Forbidden) status code. Defaults to `^$` (allow all).

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

#### Balancing Connection Reuse Against Request Distribution {#max-requests-per-socket}

**Requests-per-socket limit** allows you to limit the number of HTTP requests an upstream client can send on one network connection. Once the limit is reached, Cribl Edge uses HTTP headers to inform the client that it must establish a new connection to send any more requests. (Specifically, Cribl Edge sets the HTTP `Connection` header to `close`.) After that, if the client disregards what Cribl Edge has asked it to do and tries to send another HTTP request over the existing connection, Cribl Edge will respond with an HTTP status code of `503 Service Unavailable`.

Use this setting to strike a balance between connection reuse by the client, and distribution of requests among one or more Edge Node processes by Cribl Edge:

* When a client sends a sequence of requests on the same connection, that is called connection reuse. Because connection reuse benefits client performance by avoiding the overhead of creating new connections, clients have an incentive to **maximize** connection reuse.

* Meanwhile, a single process on that Edge Node will handle all the requests of a single network connection, for the lifetime of the connection. When receiving a large overall set of data, Cribl Edge performs better when the workload is distributed across multiple Edge Node processes. In that situation, it makes sense to **limit** connection reuse.

There is no one-size-fits-all solution, because of variation in the size of the payload a client sends with a request and in the number of requests a client wants to send in one sequence. Start by estimating how long connections will stay open. To do this, multiply the typical time that requests take to process (based on payload size) times the number of requests the client typically wants to send.

If the result is 60 seconds or longer, set **Requests-per-socket limit** to force the client to create a new connection sooner. This way, more data can be spread over more Edge Node processes within a given unit of time.

For example: Suppose a client tries to send thousands of requests over a very few connections that stay open for hours on end. By setting a relatively low **Requests-per-socket limit**, you can ensure that the same work is done over more, shorter-lived connections distributed between more Edge Node processes, yielding better performance from Cribl Edge.

A final point to consider is that one Cribl Edge Source can receive requests from more than one client, making it more complicated to determine an optimal value for **Requests-per-socket limit**.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Edge uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

  * `__headers` – Added only when **Advanced Settings** > **Capture request headers** is toggled on.
  * `__inputId`
  * `__hecToken`
  * `__srcIpPort` – See details [below](#src-ip-port).
  * `__host` (Elastic In)
  * `__id` (Elastic In)
  * `__index` (Elastic In)
  * `__type` (Elastic In)

### Overriding `__srcIpPort` with Client IP/Port {#src-ip-port}

The `__srcIpPort` field's value contains the IP address and (optionally) port of the client sending data to this Source.

When any proxies (including load balancers) lie between the HTTP client and the Source, the last proxy adds an `X‑Forwarded‑For` header whose value is the IP/port of the original HTTP client. With multiple proxies, this header's value will be an array, whose first item is the original client IP/port.

If `X‑Forwarded‑For` is present, and **Advanced Settings** > **Show originating IP** is toggled off, the original client IP/port in this header will override the value of `__srcIpPort`.

If **Show originating IP** is toggled on, the `X‑Forwarded‑For` header's contents will **not** override the `__srcIpPort` value. (Here, the upstream proxy can convey the client IP/port without using this header.)

## Format and Endpoint

Cribl Edge expects HTTP(S) events to be formatted as one JSON record per event. Here are two event records:

```json {title="Sample Event Format"}
{"_time":1541280341, "_raw":"this is a sample event ", "host":"myHost", "source":"mySource", "fieldA":"valueA", "fieldB":"valueB"}
{"_time":1541280341, "host":"myOtherHost", "source":"myOtherSource", "_raw": "{\"message\":\"Something informative happened\", \"severity\":\"INFO\"}"}
```

**Note 1**: Events can be sent as separate POSTs, but Cribl **highly** recommends combining multiple events in newline-delimited groups, and POSTing them together.

**Note 2**: If an HTTP(S) source is routed to a Splunk destination, fields within the JSON payload are mapped to Splunk fields. Fields that do not have corresponding (native) Splunk fields become index-time fields. For example, let's assume we have a HTTP(S) event like this:

`{"_time":1541280341, "host":"myHost", "source":"mySource", "_raw":"this is a sample event ",
 "fieldA":"valueA"}`

Here, `_time`, `host` and `source` become their corresponding fields in Splunk. The value of `_raw` becomes the actual body of the event, and `fieldA` becomes an index-time field. (`fieldA`::`valueA`).

##  Examples  {#examples}

The examples in this section demonstrate sending HTTP data into a Bulk HTTP Source configured to listen on port `10080`. The token is set to `myToken42` and included in the `Authorization` header of requests.

### Cribl Endpoint – Single Event

```shell {title="Cribl Endpoint, Single Event Example"}
curl -k 'http://<myCriblHost>:10080/cribl/_bulk' \
-H 'Authorization: myToken42' \
-d '{"_raw":"this is a sample event ", "host":"myHost", "source":"mySource", "fieldA":"valueA", "fieldB":"valueB"}'
```

### Cribl Endpoint – Multiple Events

```shell {title="Cribl Endpoint, Multiple Events Example"}
curl -k 'http://<myCriblHost>:10080/cribl/_bulk' \
-H 'Authorization: myToken42' \
-d $'{"_raw":"this is a sample event ", "host":"myHost", "source":"mySource", "fieldA":"valueA", "fieldB":"valueB"}\n\
{"_raw":"this is another sample event ", "host":"myOtherHost", "source":"myOtherSource", "fieldA":"valueA", "fieldB":"valueB"}'
```

### Splunk HEC Event Endpoints

> For Splunk HEC, the token specification can be either  `Splunk <token>` or `<token>`.
>
{.box .info}

```shell {title="Splunk HEC Event Endpoint, /services/collector/event Example"}
curl -k 'http://<myCriblHost>:10080/services/collector/event' \
-H 'Authorization: myToken42' \
-d '{"event":"this is a sample event ", "host":"myHost", "source":"mySource", "fieldA":"valueA", "fieldB":"valueB"}'
```

```shell {title="Splunk HEC Event Endpoint, /services/collector Example"}
curl -k 'http://<myCriblHost>:10080/services/collector' \
-H 'Authorization: myToken42' \
-d '{"event":"this is a sample event ", "host":"myHost", "source":"mySource", "fieldA":"valueA", "fieldB":"valueB"}'
```

### Cribl.Cloud – Single Event

Replace `<organizationId>` with your Cribl.Cloud [**Organization ID**](workspaces#view-workspace-details).

```shell {title="Cribl.Cloud, Single Event Example"}
curl -k 'https://default.main.<organizationId>.cribl.cloud:10080/cribl/_bulk' \
-H 'Authorization: myToken42' \
-d '{"_raw":"this is a sample event ", "host":"myHost", "source":"mySource", "fieldA":"valueA", "fieldB":"valueB"}'
```

> For Cribl.Cloud Enterprise plans, replace `default` in the request URL with the name of the Fleet that you want to send to.
{.box .info}

## Periodic Logging {#logging}

Cribl Edge logs metrics about incoming requests and ingested events once per minute.

If one or more auth tokens are configured and enabled, Cribl Edge logs requests and events for each enabled auth token individually. Since the tokens themselves are redacted for security, Cribl Edge logs the initial text of the token description to help you identify which token a given log is for.

If no auth token is configured and enabled, Cribl Edge simply logs overall statistics about incoming requests and ingested events.

These logs are stored in the `metrics.log` file. To view them in the UI, open the Source's **Logs** tab and choose **Worker Process X Metrics** from the drop-down, where **X** is the desired Worker process.



## Troubleshooting

The Source's configuration modal has helpful tabs for troubleshooting:

**Live Data**: Try capturing [live data](sources#capture-source-data) to see real-time events as they are ingested. On the **Live Data** tab, click **Start Capture** to begin viewing real-time data.

**Logs**: Review and search the logs that provide detailed information about the ingestion process, including any errors or warnings that may have occurred.

You can also view the [Monitoring](monitoring) page that provides a comprehensive overview of data volume and rate, helping you identify ingestion issues. Analyze the graphs showing events and bytes in/out over time.

### Common Issue

### Dropping request because token invalid","authToken": "Bas...Njc="

The specified token is invalid. Note that the above message is logged only at the debug level.
