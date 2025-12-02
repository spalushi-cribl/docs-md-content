# Health Check Collector



The Health Check Collector monitors endpoints by sending requests and generating events based on the responses. This Collector provides multiple Discover types and Health Check options. 

## Configure a Health Check Collector 


1. Navigate to **Products** > **Stream** > **Worker Groups**. Select a Worker Group, then go to **Data** > **Sources**. Choose the Source and select **Add Collector**. 
2. In the **New Collector** modal, configure the following under **Collector Settings**:
   - **Collector ID**: Unique ID for this Collector. For example, `healthMonitoring`. If you clone this Collector, Cribl Stream will add `-CLONE` to the original **Collector ID**.
   - **Description**: Optionally, enter a description.
3. Within the **Discover** accordion, the **Discover type** drop-down provides four options, corresponding to different use cases. Each **Discover type** selection will expose a different set of fields. Below, we cover the **Discover type**s from the simplest to the most complex. 
   - **[Discover type: None](#common)** matches cases where one simple API call will retrieve all the data you need. This default option suppresses the Discover stage. Use the modal's **Health check** section to specify the endpoint and other details. Cribl Stream will assign the Health Check task to a single Worker.

   - **[Discover type: Item List](#list)** matches cases where you want to enumerate a known list of **Discover items** to retrieve, like specific AWS services to identify hosts to check. Discovery will return one Health Check task per item in the list, and Cribl Stream will spread each of those tasks across all available Workers.

   - **[Discover type: JSON Response](#json)** provides a **Discover result** field where you can (optionally) define Discover tasks as a JSON array of objects. Each entry returned by Discover will generate a Health Check task, and Cribl Stream will spread each of those tasks across all available Workers.

   - **[Discover type: HTTP Request](#http)** matches cases where you need to dynamically discover what you can Health Check from a REST endpoint. This Discover type most fully exploits Cribl Stream's Discover-and-then-health-check architecture. (Example: Make a REST call to get a list of available hosts, then run Health Checks against each of those hosts.) Each item returned will generate a Health Check task, and Cribl Stream will spread each of those tasks across all available Workers.
4. In the **Authentication** accordion, use the **Authentication** drop-down to select one of these options:
   - **None**: Don't use authentication. Compatible with REST servers like AWS, where you embed a secret directly in the request URL.
   - **Basic**: Displays **Username** and **Password** fields for you to enter HTTP Basic authentication credentials.
   - **Basic (credentials secret)**: Provide username and password credentials referenced by a secret. Select a stored text secret in the resulting **Credentials secret** drop-down, or select **Create** to configure a new secret.
   - **Login**: Enables you to specify several credentials, then perform a POST to an endpoint during the Discover operation. The POST response returns a token, which Cribl Stream uses for later Health Check operations. Exposes multiple extra fields – see  [Login Authentication](#login-auth) below.
   - **Login (credentials secret)**: Like **Login**, except that you specify a secret referencing the credentials, rather than the credentials themselves. Exposes multiple extra fields – see  [Login Secret Authentication](#login-secret-auth) below.
   - **OAuth**: Enables you to directly enter credentials for authentication via the OAuth protocol. Exposes multiple extra fields – see [OAuth Authentication](#oauth-auth) below.
   - **OAuth (text secret)**: Like the **OAuth** option, except that you specify a stored secret referencing the credentials. Exposes multiple extra fields – see [OAuth Secret Authentication](#oauth-secret-auth) below.
5. In the **Health Check** accordion, configure the following:
    - **Health check URL**: URL (constant or JavaScript expression) to use for the Health Check operation. Refer to [Health Checks](#common) below for details. 
    - **Health check method**: Select the HTTP verb to use for the Health Check operation – `GET`, `POST`, or `POST with body`.
    - **Health check POST body**: Template for POST body to send with the Health Check request. (This field is displayed only when you set the **Health check method** to `POST with body`.) You can reference parameters from the Discover response using template params of the form: `` `${variable}` ``.
    - **Health check parameters**: Optional HTTP request parameters to append to the request URL. These refine or narrow the request. For details, see [Health Check Parameters](#param). 
   - **Health check headers**: Optional HTTP request headers to append to the request URL. For details, see [Health Check Headers](#headers).
   -  **Authenticate health check**: Toggle on if you want to authenticate Health Checks. Toggle off (default) to apply [**Authentication**](#authentication) only to Discover.

      > By adding the appropriate **Health check headers**, you can specify authentication based on API keys, as an alternative to the **Authentication**: `Basic` or `Login` options below.
      >
      {.box .success}

6. In the **Retries** accordion, configure the following:
      - **Retry type**: The algorithm to use when performing HTTP retries. Options include `Backoff` (the default), `Static`, and `Disabled`.
      - **Initial retry interval (ms)**: Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute). A value of `0` means retry immediately until reaching the limit specified in **Retry limit**. 
      - **Retry limit**: Maximum number of times to retry a failed HTTP request. Defaults to `5`. Maximum: `20`. A value of `0` means don't retry at all.
      - **Backoff multiplier**: Base for exponential backoff. A value of `2` (default) means that Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      - **Retry HTTP codes**: List of HTTP codes that trigger a retry. Leave empty to use the defaults (`429` and `503`). Cribl Stream does not retry codes in the `200` series.
      - **Honor Retry-After header**: If toggled on (default) and the `retry-after` [header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) is present, Cribl Stream honors any `retry-after` header that specifies a delay, up to a maximum of 20 seconds. Cribl Stream always ignores `retry-after` headers that specify a delay longer than 20 seconds.

        Cribl Stream will log a warning message with the delay value retrieved from the `retry-after` header (converted to ms).

        Toggle off if you want Cribl Stream to ignore all `retry-after` headers.

7. Next, you can configure the following **Optional Settings**: 
     - **Request Timeout (secs)**: Here, you can set a maximum time period (in seconds) for an HTTP request to complete before Cribl Stream treats it as timed out. Defaults to `0`, which disables timeout metering.
     - **Reject unauthorized certificates**: Whether to accept certificates that cannot be verified against a valid Certificate Authority (for example, self-signed certificates). Defaults to toggled on.
     - **Safe headers**: Optionally, list headers that you consider safe to log in plain text. Separate the header names with tabs or hard returns.
     - **Encoding**: Character encoding to use when parsing ingested data. If not set, Cribl Stream will default to UTF-8 but might incorrectly interpret multi-byte characters. This option is ignored for Parquet files. UTF-16LE and Latin-1 are also supported.
     - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
8. Optionally, configure any [Result](#result-settings), [Result Routing](#result-routing), and [Advanced](#advanced-settings) settings outlined in the sections below.
9. Select **Save**, then **Commit & Deploy**. 
10. To verify that the Collector actually collects data, you can start a single run in the [Preview](/stream/collectors-schedule-run#mode/) mode.


> The sections described below are spread across several tabs, some of which contain collapsible accordions. Click the tab links at left to navigate among tabs. Click **Save** when you've configured your Collector.
> 
> See [Formatting Expressions](#expressions) below for guidance on this Collector's specific requirement to enter JavaScript expressions as template literals.
> 
> Collector Sources cannot be selected or enabled in the QuickConnect UI.
>
{.box .info}


### Discover {#discover}

This section guides you through configuring Discover Types.

#### Discover Type: Item List {#list}

Setting the **Discover type** to `Item List` exposes this additional field above the [Health Check settings](#common):

**Discover items**: List of items to return from the Discover task. Each returned item will generate a Health Check task, and can be referenced using `${id}` in the **Health Check URL**, the **Health Check parameters**, or the **Health Check headers**. Sample list with Cribl's [health endpoint](monitoring#endpoint):

```
http://localhost:8080/services/collector/health,http://0.0.0.0:3000/services/collector/health
```

#### Discover Type: JSON Response {#json}

Setting the **Discover type** to `JSON Response` exposes these additional fields above the [Health Check settings](#common):

**Discover result**: Allows hard-coding the Discover result. Must be a JSON object. Works with the **Discover data field**. Sample JSON entry:

```json
 `[ {"url": "http://localhost:8088/services/collector/health"},{ "url": "http://localhost:8088/services/collector/health/1.0" } ]`
```

**Discover data field**: Within the response, this is the name of the field that contains discovery results. (Leave blank if the result is an array.) Sample JSON entry:

```json
items, json: { items: [{id: 'first'},{id: 'second'}] }
```

In an XML response, this is the name of the element that contains discovery results. Sample XML entry:

```xml
result.items 
<response>
 <items>
  <element>
   <id>first</id>
  </element>
  <element>
   <id>second</id>
  </element>
 </items>
</response>
```

#### Discover Type: HTTP Request {#http}

Setting the **Discover type** to `HTTP Request` exposes these additional fields above the [Health Check settings](#common):

**Discover URL**: Enter the URL to use for the Discover operation. This can be a constant URL, or a JavaScript expression to derive the URL.

> Where a URL (path or parameters) includes variables that might contain unsafe ASCII characters, encode these variables using `C.Encode.uri(paramName)`. (Examples of unsafe characters are: space, `$`, `/`, `=`.) Example URL with encoding: ``'http://localhost:9000/api/v1/system/logs/' + C.Encode.uri(`${id}`)``. 
> 
> Request parameters that are not contained directly in the URL are automatically encoded. Cribl Stream URLs/expressions specified in the **Discover URL** field follow redirects.
>
{.box .info}

**Discover method**: Select the HTTP verb to use for the Discover operation – `GET`, `POST`, or `POST with body`.

**Discover POST body**: Template for POST body to send with the Discover request. (This field is displayed only when you set the **Discover method** to `POST with body`.)

**Discover parameters**:  Optional HTTP request parameters to append to the Discover request URL. These refine or narrow the request. Click **Add Parameter** to add parameters as key-value pairs:

  * **Name**: Parameter name.
  * **Value**: JavaScript expression to compute the parameter's value, normally enclosed in backticks (for example, `` `${id}` ``). Can also be a constant, enclosed in single quotes (`'id'`). Values without delimiters (for example, `id`) are evaluated as strings.

**Discover headers**: Optional Discover request headers.: Click **Add Header** to add headers as key-value pairs:

  * **Name**: Header name.
  * **Value**: JavaScript expression to compute the header's value, normally enclosed in backticks (for example, `` `${id}` ``). Can also be a constant, enclosed in single quotes (`'id'`). Values without delimiters (for example, `id`) are evaluated as strings.

**Discover data field**: Within the response JSON, name of the field  that contains Discover results. Leave blank if the result is an array.

> The following sections describe the remaining tabs, whose settings and content apply equally to all **Discover type** selections.
>
{.box .success}

### Health Check {#common}

Each request that this Collector sends constitutes one "health check." One source of insight into the health of the endpoint is the [internal fields](#internal-fields) that the Collector emits. For example, to observe how slowly or quickly an endpoint is responding, you can monitor the value of the `elapsedMS` element nested in the `__collectStats` internal field.

> Where a URL (path or parameters) includes variables that might contain unsafe ASCII characters, encode these variables using `C.Encode.uri(paramName)`. (Examples of unsafe characters are: space, `$`, `/`, `=`.) Example URL with encoding: ``'http://localhost:9000/api/v1/system/logs/' + C.Encode.uri(`${id}`)``. 
> 
> Request parameters that are not contained directly in the URL are automatically encoded. Cribl Stream URLs/expressions specified in the **Health check URL** field follow redirects.
>
{.box .info}

### Health Check Parameters {#param}
In **Health check parameters**, select **Add parameter** to add parameters as key-value pairs:
   - **Field Name**: Unique name for the field you're adding.
   - **Value**: JavaScript expression to compute the field's value, normally enclosed in backticks (for example, `` `${id}` ``). Can also be a constant, enclosed in single quotes (`'id'`). Values without delimiters (for example, `id`) are evaluated as strings.

### Health check headers {#headers}
In **Health check headers**, select **Add Header** to (optionally) add request headers as key-value pairs:
  - **Name**: Header name.
  - **Value**: JavaScript expression to compute the header's value, normally enclosed in backticks (for example, `` `${id}` ``). Can also be a constant, enclosed in single quotes (`'id'`). Values without delimiters (for example, `id`) are evaluated as strings.



### Authentication {#authentication}

This section covers the process of configuring authentication options.

#### Login Authentication {#login-auth}

Selecting `Login` exposes the following additional fields:

- **Login URL**: URL for the login API call, which is expected to be a POST call.

- **Username**: Login username.

- **Password**: Login password.

- **POST body**: Template for POST body to send with the login request. The `${username}` and `${password}` variables specify the corresponding credentials' locations in the message.

- **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes.

- **Authorize expression**: JavaScript expression used to compute the Authorization header to pass in Discover and Health Check calls. Uses `${token}` to reference the token obtained from the login POST request.

- **Authentication headers**: Optionally, click **Add Header** for each custom auth header you want to define. In the resulting table, enter each header row's **Name**. 

  The corresponding **Value** is a JavaScript expression to compute the header's value. This can also evaluate to a constant. Values not formatted as expressions – for example, `id` instead of `` `${id}` `` – will be evaluated as strings.

#### Login Secret Authentication {#login-secret-auth}

Selecting **Login (credentials secret)** exposes the following additional fields:

- **Login URL**: URL for the login API call, which is expected to be a POST call.

- **Credentials secret**: Select a stored text secret in this drop-down, or click **Create** to configure a new secret.

- **POST body**: Template for POST body to send with the login request. The `${username}` and `${password}` variables specify the corresponding credentials' locations in the message.

- **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes.

- **Authorize expression**: JavaScript expression used to compute the Authorization header to pass in Discover and Health Check calls. Uses `${token}` to reference the token obtained from the login POST request.

- **Authentication headers**: Optionally, click **Add Header** for each custom auth header you want to define. In the resulting table, enter each header row's **Name**. 

  The corresponding **Value** is a JavaScript expression to compute the header's value. This can also evaluate to a constant. Values not formatted as expressions – for example, `id` instead of `` `${id}` `` – will be evaluated as strings.

#### OAuth Authentication {#oauth-auth}

Selecting **OAuth** exposes the following additional fields:

- **Login URL**: Endpoint for the OAuth API call, which is expected to be a POST call.

- **Client secret parameter**: The name of the parameter to send with the **Client secret value**.

- **Client secret value**: The OAuth access token to authorize requests.

- **Extra authentication parameters**: Optionally, click **Add parameter** for each additional OAuth request parameter you want to send in the body of POST requests. Automatically sets the `Content‑Type` header to `application/x‑www‑form‑urlencoded`. 

  In the resulting table, enter each parameter row's **Name**. The corresponding **Value** is a JavaScript expression to compute the parameter's value. This can also evaluate to a constant. Values not formatted as expressions – for example, `id` instead of `` `${id}` `` – will be evaluated as strings.

- **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes.

- **Authorize expression**: JavaScript expression used to compute the Authorization header to pass in Discover and Health Check calls. Uses `${token}` to reference the token obtained from the login POST request.

- **Authentication headers**: Optionally, click **Add Header** for each custom auth header you want to define. In the resulting table, enter each header row's **Name**. 

  The corresponding **Value** is a JavaScript expression to compute the header's value. This can also evaluate to a constant. Values not formatted as expressions – for example, `id` instead of `` `${id}` `` – will be evaluated as strings.

#### OAuth Secret Authentication {#oauth-secret-auth}

Selecting **OAuth (text secret)** exposes the following additional fields:

- **Login URL**: Endpoint for the OAuth API call, which is expected to be a POST call.

- **Client secret parameter**: The name of the parameter to send with the **Client secret value**.

- **Client secret value (text secret)**: Select a stored text secret in this drop-down, or click **Create** to configure a new secret.

- **Extra authentication parameters**: Optionally, click **Add parameter** for each additional OAuth request parameter you want to send in the body of POST requests. Automatically sets the `Content‑Type` header to `application/x‑www‑form‑urlencoded`. 

  In the resulting table, enter each parameter row's **Name**. The corresponding **Value** is a JavaScript expression to compute the parameter's value. This can also evaluate to a constant. Values not formatted as expressions – for example, `id` instead of `` `${id}` `` – will be evaluated as strings.

- **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes.

- **Authorize expression**: JavaScript expression used to compute the Authorization header to pass in Discover and Health Check calls. Uses `${token}` to reference the token obtained from the login POST request.

- **Authentication headers**: Optionally, click **Add Header** for each custom auth header you want to define. In the resulting table, enter each header row's **Name**. 

  The corresponding **Value** is a JavaScript expression to compute the header's value. This can also evaluate to a constant. Values not formatted as expressions – for example, `id` instead of `` `${id}` `` – will be evaluated as strings.


## Result Settings

The Result Settings determine how Cribl Stream transforms and routes the Health Check data. 

#### Custom Command

In this section, you can pass the data from this input to an external command for processing, before the data continues downstream.

- **Enabled**: Defaults to toggled off. Toggle on to enable the custom command.

- **Command**: Enter the command that will consume the data (via `stdin`) and will process its output (via `stdout`).

- **Arguments**: Click **Add Argument** to add each argument to the command. You can drag arguments vertically to resequence them.

#### Event Breakers

In this section, you can apply event breaking rules to convert data streams to discrete events.

- **Event Breaker rulesets**: A list of event breaking rulesets that will be applied, in order, to the input data stream. Defaults to `Cribl`.

- **Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

[Snippet not found: content/shared/4.13/snippets/_sources-add-fields.md]

#### Result Routing

**Send to Routes**: Toggle on (default) if you want Cribl Stream to send events to normal routing and event processing. Toggle off to select a specific Pipeline/Destination combination. Toggling off exposes these two additional fields:
- **Pipeline**:  Select a Pipeline to process results.
- **Destination**: Select a Destination to receive results.

Toggling on (default) exposes this field:
- **Pre-processing Pipeline**: Pipeline to process results before sending to Routes. Optional.

This field is always exposed:
- **Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, `GB`, etc. (Example: `42 MB`.) Default value of `0` indicates no throttling.

> You might toggle **Send to Routes** off when configuring a Collector that will connect data from a specific Source to a specific Pipeline and Destination. One use case might be a Health Check Collector that gathers a known, simple type of data from a single endpoint. This approach keeps the Collector's configuration self‑contained and separate from Cribl Stream's routing table for live data – potentially simplifying the Routes structure.
>
{.box .info}

## Advanced Settings

Advanced Settings enable you to customize post-processing and administrative options.

- **Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

- **Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

- **Remove Discover fields**: List of fields to remove from the Discover results. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface. You can specify wildcards (such as `aws*`).

- **Resume job on boot**: Toggle on to resume ad hoc Health Check jobs if Cribl Stream restarts during the jobs' execution.

## Formatting Expressions {#expressions}

JavaScript expression fields in this Health Check Collector behave differently from those elsewhere in Cribl Stream. Here, you must enter expressions as template literals, with placeholders referencing variables and JS functions. Here are a few examples: 
- To reference the `__collectible.id` internal field in a URL, header, or parameter (etc.), you would write the variable as `` `${id}` ``. 
- To reference a function call: `` `${Date.now()}` ``. 
- To call a function that references a variable: `` `${Math.floor(earliest)}` ``.

## Responses

The Health Check Collector generates error or success events based on the received response from the Health Check request.

### Error Events

The Health Check Collector generates error events when no response is received, a request times out, or receives an error (non 200) response. The following is an example error event:

```json
{
  "host": "Cribl.local",
  "_raw": "{\"url\":\"http://0.0.0.0:3000/services/collector/health\",\"method\":\"get\",\"statusCode\":404,\"error\":\"Http Error, statusCode: 404, details: {\\\"host\\\":\\\"0.0.0.0:3000\\\",\\\"port\\\":\\\"3000\\\",\\\"path\\\":\\\"/services/collector/health\\\",\\\"method\\\":\\\"get\\\",\\\"statusCode\\\":404,\\\"response\\\":\\\"<!DOCTYPE html>\\\\n<html lang=\\\\\\\"en\\\\\\\">\\\\n<head>\\\\n<meta charset=\\\\\\\"utf-8\\\\\\\">\\\\n<title>Error</title>\\\\n</head>\\\\n<body>\\\\n<pre>Cannot GET /services/collector/health</pre>\\\\n</body>\\\\n</html>\\\\n\\\",\\\"elapsed\\\":24}\"}",
  "method": "get",
  "url": "http://0.0.0.0:3000/services/collector/health",
  "statusCode": 404,
  "error": "Http Error, statusCode: 404, details: {\"host\":\"0.0.0.0:3000\",\"port\":\"3000\",\"path\":\"/services/collector/health\",\"method\":\"get\",\"statusCode\":404,\"response\":\"<!DOCTYPE html>\\n<html lang=\\\"en\\\">\\n<head>\\n<meta charset=\\\"utf-8\\\">\\n<title>Error</title>\\n</head>\\n<body>\\n<pre>Cannot GET /services/collector/health</pre>\\n</body>\\n</html>\\n\",\"elapsed\":24}",
  "_time": 1660152577.93,
  "cribl_breaker": "Cribl:ndjson"
}
```

### Success Events

The Health Check Collector generates success events when a 200 success status code is received. The following is an example success event:

```json
{
  "host": "Cribl.local",
  "_raw": "{\"url\":\"http://0.0.0.0:3000/api/v1/health\",\"method\":\"get\",\"statusCode\":200,\"body\":\"{\\\"status\\\":\\\"healthy\\\",\\\"startTime\\\":1660152114781}\"}",
  "method": "get",
  "url": "http://0.0.0.0:3000/api/v1/health",
  "statusCode": 200,
  "body": "{\"status\":\"healthy\",\"startTime\":1660152114781}",
  "_time": 1660152114.781,
  "cribl_breaker": "Cribl:ndjson"
}
```

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

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
* `__collectStats` – This object's nested fields are:
    * `method`: The value you set for **Health Check** > **Health check method**.
    * `url`: The value you set for **Health Check** > **Health check URL**.
    * `elapsedMS`: The time elapsed from the moment the Health Check request was sent until the response was received.

## Mitigating Stuck-Job Problems

Occasionally, a scheduled job fails, but continues running for hours or even days, until someone intervenes and cancels it. If left alone, such a "stuck", "orphaned," or "zombie" job will never complete. This can cause missing events in downstream receivers, along with `HTTP timeout` or similar errors in Cribl Stream's logs.

To keep stuck jobs from running excessively long:

* First, try setting **Optional Settings** > **Request Timeout (secs)** – whose default of `0` means "wait forever" – to a desired maximum duration.
* If adjusting **Timeout (secs)** does not fix the problem, try the global setting **Job Timeout** – whose default of `0` allows a job to run indefinitely – to a desired maximum duration. You'll find **Job Timeout** among the [task manifest and buffering limits](collectors-job-limits#task-manifest-and-buffering-limits). 

Using these settings in tandem works like this:

* **Timeout (secs)** limits the time that Cribl Stream will wait for an HTTP request to complete. 
* Then, if a job gets stuck and keeps running beyond that limit, **Job Timeout** can catch and terminate the job, because it monitors the overall time the job has been running.
