# Splunk Search Collector


Cribl Stream supports collecting search results from Splunk queries. The queries can be both simple and complex, as well as real-time searches. This page covers how to configure the Collector.

## Splunk Cloud Prerequisite: IP Allowlist

To enable Splunk Cloud to accept connections from your Cribl Workers, you must allowlist their egress IP addresses as described in the [Splunk IP allow-list documentation](https://help.splunk.com/en/splunk-cloud-platform/administer/admin-config-service-manual/10.0.2503/administer-splunk-cloud-platform-using-the-admin-config-service-acs-api/configure-ip-allow-lists-for-splunk-cloud-platform). For Cribl.Cloud, [egress IPs](workspaces#stream-worker-group-details) are group-specific and dynamic. Cribl might update them during infrastructure changes, so you must monitor and update your allowlist accordingly. For on-prem and hybrid deployments, you control your network and can assign static egress IPs, allowing you to configure the allowlist once unless your network changes.

## Configure a Splunk Search Collector 

1. Navigate to **Products** > **Stream** > **Worker Groups**. Select a Worker Group, then go to **Data** > **Sources**. Choose the Source and select **Add Collector**.  
1. In the **New Collector** modal, configure the following under **Collector Settings**:
   - **Collector ID**: Unique ID for this Collector. For example, `splunk2search`. If you clone this Collector, Cribl Stream will add `-CLONE` to the original **Collector ID**.
   - **Description**: Optionally, enter a description.
   - **Search endpoint**: Rest API used to conduct a search. Defaults to `/services/search/v2/jobs/export`.
   - **Output mode**: Format of the returned output: `JSON` or `CSV`. Defaults to `JSON` format.
1.  Under **Search**, configure the following.
    - **Search**: Enter the Splunk query. For example: `index=myAppLogs level=error channel=myApp` OR `| mstats avg(myStat) as myStat WHERE index=myStatsIndex`. The optional **Preserve escaped quotes** setting maintains escaped quotes.
    - **Search head**: Enter the search head base URL. The default is `https://localhost:8089`.
    - **Earliest**: You can enter the earliest time boundary for the search. This can be an exact or relative time. For example: `2022-01-14T12:00:00Z` or `-16m@m`. 
    - **Latest**: You can enter the latest time boundary for the search. This can be an exact or relative time. For example: `2022-01-14T12:00:00Z` or `-16m@m`.
1.  Under **Authentication**, select an **Authentication method** from the dropdown:
    - **None**: Don't use authentication. Compatible with REST servers like AWS, where you embed a secret directly in the request URL.
    - **Basic**: Displays **Username** and **Password** fields for you to enter HTTP Basic authentication credentials.
    - **Basic (credentials secret)**: Provide username and password credentials referenced by a secret. Select a stored text secret in the resulting **Credentials secret** drop-down, or select **Create** to configure a new secret.
    - **Bearer token**: Provide the `token` value configured and generated in Splunk.
    - **Bearer token (text secret)**: Provide the Bearer token referenced by a secret. Select a stored text secret in the resulting **Token (text secret)** drop-down, or select **Create** to configure a new secret.

      > {{<id extra-headers-auth >}} You can also use [**Extra Headers**](#extra-header-value) as an alternative way of authenticating a request.
      > Select `None` in the **Authentication** drop-down
      > and use the [`C.Secret`](expressions-other#csecret) expression method to provide the value for the `Authentication` header.
      > For example:
      > 
      > `` `Bearer: ${C.Secret('<secretId>', 'text').value}` ``
      {.box .success}

1. Under **Retries** configure the following.
   - **Retry type**: The algorithm to use when performing HTTP retries. Options include `Backoff` (the default), `Static`, and `Disabled`.
   - **Initial retry interval (ms)**: Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute). A value of `0` means retry immediately until reaching the limit specified in **Retry limit**. 
   - **Retry limit**: Maximum number of times to retry a failed HTTP request. Defaults to `5`. Maximum: `20`. A value of `0` means don't retry at all.
   - **Backoff multiplier**: Base for exponential backoff. A value of `2` (default) means that Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
   - **Retry HTTP codes**: List of HTTP codes that trigger a retry. Leave empty to use the defaults (`429` and `503`). Cribl Stream does not retry codes in the `200` series.
   - **Honor Retry-After header**: If toggled on (default) and the `retry-after` [header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) is present, Cribl Stream honors any `retry-after` header that specifies a delay, up to a maximum of 20 seconds. For details, see [Honor Retry-After Header](#retry).
   - **Retry connection timeout**: Toggle on to automatically retry a single connection attempt after a timeout (`ETIMEDOUT`) to ensure data continuity.
   - **Retry connection reset**: Toggle on to automatically retry a connection after a peer reset (`ECONNRESET`) to maintain data flow.
1.  Next, you can configure the following **Optional Settings**: 
    - **Extra parameters**: Optional HTTP request parameters to append to the request URL. For details, see [Extra Parameters](#param)  below. 
    - {{<id extra-header-value >}}**Extra headers**: Select **Add Header** to (optionally) add collection request headers as key-value pairs. For details, see [Extra Headers](#headers) below.
    - In the **Request Timeout (secs)** field, you can set a maximum time period (in seconds) for an HTTP request to complete before Cribl Stream treats it as timed out. Defaults to `0`, which disables timeout metering. When running a real-time search you must update the **Request Timeout Parameter** to avoid having the Collector stuck in a forever running state. Updating the **Request Timeout Parameter** stops the search after the allocated period of time. 
    - **Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Stream will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.
    - **Disable time filter**: Toggle on to bypass the [collection job](collectors-schedule-run) event timestamp filtering. By default, Collectors apply time filtering based on the configured time range. Events with timestamps outside this range are discarded. Enabling this setting disables timestamp-based filtering, allowing all returned events to be processed regardless of their timestamps. This is useful when the data source's timestamps do not align with the configured time range, causing unexpected filtering, or when you need to process all data regardless of their timestamps. If you encounter zero results despite the data source returning events, disabling the time filter can help isolate whether timestamp filtering is the issue. For details, see [No Discover or Collector Event Results](collectors-schedule-run#disable-time-filter).
    - **Reject unauthorized certificates**: Whether to accept certificates that cannot be verified against a valid Certificate Authority (for example, self-signed certificates). Defaults to toggled on.
    - **Preserve escaped characters**: Toggle on to preserve backslash-escaped characters in **Search** queries. This ensures complex search strings such as `| search "\"foo\" \"bar\""` execute as intended in Splunk by preventing the loss or alteration of escape sequences during processing. Defaults to toggled on.
    - **Encoding**: Character encoding to use when parsing ingested data. If not set, Cribl Stream will default to UTF-8 but might incorrectly interpret multi-byte characters. This option is ignored for Parquet files. UTF-16LE and Latin-1 are also supported.
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
1.  Optionally, configure any [Result](#result-settings), [Result Routing](#result-routing), and [Advanced](#advanced-settings) settings outlined in the sections below.
    > Cribl offers a **Splunk Search Ruleset** Event Breaker that is designed for data from Splunk queries.
    {.box .success}
1.  Select **Save**, then **Commit & Deploy**. 
1.  To verify that the Collector actually collects data, you can start a single run
in the [Preview](/stream/collectors-schedule-run#mode/) mode.

> The sections described below are spread across several tabs. Select the tab links at left to navigate among tabs. 
> 
> Collector Sources currently cannot be selected or enabled in the QuickConnect UI.
>
{.box .info}

### Honor Retry-After Header {#retry}
 Cribl Stream always ignores `retry-after` headers that specify a delay longer than 20 seconds.
     -  Cribl Stream will log a warning message with the delay value retrieved from the `retry-after` header (converted to ms).
     -  When toggled off, Cribl Stream ignores all `retry-after` headers.

### Extra Parameters {#param}

The extra parameters refine or narrow the request. Select  **Add Parameter** to add parameters as key-value pairs:
- **Field Name**: Unique name for the field you're adding.
- **Value**: JavaScript expression to compute the field's value (can be a constant).  

### Extra Headers {#headers}
 By adding the appropriate **Extra headers**, you can specify authentication based on API keys using [secrets](#extra-headers-auth) as an alternative to the **Authentication** options above. The key-value pairs are: 
 - **Name**: Header name.
 - **Value**: JavaScript expression to compute the header's value (can be a constant).
 
## Result Settings {#result-settings}

The Result Settings determine how Cribl Stream transforms and routes the collected data. 

#### Custom Command

In this section, you can pass the data from this input to an external command for processing, before the data continues downstream.

**Enabled**: Defaults to toggled off. Toggle on to enable the custom command.

**Command**: Enter the command that will consume the data (via `stdin`) and will process its output (via `stdout`).

**Arguments**: Select **Add Argument** to add each argument to the command. You can drag arguments vertically to resequence them.

#### Event Breakers

In this section, you can apply event breaking rules to convert data streams to discrete events.

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied, in order, to the input data stream. Defaults to `System Default Rule`. Cribl offers a **Splunk Search Ruleset** designed for data from Splunk queries.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

[Snippet not found: content/shared/4.14/snippets/_sources-add-fields.md]

#### Result Routing {#result-routing}

**Send to Routes**: Toggle on (default) if you want Cribl Stream to send events to normal routing and event processing. Toggle off to select a specific Pipeline/Destination combination. Toggling off exposes these two additional fields:
- **Pipeline**:  Select a Pipeline to process results.
- **Destination**: Select a Destination to receive results.

Toggling on (default) exposes this field:
- **Pre-processing Pipeline**: Pipeline to process results before sending to Routes. Optional.

This field is always exposed:
- **Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, `GB`, and so on. (Example: `42 MB`.) Default value of `0` indicates no throttling.

> You might toggle **Send to Routes** off when configuring a Collector that will connect data from a specific Source to a specific Pipeline and Destination. One use case might be a Splunk Search Collector that gathers a known, simple type of data. This approach keeps the Collector's configuration self‑contained and separate from Cribl Stream's routing table for live data – potentially simplifying the Routes structure.
>
{.box .info}

**Pre-processing Pipeline**: Pipeline to process results before sending to Routes. Optional, and available only when **Send to Routes** is toggled on.

**Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, `GB`, and so on. (Example: `42 MB`.) Default value of `0` indicates no throttling.

## Advanced Settings {#advanced-settings}

Advanced Settings enable you to customize post-processing and administrative options.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Remove Discover fields**: List of fields to remove from the Discover results. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface. You can specify wildcards (such as `aws*`).

**Resume job on boot**: Toggle on to resume ad hoc collection jobs if Cribl Stream restarts during the jobs' execution.

## Internal Fields {#internal-fields}

Cribl Stream uses a set of internal fields to assist in data handling. These "meta" fields are **not** part of an event, but they are accessible, and you can use them in [Functions](functions) to make processing decisions.

Relevant fields for this Collector:

* `__collectible` – This object's nested fields contain metadata about each collection job.
  * `collectorType`: Indicates the type of Collector used for the job.
  * `collectorId`: Represents the **Collector ID** of the Collector, as configured during setup.
* `__inputId` – Uniquely identifies the origin of data for a [collection job](collectors-schedule-run). Its format varies depending on whether the job is ad hoc or scheduled:
  * Ad hoc jobs are formatted as `collection:<timestamp>.<randomId>.adhoc.<Collector ID>`.
    * `<timestamp>`: The Unix timestamp when the job was initiated.
    * `<randomId>`: A random identifier to ensure uniqueness.
    * `adhoc`: Indicates the job was manually triggered.
    * `<Collector ID>`: The ID of the Collector.
  * Scheduled jobs are formatted as `collection:<Collector ID>`.
    * `<Collector ID>`: The ID of the Collector.

## How Cribl Stream Pulls Data

This Collector will gather data from the specified **Search head** URL. If you enable [scheduled collection](collectors-schedule-run), searches will repeat on the interval specified in the **Schedule** modal's **Cron schedule** field. 

A single Worker executes each collection job. If the Leader goes down, search jobs in progress will complete, but future scheduled searches will not run until the Leader relaunches.

## Mitigating Stuck-Job Problems

Occasionally, a scheduled job fails, but continues running for hours or even days, until someone intervenes and cancels it. If left alone, such a "stuck", "orphaned," or "zombie" job will never complete. This can cause missing events in downstream receivers, along with `HTTP timeout` or similar errors in Cribl Stream's logs.

To keep stuck jobs from running excessively long:

* First, try setting **Optional Settings** > **Request timeout (secs)** – whose default of `0` means "wait forever" – to a desired maximum duration.
* If adjusting **Timeout (secs)** does not fix the problem, try the global setting **Job Timeout** – whose default of `0` allows a job to run indefinitely – to a desired maximum duration. You'll find **Job Timeout** among the [task manifest and buffering limits](collectors-job-limits#task-manifest-and-buffering-limits). 

Using these settings in tandem works like this:

* **Timeout (secs)** limits the time that Cribl Stream will wait for an HTTP request to complete. 
* Then, if a job gets stuck and keeps running beyond that limit, **Job Timeout** can catch and terminate the job, because it monitors the overall time the job has been running.
