# Dynatrace HTTP Destination


The Dynatrace HTTP Destination sends logs to [Dynatrace](https://docs.dynatrace.com/docs)'s SaaS or ActiveGate endpoints.

> Type: Streaming | TLS Support: Configurable | PQ Support: Yes
>
{.box .info}

## Configure a Dynatrace HTTP Destination

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
    * To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing**.
    * To configure via the [Routes](routes), select **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.

1. In the Destination modal, configure the following under **General Settings**:
    * **Output ID**: Enter a unique name to identify this Destination definition. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**. A **Description** is optional.
    * **Endpoint**: Select the type of Dynatrace endpoint where you want to send your data.
      * **Cloud**: Sends data to Dynatrace's SaaS endpoint. Provide your Dynatrace [**Environment ID**](https://docs.dynatrace.com/docs/get-started/monitoring-environment). This ID uniquely identifies your Dynatrace environment and is required for routing data correctly. You can find your environment ID in your Dynatrace account settings.
      * **ActiveGate**: Choose this option if you are routing data through an ActiveGate. Provide your Dynatrace [**Environment ID**](https://docs.dynatrace.com/docs/get-started/monitoring-environment) and **ActiveGate domain** in the additional fields that appear. Your ActiveGate domain is displayed in your Dynatrace settings in the **Details** section of an ActiveGate under the **Hostname** field. The format should be a fully qualified domain name (FQDN) or IP address. For example, `https://{activeGate-domain}:9999/e/{environment-id}/api/v2/logs/ingest`.
      * **Manual**: Select this option if you want to manually specify the endpoint URL. This is useful for custom setups or non-standard configurations. You will need to enter the full **URL** in the field that appears. Can be overwritten by an event's `__url` field.

1. Next, you can configure the following **Optional Settings**:
    * **Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`.
    * **Tags**: Optionally, add tags that you can use to filter and group Destinations in Cribl Edge's **Manage Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

1. Select **Authentication** from the sidebar and select an **Authentication type**.
   * `Auth token`: Provide a Dynatrace API [access token](https://docs.dynatrace.com/docs/dynatrace-api/basics/dynatrace-api-authentication) to authenticate in the **Token** field. This is the most common method for securing data transmission to Dynatrace. You can generate this token in your Dynatrace account settings under the API tokens section. Ensure that the token has the necessary permissions for the data you are sending.
   * `Token (text secret)`: Create or select a Cribl [stored text secret](/stream/securing-secrets) that contains your Dynatrace API [access token](https://docs.dynatrace.com/docs/dynatrace-api/basics/dynatrace-api-authentication).

1. Optionally, configure any [Persistent Queue](#pq), [Processing](#processing), [Retries](#retries), and [Advanced](#advanced) settings outlined in the below sections.
   * **Persistent Queue** ensures data durability and reliability by buffering and preserving incoming events when the receiver is down or exhibiting backpressure.
   * **Processing Settings** allow you to define pre-processing and post-processing Pipelines. This includes adding metadata, applying transformations, and conditioning the data before it's sent.
   * **Retries** allow you to specify how the system should handle different HTTP response status codes and timeouts, ensuring reliable data delivery.
   * **Advanced Settings** offer additional configuration options that enhance the flexibility and performance of data transmission.

    > To track data sent via Cribl, you can add a custom attribute in the **Extra HTTP headers** section under [Advanced Settings](#advanced). For example, select `Add Header` and enter `cribl` as the name and `yes` as the value.
    {.box .success}

1. Select **Save**, then **Commit & Deploy**.

1. Use the **Test** tab in the Destination’s configuration modal to validate the configuration and connectivity of your Destination.

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.13/snippets/_destinations-persistent-queue-settings.md]

### Processing Settings  {#processing}

#### Post‑Processing  {#post-processing}

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Edge Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

### Retries

[Snippet not found: content/shared/4.13/snippets/_destinations-http-retries.md]

###  Advanced Settings  {#advanced}

**Validate server certs**: Reject certificates that are **not** authorized by a CA in the **CA certificate path**, nor by another trusted CA (for example, the system's CA). Defaults to toggled on.

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Edge will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.

**Compress**: Toggle on (default) to compress the payload body before sending.

**Keep alive**: By default, Cribl Edge sends `Keep-Alive` headers to the remote server and preserves the connection from the client side up to a maximum of 120 seconds. Toggle this off if you want Cribl Edge to close the connection immediately after sending a request.

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30`.

**Request concurrency**: Maximum number of concurrent requests per Worker Process. When Cribl Edge hits this limit, it begins throttling traffic to the downstream service. Defaults to `5`. Minimum: `1`. Maximum: `32`.

**Body size limit (KB)**: Maximum size of the request body before compression. Defaults to `4096` KB. You can set this limit to as high as 500 MB (`512000` KB). Be aware that high values can cause high memory usage per Worker Node, especially if a dynamically constructed URL (see [Internal Fields](#internal-fields)) causes this Destination to send events to more than one URL. The actual request body size might exceed the specified value because the Destination adds bytes when it writes to the downstream receiver. Cribl recommends that you experiment with the **Body size limit** value until downstream receivers reliably accept all events.

**Buffer memory limit (KB)**: Total amount of memory used to buffer outgoing requests waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced. This provides granular control over the memory allocated for buffering outgoing batched requests. Increasing the limit allows batches to grow larger before being flushed, improving efficiency for data with high cardinality (a large number of unique batches). Finding the optimal balance between efficient data transfer and memory usage involves adjusting both the **Buffer memory limit** and **Body size limit** settings.

**Events-per-request limit**: Maximum number of events to include in the request body. The `0` default allows unlimited events.

**Flush period (sec)**: Maximum time between requests. Low values could cause the payload size to be smaller than its configured maximum. Defaults to `1`.

**Extra HTTP headers**: Name-value pairs to pass to all events as additional HTTP headers. Values will be sent encrypted. You can also add headers dynamically on a per-event basis in the `__headers` field. See [Internal Fields](#internal-fields) below.

**Failed request logging mode**: Use this drop-down to determine which data should be logged when a request fails. Select among `None` (the default), `Payload`, or `Payload + Headers`. With this last option, Cribl Edge will redact all headers, except non-sensitive headers that you declare below in **Safe headers**.

**Safe headers**: Add headers to declare them as safe to log in plaintext. (Sensitive headers such as `authorization` will always be redacted, even if listed here.) Use a tab or hard return to separate header names.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Internal Fields {#internal-fields}

Cribl Edge uses a set of internal fields to assist in forwarding data to a Destination.

Fields for this Destination:

* `__criblMetrics`
* `__url`
* `__headers`



If an event contains the internal field `__url`, that field's value will override the **General Settings** > **Endpoint** value. This way, you can determine the endpoint URL dynamically.

For example, you could create a Pipeline containing an [Eval Function](eval-function) that adds the `__url` field, and connect that Pipeline to your Dynatrace HTTP Destination. Configure the Eval Function to set `__url`'s value to a URL that varies depending on a [global variable](global-variables-library), or on some property of the event, or on some other dynamically-generated value that meets your needs.

If an event contains the internal field `__headers`, that field's value will be a JSON object containing Name-value pairs, each of which defines a header. By creating Pipelines that set the value of `__headers` according to conditions that you specify, you can add headers dynamically.

For example, you could create a Pipeline containing [Eval Function](eval-function)s that add the `__headers` field, and connect that Pipeline to your Dynatrace HTTP Destination. Configure the Eval Functions to set `__headers` values to Name-value pairs that vary depending on some properties of the event, or on dynamically-generated values that meet your needs.

Here's an overview of how to add headers:

- To add "dynamic" (a.k.a. "custom") headers to some events but not others, use the `__headers` field.
- To define headers to add to **all** events, use **Advanced Settings** > **Extra HTTP Headers**.
- An event can include headers added by both methods. So if you use `__headers` to add `{ "api‑key": "foo" }` and **Extra HTTP Headers** to add `{ "goat": "Kid A" }`, you'll get both headers.
- Headers added via the `__headers` field take precedence. So if you use `__headers` to add `{ "api‑key": "foo" }` and **Extra HTTP Headers** to add `{ "api‑key": "bar" }`, you'll get only one header, and that will be `{ "api‑key": "foo" }`.

## Notes on HTTP-Based Outputs

- To proxy outbound HTTP/S requests, see [System Proxy Configuration](proxy-config).

- Cribl Edge will attempt to use keepalives to reuse a connection for multiple requests. After two minutes of the first use, it will throw away the connection and attempt a new one. This is to prevent sticking to a particular destination when there is a constant flow of events.

- If the server does not support keepalives (or if the server closes a pooled connection while idle), Cribl Edge will establish a new connection for the next request.

- When resolving the Destination's hostname, Cribl Edge will pick the first IP in the list for use in the next connection. Enable **Round‑robin DNS** to better balance the distribution of events among destination cluster nodes.

## Troubleshoot

[Snippet not found: content/shared/4.13/snippets/_destinations-troubleshooting.md]