# InfluxDB Destination


Cribl Edge supports sending data to [InfluxDB](https://www.influxdata.com/products/influxdb/) (versions 1.x and 2.0.x) and [InfluxDB Cloud](https://www.influxdata.com/products/influxdb-cloud/). 

> Type: Streaming | TLS Support: Configurable | PQ Support: Yes 
>
{.box .info} 

## Configure Cribl Edge to Output to InfluxDB

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this InfluxDB definition. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**.
   - **Description**: Optionally, enter a description.
   - **Write API URL**: The URL of an InfluxDB cluster to send events to:
      - v1 API: `http://localhost:8086/write`
      - v2 API: `http://localhost:8086/api/v2/write`
   - **Use v2 API**: You can enable the [InfluxDB v2 API](https://docs.influxdata.com/influxdb/v2.1/reference/api/) with InfluxDB version 1.8 or later. Toggle off (default) to fall back to the v1 API and display a **Database** field. If you toggle **Use v2 API** on, Cribl Edge communicates using InfluxDB's v2 API, and instead displays these two fields:
      - **Bucket**: Enter the bucket to write to (required).
      - **Organization**: The Organization ID corresponding to the specified **Bucket** (required in this configuration, although InfluxDB v.1.8 will ignore it).
   - **Database**: Name of the database on which to write data points. Displayed only for configurations that use the v1 API (required).
3. Next, you can configure the following **Optional Settings**: 
    - **Timestamp precision**: Sets the precision for the supplied UNIX time values. Defaults to `Milliseconds`.
    - **Dynamic value fields**: When toggled on (default), Cribl Edge will pull the value field from the metric name. For example, `db.query.user` will use `db.query` as the measurement and `user` as the value field.
    - **Value field name**: Name of the field in which to store the metric when sending to InfluxDB. This will be used as a fallback if dynamic name generation is enabled but fails. Defaults to `value`.
    - **Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`.
    - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [Authentication](#authentication-settings), [Persistent Queue](#pq), [Processing](#processing), [Retries](#retries), and [Advanced](#advanced-settings) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 

### Authentication  {#authentication-settings}

Use the **Authentication type** drop-down to select one of these options:

**None**: This default setting does not use authentication.

**Auth token**: Use HTTP token authentication. In the resulting **Token** field, enter the bearer token that must be included in the HTTP authorization header.

**Auth token (text secret)**: This option exposes a **Token (text secret)** drop-down, in which you can select a [stored text secret](/stream/securing-secrets) that references the bearer token described above. A **Create** link is available to store a new, reusable secret.

**Basic**: Displays **Username** and **Password** fields for you to enter HTTP Basic authentication credentials.

**Basic (credentials secret)**: This option exposes a **Credentials secret** drop-down, in which you can select a [stored text secret](/stream/securing-secrets) that references the Basic authentication credentials described above. A **Create** link is available to store a new, reusable secret.

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.12/snippets/_destinations-persistent-queue-settings.md]

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Edge Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

### Retries {#retries}

[Snippet not found: content/shared/4.12/snippets/_destinations-http-retries.md]

### Advanced Settings {#advanced-tab}

**Validate server certs**: Reject certificates that are **not** authorized by a trusted CA (for example, the system's CA). Defaults to toggled on.

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Edge will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.

**Compress**: Toggle on (default) to compress the payload body before sending.

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30`.

**Request concurrency**: Maximum number of concurrent requests before blocking. This is set per Worker Process. Defaults to `5`.

**Body size limit (KB)**: Maximum size of the request body before compression. Defaults to `4096` KB. The actual request body size might exceed the specified value because the Destination adds bytes when it writes to the downstream receiver. Cribl recommends that you experiment with the **Body size limit** value until downstream receivers reliably accept all events.

**Events-per-request limit**: Maximum number of events to include in the request body. The `0` default allows unlimited events.

**Flush period (sec)**: Maximum time between requests. Low values could cause the payload size to be smaller than its configured maximum. Defaults to `1`.

**Extra HTTP headers**: Name-value pairs to pass as additional HTTP headers. Values will be sent encrypted. 

**Failed request logging mode**: Use this drop-down to determine which data should be logged when a request fails. Select among `None` (the default), `Payload`, or `Payload + Headers`. With this last option, Cribl Edge will redact all headers, except non-sensitive headers that you declare below in **Safe headers**. 

**Safe headers**: Add headers to declare them as safe to log in plaintext. (Sensitive headers such as `authorization` will always be redacted, even if listed here.) Use a tab or hard return to separate header names.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Troubleshooting

[Snippet not found: content/shared/4.12/snippets/_destinations-troubleshooting.md]