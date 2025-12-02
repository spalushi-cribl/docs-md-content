# Elastic Cloud Destination


Cribl Stream can send events to [Elastic Cloud](https://www.elastic.co/cloud).

Use the [Elasticsearch Destination](destinations-elastic) if you need flexibility to support both self-hosted and Elastic Cloud deployments and require advanced configuration options.

> Type: Streaming | TLS Support: Yes | PQ Support: Yes
>
> For an example of using Cribl Stream to parse and enrich inbound events for Elasticsearch, see our [Splunk to Elasticsearch](/stream/usecase-splunk-elasticsearch/) topic.
> 
{.box .info}

## Configure Cribl Stream to Output to Elastic Cloud

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Elastic Cloud Destination definition. 
   - **Description**: Optionally, enter a description.
   -  **Cloud ID**: Enter the Cloud ID of the Elastic Cloud environment where you want to send events. You'll find it on the Deployments overview page of your Elastic Cloud environment.
   -  **Data stream or index**: Enter a JavaScript expression that evaluates to the name of the Elastic data stream or Elastic index where you want events to go. The expression is evaluated for each event, can evaluate to a constant value, and must be enclosed in quotes or backticks. An event's `__index` field can overwrite the index or data stream name.
   - **Elastic pipeline**: Enter the name of the [Elastic ingest pipeline](https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html) (optional). You can use an expression to override the pipeline attribute received from the [Elasticsearch Source](sources-elastic#override-pipeline).
3. Under **Authentication**, select an **Authentication method** from the dropdown:
   - **Manual**: Enter your credentials directly in the resulting **Username** and **Password** fields. 
   - **Secret**: Exposes a **Credentials secret** drop-down, in which you can select a [stored secret](/stream/securing-secrets) that references the credentials described above. A **Create** link is available to store a new, reusable secret.
   - **Manual API Key**: Exposes an **API key** field to directly enter your Elasticsearch API key.
   - **Secret API Key**: Exposes an **API key (text secret)** drop-down, in which you can select a [stored text secret](/stream/securing-secrets) that references your Elasticsearch API key. A **Create** link is available to store a new, reusable secret.
4. Next, you can configure the following **Optional Settings**: 
    - **Backpressure behavior**: Specify whether to block, drop, or queue events when all receivers are exerting backpressure. Defaults to `Block`.
    - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
5. Optionally, you can adjust the [Persistent Queue](#pq), [Processing](#processing), [Retries](#retries) and [Advanced](#advanced-tab) settings outlined in the sections below.
6. Select **Save**, then **Commit & Deploy**. 

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.10/snippets/_destinations-persistent-queue-settings.md]

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

[Snippet not found: content/shared/4.10/snippets/_destinations-http-retries.md]

### Advanced Settings {#advanced-tab}

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30`.

**Request concurrency**: Maximum number of concurrent requests per Worker Process. When Cribl Stream hits this limit, it begins throttling traffic to the downstream service. Defaults to `5`. Minimum: `1`. Maximum: `32`.

**Max body size (KB)**: Maximum size of the request body before compression. Defaults to `4096` KB. The actual request body size might exceed the specified value because the Destination adds bytes when it writes to the downstream receiver. Cribl recommends that you experiment with the **Max body size** value until downstream receivers reliably accept all events.

**Max events per request**: Maximum number of events to include in the request body. The `0` default allows unlimited events.

**Flush period (s)**: Maximum time between requests. Low values could cause the payload size to be smaller than its configured maximum. Defaults to `1`.

**Extra HTTP headers**: Name-value pairs to pass as additional HTTP headers. Values will be sent encrypted.

**Extra parameters**: Name-value pairs to pass as additional parameters. If you are using Elastic [ingest pipelines](https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html), specify an extra parameter whose name is `pipeline` and whose value is the name of your pipeline, similar to [these](https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html#add-pipeline-to-indexing-request) examples.

**Include document_id**: Toggle off to omit the `document_id` field when sending events to an Elastic TSDS (time series data stream).

**Failed request logging mode**: Determines which data is logged when a request fails. Use the drop-down to select one of these options:

- `None` (default).
- `Payload`.
- `Payload + Headers`. Use the **Safe Headers** field below to specify the headers to log. If you leave that field empty, all headers are redacted, even with this setting. 

**Safe headers**: List the headers you want to log, in plain text.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Field Normalization

This Destination normalizes the following fields:   

* `_time` becomes `@timestamp` at millisecond resolultion. 
* `host.name` is set to `host`.

See also our [Elasticsearch Source](sources-elastic#field-normalization) documentation's **Field Normalization** section.

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

## Troubleshooting

[Snippet not found: content/shared/4.10/snippets/_destinations-troubleshooting.md]