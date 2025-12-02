# Splunk Search Source


Cribl Edge supports receiving Splunk search data from [Splunk Search](https://docs.splunk.com/Documentation/Splunk/8.2.4/SearchReference/Search).

> Type: **Pull**  |  TLS Support: **Yes** | Event Breaker Support: **YES**
>
{.box .info}

## Configure Cribl Edge to Receive Splunk Search Data  

In Cribl Edge, configure Splunk Search Data: 

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
     - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect**. Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**. 
     - To configure via the [Routes](routes), select **Data** > **Sources**. Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
     - **Input ID**: Enter a unique name to identify this Splunk Search Source definition. If you clone this Source, Cribl Edge will add `-CLONE` to the original **Input ID**.  
     - **Description**: Optionally, enter a description.
     -  **Cron schedule**: Enter a [cron expression](https://crontab.cronhub.io/) to define the schedule on which to run this job. Defaults to one run every 15 minutes. The **Estimated Schedule** below this field shows the next few collection runs, as examples of the cron interval you've scheduled. You enter the **Cron schedule** expression in UTC time, but the **Estimated Schedule** examples are displayed in local time.
3.  Under **Search**, configure the following:
    - **Search**: Enter the Splunk query. For example: `index=myAppLogs level=error channel=myApp` OR `| mstats avg(myStat) as myStat WHERE index=myStatsIndex`.
    - **Search head**: Enter the search head base URL. The default is `https://localhost:8089`.  
    - **Earliest**: You can enter the earliest time boundary for the search. This maybe be an exact or relative time. For example: `2022-01-14T12:00:00Z` or `-16m@m`. 
    - **Latest**: You can enter the latest time boundary for the search. This maybe be an exact or relative time. For example: `2022-01-14T12:00:00Z` or `-16m@m`.  
4. Next, you can configure the following **Optional Settings**: 
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
5. Under **Authentication**, select the **Authentication type** from the dropdown:
   - **None**: Don't use authentication. Compatible with REST servers like AWS, where you embed a secret directly in the request URL.
   - **Basic**: Displays **Username** and **Password** fields for you to enter HTTP Basic authentication credentials.
   - **Basic (credentials secret)**: Provide username and password credentials referenced by a secret. Select a stored text secret in the resulting **Credentials secret** drop-down, or click **Create** to configure a new secret.
   - **Bearer Token**: Provide the `token` value configured and generated in Splunk.
   - **Bearer Token (text secret)**: Provide the Bearer Token referenced by a secret. Select a stored text secret in the resulting **Token (text  secret)** drop-down, or click **Create** to configure a new secret.
6. Optionally, you can adjust the [Processing](#processing), [Retries](#retries), and [Advanced](#advanced-settings) settings outlined in the sections below.
7. Select **Save**, then **Commit & Deploy**.


### Processing Settings {#processing}

#### Event Breakers

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied to the input data stream before the data is sent through the Routes. Defaults to the `Splunk Search Ruleset`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Retries {#retries}

**Retry type**: The algorithm to use when performing HTTP retries. Options include `Backoff` (the default), `Static`, and `Disabled`.

**Initial retry interval (ms)**: Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute). A value of `0` means retry immediately until reaching the retry limit in **Max retries**. 

**Max retries**: Maximum number of times to retry a failed HTTP request. Defaults to `5`. Maximum: `20`. A value of `0` means don't retry at all.

**Backoff multiplier**: Base for exponential backoff. A value of `2` (default) means that Cribl Edge will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.

**Retry HTTP codes**: List of HTTP codes that trigger a retry. Leave empty to use the defaults (`429` and `503`). Cribl Edge does not retry codes in the `200` series.

**Honor Retry-After header**: When toggled to `Yes` (the default) and the `retry-after` [header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) is present, Cribl Edge honors any `retry-after` header that specifies a delay, up to a maximum of 20 seconds. Cribl Edge always ignores `retry-after` headers that specify a delay longer than 20 seconds.
  * Cribl Edge will log a warning message with the delay value retrieved from the `retry-after` header (converted to ms).
  * When toggled to `No`, Cribl Edge ignores all `retry-after` headers.

**Retry Connection Timeout**: Toggle to `Yes` to automatically retry a single connection attempt after a timeout (`ETIMEDOUT`) to ensure data continuity.

**Retry Connection Reset**: Toggle to `Yes` to automatically retry a connection after a peer reset (`ECONNRESET`) to maintain data flow.

### Advanced Settings  

**Reject unauthorized certificates**: Whether to accept certificates (such as self-signed certificates, for example) that cannot be verified against a valid Certificate Authority. Defaults to `Yes`.

**Search endpoint**: Rest API used to conduct a search. Defaults to `services/search/jobs/export`.

**Output mode**: Format of the returned output. Defaults to JSON format.To parse the returned JSON, add the Cribl event breaker which parses newline delimited events in the [Event Breakers](#event-breakers) tab.

 Events returned from Splunk search can also be returned in the more compact CSV format. To use CSV format, set the **Output mode** to CSV and specify the CSV event breaker in the [Event Breakers](#event-breakers) tab.

**Endpoint parameters**: Optional HTTP request parameters to append to the request URL. These refine or narrow the request. Click **Add Parameter** to add parameters as key-value pairs:  
- **Name**: Field name.
- **Value**: JavaScript expression to compute the field's value (can be a constant).  

**Endpoint headers**:: Click **Add Header** to (optionally) add request headers to send to the endpoint, as key-value pairs:

  * **Name**: Header name.
  * **Value**: JavaScript expression to compute the header's value, normally enclosed in backticks (for example, `` `${earliest}` ``). Can also be a constant, enclosed in single quotes (`'earliest'`). Values without delimiters (for example, `earliest`) are evaluated as strings.

**Log level**: Set the verbosity level for the data collection's runtime log.

**Job timeout**: Maximum time a job is allowed to run. Defaults to `0`, for unlimited time. Units are seconds if not specified. Sample entries: `30`, `45s`, `15m`.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Request Timeout (secs)**: Here, you can set a maximum time period (in seconds) for an HTTP request to complete before Cribl Edge treats it as timed out. Defaults to `0`, which disables timeout metering.



**Encoding**: Character encoding to use when parsing ingested data. If not set, Cribl Stream will default to UTF-8 but might incorrectly interpret multi-byte characters. This option is ignored for Parquet files. UTF-16LE and Latin-1 are also supported.

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Edge will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.  

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Edge uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

* `__inputId`
* `__outputMode`

## How Cribl Edge Pulls Data

This Collector-based Source will gather data from the specified **Search head** URL repeatedly, on the interval specified in the **Cron schedule** field. A single Worker executes each collection job.

If the Leader goes down, search jobs in progress will complete, but future scheduled searches will not run until the Leader relaunches.

## Mitigating Stuck-Job Problems

Occasionally, a scheduled job fails, but continues running for hours or even days, until someone intervenes and cancels it. If left alone, such a "stuck", "orphaned," or "zombie" job will never complete. This can cause missing events in downstream receivers, along with `HTTP timeout` or similar errors in Cribl Edge's logs.

To keep stuck jobs from running excessively long:

* First, try setting **Advanced** > **Request Timeout (secs)** – whose default of `0` means "wait forever" – to a desired maximum duration.
* If adjusting **Timeout (secs)** does not fix the problem, try setting **Advanced** > **Job Timeout** – whose default of `0` allows a job to run indefinitely – to a desired maximum duration. 

Using these settings in tandem works like this:

* **Timeout (secs)** limits the time that Cribl Edge will wait for an HTTP request to complete. 
* Then, if a job gets stuck and keeps running beyond that limit, **Job Timeout** can catch and terminate the job, because it monitors the overall time the job has been running.

## Troubleshooting

[Snippet not found: content/shared/4.9/snippets/_sources-troubleshooting.md]
