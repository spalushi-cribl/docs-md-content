# HTTP/S (Bulk API)


Cribl Edge supports receiving data over HTTP/S from Cribl Bulk API, [Splunk HEC](https://dev.splunk.com/enterprise/docs/dataapps/httpeventcollector/), and [Elastic Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html) endpoints.

> Type: **Push**  |  TLS Support: **YES** | Event Breaker Support: **No**
> 
> This Source supports gzip-compressed inbound data when the `Content‑Encoding: gzip` connection header is set.
>
{.box .info}

## Configuring Cribl Edge to Receive Data over HTTP(S) 

From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**Push** >] **HTTP**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Push** >] **HTTP**. Next, click **New Source** to open a **New Source** modal that provides the options below.

> Cribl Edge ships with an HTTP Source preconfigured to listen on Port 10080, and on several default endpoints. You can clone or directly modify this Source to further configure it, and then enable it.
>
{.box .success}

### General Settings

**Input ID**: Enter a unique name to identify this HTTP(S) Source definition. 

**Address**: Enter the hostname/IP on which to listen for HTTP(S) data. (E.g., `localhost` or `0.0.0.0`.)

**Port**: Enter the port number.

### Authentication Settings

**Auth tokens**: Shared secrets to be provided by any client (`Authorization: <token>`). Click **Generate** to create a new secret. If empty, unauthenticated access **will be permitted**.

### Optional Settings

**Cribl HTTP event API**: Base path on which to listen for Cribl HTTP API requests. To construct the actual endpoint, Cribl Edge will append `/_bulk` to this path. For example, with the default value of `/cribl`, your senders should send events to a `/cribl/_bulk` path.  Use an empty string to disable.

**Elastic API endpoint** (for [Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html)): Base path on which to listen for Elasticsearch API requests. Currently, the only supported option is the default `/elastic`, to which Cribl Edge will append `/_bulk`. So, your senders should send events to an `/elastic/_bulk` path. Other entries are faked as success. Use an empty string to disable.

> Cribl generally recommends that you use the dedicated [Elasticsearch API](sources-elastic) Source instead of this endpoint. The Elastic API implementation here is provided for backward compatibility, and for users who want to ingest multiple inputs on one HTTP/S port.
>
{.box .info}

**Splunk HEC endpoint**: Absolute path on which to listen for Splunk HTTP Event Collector (HEC) API requests. Use an empty string to disable. Default entry is `/services/collector`. 

> This Splunk HEC implementation is an **event** (i.e., not **raw**) endpoint. For details, see [Splunk's documentation](https://docs.splunk.com/Documentation/Splunk/latest/Data/UsetheHTTPEventCollector). To send data to it from a HEC client, use either `/services/collector` or `/services/collector/event`. (See the [examples](#examples) below.)
> 
> Cribl generally recommends that you use the dedicated [Splunk HEC](sources-splunk-hec) Source instead of this endpoint. The Splunk HEC implementation here is provided for backward compatibility, and for users who want to ingest multiple inputs on one HTTP/S port.
>
{.box .info}

**Splunk HEC Acks**: Whether to enable Splunk HEC acknowledgements. Defaults to `No`. 

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### TLS Settings (Server Side)

**Enabled** defaults to `No`. When toggled to `Yes`:

**Certificate name**: Name of the predefined certificate.

**Private key path**: Server path containing the private key (in PEM format) to use. Path can reference `$ENV_VARS`.

**Passphrase**: Passphrase to use to decrypt private key.  

**Certificate path**: Server path containing certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**CA certificate path**: Server path containing CA certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**Authenticate client (mutual auth)**: Require clients to present their certificates. Used to perform mutual authentication using SSL certs. Defaults to `No`. When toggled to `Yes`:

- **Validate client certs**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (e.g., the system's CA). Defaults to `Yes`.

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
- `Always On`: This default option will always write events to the persistent queue, before forwarding them to Cribl Edge's data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from Cribl Edge's data processing engine.

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the Source. Defaults to `1000`.

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, etc. If not specified, Cribl Edge applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume, on each Worker Process. Once this limit is reached, Cribl Edge will stop queueing data, and will apply the **Queue‑full behavior**. Enter a numeral with units of KB, MB, etc. If not specified, the implicit `0` default will enable Cribl Edge to fill all available disk space on the volume.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Edge will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file is closed. Defaults to `None`; `Gzip` is also available.

> As of Cribl Edge 4.1, Source-side PQ's default **Mode** changed from `Smart` to `Always on`. This option more reliably ensures events' delivery, and the change does not affect existing Sources' configurations. However:
> - If you create Stream Sources programmatically, and you want to enforce the previous `Smart` mode, you'll need to update your existing code.
> - If you enable `Always on`, this can reduce data throughput. As a trade-off for data durability, you might need to either accept slower throughput, or provision more machines/faster disks.
> - You can optimize Workers' startup connections and CPU load at **Fleet Settings** > [Worker Processes](settings-group-fleet#processes).
>
{.box .warning}

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

**Activity log sample rate**: Determines how often request activity is logged at the `info` level. The default `100` value logs every 100th value; a `1` value would log every request; a `10` value would log every 10th request; etc.

**Socket timeout (seconds)**: How long Cribl Edge should wait before assuming that an inactive socket has timed out. The default `0` value means wait forever.

**Request timeout (seconds)**: How long to wait for an incoming request to complete before aborting it. The default `0` value means wait indefinitely.

**Keep-alive timeout (seconds)**: After the last response is sent, Cribl Edge will wait this long for additional data before closing the socket connection. Defaults to `5` seconds; minimum is `1` second; maximum is `600` seconds (10 minutes).

> The longer the **Keep‑alive timeout**, the more Cribl Edge will reuse connections. The shorter the timeout, the closer Cribl Edge gets to creating a new connection for every request. When request frequency is high, you can use longer timeouts to reduce the number of connections created, which mitigates the associated cost.
>
{.box .success}

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Edge uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

  * `__headers` – Added only when **Advanced Settings** > **Capture request headers** is set to `Yes`.
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

If `X‑Forwarded‑For` is present, and **Advanced Settings** > **Enable proxy protocol** is set to `No`, the original client IP/port in this header will override the value of `__srcIpPort`. 

If **Enable proxy protocol** is set to `Yes`, the `X‑Forwarded‑For` header's contents will **not** override the `__srcIpPort` value. (Here, the upstream proxy can convey the client IP/port without using this header.)

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

### Cribl Edge

The examples in this section demonstrate sending HTTP data into a Cribl Edge binary that you manage on-prem, or on a VM. To set up these examples:

1. Configure Cribl to listen on port `10080` for HTTP (default). Set `authToken` to `myToken42`.
2. Send a payload to your Cribl Edge receiver.

#### Cribl Endpoint – Single Event

```shell {title="Cribl Single Event Example:"}
curl -k http://<myCriblHost>:10080/cribl/_bulk -H 'Authorization: myToken42' -d '{"_raw":"this is a sample event ", "host":"myHost", "source":"mySource", "fieldA":"valueA", "fieldB":"valueB"}'
```

#### Cribl Endpoint – Multiple Events

``` {title="Cribl Endpoint - Multiple Events"}
curl -k http://<myCriblHost>:10080/cribl/_bulk -H 'Authorization: myToken42' -d $'{"_raw":"this is a sample event ", "host":"myHost", "source":"mySource", "fieldA":"valueA", "fieldB":"valueB"} \n {"_raw":"this is another sample event ", "host":"myOtherHost", "source":"myOtherSource", "fieldA":"valueA", "fieldB":"valueB"}'
```

#### Splunk HEC Event Endpoint

``` {title="Splunk HEC Event Endpoint"}
curl -k http://<myCriblHost>:10080/services/collector/event -H 'Authorization: myToken42' -d '{"event":"this is a sample event ", "host":"myHost", "source":"mySource", "fieldA":"valueA", "fieldB":"valueB"}'

curl -k http://<myCriblHost>:10080/services/collector -H 'Authorization: myToken42' -d '{"event":"this is a sample event ", "host":"myHost", "source":"mySource", "fieldA":"valueA", "fieldB":"valueB"}'
```



> For Splunk HEC, the token specification can be either  `Splunk <token>` or `<token>`.
>
{.box .info}

### Cribl Cloud – Single Event

1. Generate and copy a token in your Cribl Cloud instance's HTTP Source > **General Settings**.

1. From the command line, use `https`, your Cribl.Cloud portal’s **Ingest Address** and port, and the token's value. You can find the **Ingest Address** on the top nav’s **Network Settings** page, under the **Data Sources** tab.

``` {title="Cribl Cloud – Single Event"}
curl -k https://default.main.<Your-Org-ID>.cribl.cloud:10080/cribl/_bulk -H 'Authorization: <token_value>' -d '{"_raw":"this is a sample event ", "host":"myHost", "source":"mySource", "fieldA":"valueA", "fieldB":"valueB"}'
```

> With a Cribl.Cloud Enterprise plan, generalize  the above URL's `default.main` substring to `<group-name>.main` when sending to other Fleets.
>
{.box .info}