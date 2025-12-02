# Elasticsearch Destination


Cribl Stream can send events to an [Elasticsearch](http://elastic.co) cluster using the [Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html). As of v.3.3, Cribl Stream supports Elastic [data streams](https://www.elastic.co/guide/en/elasticsearch/reference/current/data-streams.html).

Use the [Elastic Cloud Destination](destinations-elastic-cloud) if you are exclusively targeting Elastic Cloud and prefer a simpler, optimized configuration process.

> Type: Streaming | TLS Support: Configurable | PQ Support: Yes 
>
> For an example of using Cribl Stream to parse and enrich inbound events for Elasticsearch, see our [Splunk to Elasticsearch](/stream/usecase-splunk-elasticsearch/) topic.
> 
{.box .info}

## Configure Cribl Stream to Output to Elasticsearch

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Elasticsearch Destination definition. If you clone this Destination, Cribl Stream will add `-CLONE` to the original **Output ID**. 
   - **Description**: Optionally, enter a description.
   - **Load balancing**: Toggle on (default) to specify multiple [bulk API URLs](#bulk-api-urls) and load weights. If toggled off and you notice that Cribl Stream is not sending data to all possible IP addresses, enable **Advanced Settings** > **Round-robin DNS**.
   - **Bulk API URL or Cloud ID**: Specify either an Elasticsearch cluster or Elastic Cloud deployment to send events to. For an Elasticsearch cluster, enter a URL (for example, `http://<myElasticCluster>:9200/_bulk`). For an Elastic Cloud deployment, enter its Cloud ID. This setting is not available when **Load balancing** is enabled.
   - **Index or data stream**: Enter a JavaScript expression that evaluates to the name of the Elastic data stream or Elastic index where you want events to go. The expression is evaluated for each event, can evaluate to a constant value, and must be enclosed in quotes or backticks. An event's `__index` field can overwrite the index or data stream name.
3. Under **Authentication**, toggle **Authentication enabled** on to display **Authentication method** dropdown. Select one of these options:
    - **Manual**: Enter your credentials directly in the resulting **Username** and **Password** fields.
    - **Secret**: Exposes a **Credentials secret** drop-down, in which you can select a [stored secret](/stream/securing-secrets) that references the credentials described above. A **Create** link is available to store a new, reusable secret.
    - **Manual API Key**: Exposes an **API key** field to directly enter your Elasticsearch API key.
    - **Secret API Key**: Exposes an **API key (text secret)** drop-down, in which you can select a [stored text secret](/stream/securing-secrets) that references your Elasticsearch API key. A **Create** link is available to store a new, reusable secret.
4. Next, you can configure the following **Optional Settings**: 
   - **Exclude current host IPs**: This toggle appears when **Load balancing** is toggled on. It determines whether to exclude all IPs of the current host from the list of any resolved hostnames. Default is toggled off, which keeps the current host available for load balancing.
   - **Type**: Specify document type to use for events. An event's `__type` field can overwrite this value.
   - **Backpressure behavior**: Specify whether to block, drop, or queue events when all receivers are exerting backpressure. Defaults to `Block`.
   - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
5. Optionally, you can adjust the [Persistent Queue](#pq), [Processing](#processing), [Retries](#retries), and [Advanced](#advanced-tab) settings outlined in the sections below.
6. Select **Save**, then **Commit & Deploy**. 
   
#### Bulk API URLs {#bulk-api-urls}

Use the **Bulk API URLs** table to specify a known set of receivers on which to load-balance data. To specify more receivers on new rows, click **Add URL**. Each row provides the following fields:

**URL**: Specify the URL to an Elastic node to send events to – for example, `http://elastic:9200/_bulk`

**Load weight**: Set the relative traffic-handling capability for each connection by assigning a weight (> `0`). This column accepts arbitrary values, but for best results, assign weights in the same order of magnitude to all connections. Cribl Stream will attempt to distribute traffic to the connections according to their relative weights.

The final column provides an `X` button to delete any row from the table.

For details on configuring all these options, see [About Load Balancing](load-balancing).

> When you first enable load balancing, or if you edit the load weight once your data is load–balanced, give the logic time to settle. The changes might take a few seconds to register.
>
{.box .info}


### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.14/snippets/_destinations-persistent-queue-settings.md]

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

### Retries {#retries}

[Snippet not found: content/shared/4.14/snippets/_destinations-http-retries.md]

### Advanced Settings {#advanced-tab}

**Validate server certs**: Reject certificates that are **not** authorized by a trusted CA (for example, the system's CA). Default is toggled on.

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Stream will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts. (This option is visible only when the **General Settings** > **Load balancing** option is toggled off.)

**Compress**: Compresses the payload body before sending. Default is toggled on (recommended).

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30`.

**Request concurrency**: Maximum number of concurrent requests per Worker Process. When Cribl Stream hits this limit, it begins throttling traffic to the downstream service. Defaults to `5`. Minimum: `1`. Maximum: `32`.

**Body size limit (KB)**: Maximum size of the request body before compression. Defaults to `4096` KB. The actual request body size might exceed the specified value because the Destination adds bytes when it writes to the downstream receiver. Cribl recommends that you experiment with the **Body size limit** value until downstream receivers reliably accept all events.

**Events-per-request limit**: Maximum number of events to include in the request body. The `0` default allows unlimited events.

**Flush period (s)**: Maximum time between requests. Low values could cause the payload size to be smaller than its configured maximum. Defaults to `1`.

**Extra HTTP headers**: Name-value pairs to pass as additional HTTP headers. Values will be sent encrypted.

**Extra parameters**: Name-value pairs to pass as additional parameters. If you are using Elastic [ingest pipelines](https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html), specify an extra parameter whose name is `pipeline` and whose value is the name of your pipeline, similar to [these](https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html#add-pipeline-to-indexing-request) examples.

**Elastic version**: Determines how to format events. For Elastic Cloud, you must explicitly set version  `7.x`. For other Elasticsearch clusters, the `Auto` default will discover the downstream Elasticsearch version automatically, but you have the option to explicitly set version `6.x` or `7.x`.

**Elastic pipeline**: To send data to an [Elastic Ingest pipeline](https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html), optionally enter that pipeline's name as a constant. Or, enter a JavaScript expression that evaluates outgoing events and sends matching events to the desired Elastic Ingest pipeline(s). Cribl Stream will first interpret your entry as a constant and try to find a matching value in the event. If it finds no matching value, it will evaluate your **Elastic pipeline** entry as an expression.

For example, the expression `sourcetype=='access_common'?'cribl_pipeline':undefined` matches events whose `sourcetype` is `access_common`, and sends them to an Elastic Ingest pipeline named `cribl_pipeline`.

You can also specify the name of the pipeline in an event field. For example, `myPipelineField` would use the value from the event's `myPipelineField` property (if present) to identify the Elastic Ingest pipeline to process the event. Alternately, the expression `myPipelineField != null ? myPipelineField : undefined` would also identify this field. If the event does not contain such a field, `myPipelineField != null ? myPipelineField : 'theDefaultIndex'` will provide a default index. With this approach, you can override the Elastic Ingest pipeline at the event level.

See also the [Elasticsearch Source documentation](sources-elastic#override-pipeline).

> The next two fields appear only when you toggle **Load balancing** on in the **General Settings**.
>
{.box .info}

**DNS resolution period (seconds)**: Re-resolve any hostnames each time this interval recurs, and pick up destinations from the A records. Defaults to `600` seconds.

**Load balance stats period (seconds)**: Lookback traffic history period. Defaults to `300` seconds.

**Include document_id**: Toggle off to omit the `document_id` field when sending events to an Elastic TSDS (time series data stream).

**Write action**: Action to use when writing events. Set this option to `Create` (default) when writing to a data stream, which is append-only. Set this option to `Index` only when you write directly to an index and need to update existing records. `Index` will fail if you use it to write to a data stream.

**Failed request logging mode**: Determines which data is logged when a request fails. Use the drop-down to select one of these options:

- `None` (default).
- `Payload`.
- `Payload + Headers`. Use the **Safe Headers** field below to specify the headers to log. If you leave that field empty, all headers are redacted, even with this setting. 

**Safe headers**: List the headers you want to log, in plain text.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Field Normalization

When sending data to Elasticsearch, this Destination applies the following field normalization to align with the Elasticsearch schema:

* Renames `_time` to `@timestamp`, maintaining millisecond precision.
* Renames `host` to `host.name`.

See also our [Elasticsearch Source](sources-elastic) documentation's **Field Normalization** section.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in forwarding data to a Destination. 

Fields for this Destination:

  * `__id`
  * `__type`
  * `__index`
  * `__host`

## Notes on HTTP-Based Outputs

- To proxy outbound HTTP/S requests, see [System Proxy Configuration](proxy-config).

- Cribl Stream will attempt to use keepalives to reuse a connection for multiple requests. After two minutes of the first use, the connection will be thrown away, and a new one will be reattempted. This is to prevent sticking to a particular destination when there is a constant flow of events. 

- If the server does not support keepalives (or if the server closes a pooled connection while idle),  a new connection will be established for the next request.

- When resolving the Destination's hostname with load balancing disabled, Cribl Stream will pick the first IP in the list for use in the next connection. Enable Round-robin DNS to better balance distribution of events between Elasticsearch cluster nodes.

## Troubleshooting

[Snippet not found: content/shared/4.14/snippets/_destinations-troubleshooting.md]