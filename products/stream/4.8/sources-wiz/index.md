# Wiz Source


Cribl Stream supports collecting data from the [Wiz](https://www.wiz.io/) cloud security platform. The Wiz Source will communicate with APIs that your organization's Wiz portal exposes: Audit Logs, Configuration Findings (sometimes called Cloud Configuration), Issues, and Vulnerabilities.

> Type: **Pull**  |  TLS Support: **No** | Event Breaker Support: **No**
>
{.box .info}

> The Wiz Source polls the Wiz API using the `createdAt` timestamp to fetch new events. If an already ingested event, such as an Issue, is updated in Wiz, the Wiz Source won't recognize these changes because it doesn't track the `updatedAt` timestamp. To capture these modifications, you can use a custom [REST Collector](collectors-rest). You can adapt the existing Wiz REST Collector [template](https://github.com/criblio/collector-templates/tree/main/collectors/rest/wiz), ensuring you modify it to utilize the `updatedAt` field in your API calls and event breaking logic instead of relying on `createdAt`.
{.box .success}

## Prerequisites

Reference the [Wiz integration](https://docs.wiz.io/wiz-docs/docs/cribl-integration) documentation for gathering your Wiz account's unique GraphQL API endpoint URL and authentication information. You'll provide these in the Cribl Stream Wiz Source configuration.

## Configuring a Wiz Source 

In Cribl Stream, set up a Wiz Source.

1. From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:
    * To configure via the graphical [QuickConnect](quickconnect) UI, click **Routing** then **QuickConnect**. Next, click **Add Source** and from the resulting drawer's tiles, select **Wiz**. Next, click either **Add Source** or (if displayed) **Select Existing**.
    * To configure via the [Routing](routes) UI, click **Data** then **Sources**. Select **Wiz** from the list of tiles or the **Sources** left nav. Next, click **Add Source** to open the **Add Source** modal.

1. In the Source modal, configure the following under **General Settings**:
    * **Input ID**: Enter a unique name to identify this Wiz Source definition. If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**. 
    * **GraphQL endpoint**: Enter the Wiz GraphQL API endpoint URL. The UI prefills with a URL that you need to update with the appropriate `<region>`, `https://api.<region>.app.wiz.io/graphql`. For example, `https://api.us1.app.wiz.io/graphql`.
    * **Authentication URL**: Select the authentication URL to generate an OAuth token. If you're using a custom or self-hosted identity provider, enter the specific URL provided by your organization.
      * **Authentication audience**: When using a custom **Authentication URL** you can define the audience to use when requesting an OAuth token. This is used to validate that the token is meant for your application. Enter the audience value required by your identity provider. Defaults to `wiz-api`.
    * **Client ID**: Enter the Wiz **Client ID** (53-character string).
    * **Authentication method**: Enter the Wiz **Client secret** (64-character string) password credentials manually or with a [secret](securing-secrets). When choosing **Secret**, you'll select a stored text secret in the resulting **Client Secret (text secret)** drop-down, or click **Create** to configure a new secret.
    * **Content types**: Toggle **Enable content** to `Yes` for the API types you want to query. The available types are **Audit Logs**, **Configuration Findings**, **Issues**, and **Vulnerabilities**.
      * The schedule and query are automatically configured for you. See [scheduling options](#scheduling) for details on how these options work.
      * Toggle [**State tracking**](#state-tracking) to `Yes` to track state by time, which prevents overlaps between collection jobs where subsequent jobs may return some of the same results as previous jobs. Similarly, it can help prevent gaps in data by allowing the next run to pick up from where the last run ended.
    * **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream’s **Manage Sources** page. These tags aren’t added to processed events. Use a tab or hard return between (arbitrary) tag names.

1. The remaining options, [Processing Settings](#processing), [Retries](#retries), and [Advanced Settings](#advanced), are already configured for Wiz data collection.

1. Click **Save**, then **Commit & Deploy**.

1. Verify that data is making it to Cribl Stream by viewing the [**Live Data**](sources#capturing-source-data) feed from the Source.

### Processing Settings {#processing}

   * **Fields**: You can add Fields to each event, using [Eval](eval-function)-like functionality. A field consists of a **Name** and **Value** pair. The **Value** is a JavaScript expression and can be a constant.
   * **Pre-Processing**: Select a **Pipeline** to process results before sending to Routes. Optional, and available only when **Send to Routes** is toggled to `Yes`.

### Retries {#retries}

Optionally adjust the default retry settings for failed collect jobs.

   * **Retry type**: The algorithm to use when performing HTTP retries. Options include `Backoff` (the default), `Static`, and `Disabled`.
   * **Initial retry interval (ms)**: Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute). A value of `0` means retry immediately until reaching the retry limit in **Max retries**. 
   * **Max retries**: Maximum number of times to retry a failed HTTP request. Defaults to `5`. Maximum: `20`. A value of `0` means don't retry at all.
   * **Backoff multiplier**: Base for exponential backoff. A value of `2` (default) means that Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, and so forth.
   * **Retry HTTP codes**: List of HTTP codes that trigger a retry. Leave empty to use the defaults (`429` and `503`). Cribl Stream does not retry codes in the `200` series.
   * **Honor Retry-After header**: When toggled to `Yes` (the default), `retry-after` [headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) up to a maximum of 20 seconds are processed. Delays longer than 20 seconds are ignored.
     * Cribl Stream will log a warning message with the delay value retrieved from the `retry-after` header (converted to ms).
     * When toggled to `No`, Cribl Stream ignores all `retry-after` headers.
   * **Retry Connection Timeout**: Toggle to `Yes` to automatically retry a single connection attempt after a timeout (`ETIMEDOUT`) to ensure data continuity.
   * **Retry Connection Reset**: Toggle to `Yes` to automatically retry a connection after a peer reset (`ECONNRESET`) to maintain data flow.

### Advanced Settings {#advanced}

**Advanced Settings** enable you to customize post-processing and administrative options.

   * **Request Timeout (seconds)**: The maximum time period for an HTTP request to complete before Cribl Stream treats it as timed out. Defaults to `0`, which disables timeout metering.
   * **Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.
   * **Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

**Connected Destinations** mimics which method you used to open the Wiz configuration modal in step 1, either the QuickConnect UI or the Routes UI. **Send to Routes** enables conditional routing, filtering, and cloning of this Source's data via the Routing table. **QuickConnect** allows you to send this Source’s data to one or more Destinations via independent, direct connections.

## Scheduling {#scheduling}

When you enable a **Content type** under **General Settings** the following scheduling options are available:

The [**Cron schedule**](https://en.wikipedia.org/wiki/Cron) configures when collection requests are made.

**Job timeout**: Maximum time this job will be allowed to run. Units are seconds, if not specified. Sample values: `30`, `45s`, or `15m`. Minimum granularity is 10 seconds, so a `45s` value would round up to a 50-second timeout. Defaults to `0`, meaning unlimited time (no timeout).

**Log Level**: Level at which to set task logging. More verbose levels are useful for troubleshooting jobs and tasks, but use them sparingly.

The **Earliest time** and **Latest time** defines the time range of events to collect, based on the `_time` field. These fields accept the following syntax:

`[+|-]<time_integer><time_unit>@<snap-to_time_unit>` 

To break down this syntax:

Syntax Element | Values Supported
--- | ---
Offset | Specify: `-` for times in the past, `+` for times in the future, or omit with `now`.
<time_integer> | Specify any integer, or omit with `now`.
<time_unit> | Specify the `now` constant, or one of the following abbreviations: `s[econds]`, `m[inutes]`, `h[ours]`, `d[ays]`, `w[eeks]`, `mon[ths]`, `q[uarters]`, `y[ears]`.
@<snap-to_time_unit> | Optionally, you can append the `@` modifier, followed by any of the above `<time_unit>`s, to round down to the nearest instance of that unit. (See the next section for details.)

Cribl Stream validates relative time values using these rules:

- **Earliest** must not be later than **Latest**.
- Values without units get interpreted as seconds. For example, `-1` = `-1s`.

#### Snap-to-Time Syntax

The `@` snap modifier always rounds **down** (backwards) from any specified time. This is true even in relative time expressions with `+` (future) offsets. For example:

- `@d` snaps back to the beginning of today, 12:00 AM (midnight). 

- `+128m@h` looks forward 128 minutes, then snaps back to the nearest round hour. (If you specified this in the `Latest` field, and ran the Source at 4:20 PM, collection would end at 6:00 PM. The expression would look forward to 6:28 PM, but snap back to 6:00 PM.)

Other options:

- `@w` or `@w7` to snap back to the beginning of the week – defined here as the preceding Sunday.
- To snap back to other days of a week, use `w1` (Monday) through `w6` (Saturday).
- `@mon` to snap back to the 1st of a month.
- `@q` to snap back to the beginning of the most recent quarter – Jan. 1, Apr. 1, Jul. 1, or Oct. 1.
- `@y` to snap back to Jan. 1.

## Working with State Tracking {#state-tracking}

You can configure the Source to track state, either by time or another arbitrary value. This can help prevent overlaps between jobs, where subsequent runs may return some of the same results as previous runs. Similarly, it can help prevent gaps in data by allowing a run to pick up from where the last run ended.

**State update expression**: JavaScript expression that defines how to update the state from an event. Use the event's data and the current state to compute the new state.

**State merge expression**: JavaScript expression that defines which state to keep when merging a task's newly reported state with the previously saved state. Evaluates `prevState` and `newState` variables, resolving to the state to keep.

The default values for these fields are configured to track state by the latest `_time` field found in events gathered in a collection run.

### Understanding State Expression Fields {#state-tracking-expression-fields}

 The **State update** and **State merge** expressions control how state is derived from a collection run and how it is merged with existing state, respectively. They're preconfigured to work with the common use case of tracking state by latest `_time`, but you may need to update them for other use cases. To understand what these fields do, let's break down the default values.

#### State update expression
This expression has a default value of:
```js
__timestampExtracted !== false && {latestTime: (state.latestTime || 0) > _time ? state.latestTime : _time}
```
**__timestampExtracted !== false** - the `__timestampExtracted` field is set to false if the Event Breaker was unable to parse time for the event. If this is the case, we don't want to update state (the event's `_time` value defaults to `Date.now()` if the Event Breaker was unable to parse out the correct time). If `_timestampExtracted` is `false`, we take advantage of [short-circuit evaluation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND#short-circuit_evaluation) to not update state. 

State values must resolve to an object, such as:

```json
{ "latestTime": 17122806161 }
```
If the expression does not resolve to an object, Cribl Stream will ignore the result.

**{latestTime: (state.latestTime || 0) > _time ? state.latestTime : _time}** - compare `state.latestTime` to the event's `_time` value, keeping whichever value is greater.

#### State merge expression
This expression has a default value of:

```js
prevState.latestTime > newState.latestTime ? prevState : newState
```
It compares `prevState` (the state that was previously saved) to `newState` (the state reported from the most recent collection task), keeping the state with the greatest `latestTime` value.

### Managing State

Click **Manage State** to view, modify, or delete a state. For more information, see [Manage State](collectors-manage-state). 

## Wiz API Results Limits {#limits}

The table below shows the results limits for each Wiz API. 

API | Limit (results)
--- | ------
Audit Logs | 10,000
Cloud Configuration | 10,000
Issues | no limit
Vulnerability | no limit

> Once your query hits the limit of results for a given Wiz API, the API will send no further results. 
>
{.box .warning}

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## Internal Fields {#internal-fields}

Cribl Stream uses a set of internal fields to assist in data handling. These "meta" fields are **not** part of an event, but they are accessible, and you can use them in [Functions](functions) to make processing decisions.

Relevant fields for this Source:

* `__collectible` – This object's nested fields contain metadata about each collection job.
* `__collectStats` – This object's nested fields are `method`, `url`, and `elapsedMS`.

## Troubleshooting

Navigate to and open the Wiz Source, then use the Source's **Job Inspector** tab to see the results of recent collection runs.

[Snippet not found: content/shared/4.8/snippets/_sources-troubleshooting.md]

### Response Errors

The Source treats all non-`200` responses from configured URL endpoints as errors. This includes `1xx`, `3xx`, `4xx`, and `5xx` responses.

A few exceptions are treated as non-fatal errors:

- Where a collect job launches multiple tasks, and only a subset of those tasks fail, Cribl Stream places the job in failed status, but treats the error as non-fatal. (Note that Cribl Stream does not retry the failed tasks.)

- Where a collect job receives a `3xx` redirection error code, it follows the error's treatment by the underlying library, and does not necessarily treat the error as fatal.
