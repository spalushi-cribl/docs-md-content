# REST / API Endpoint



Cribl Stream supports collecting data from REST endpoints. This Collector provides multiple Discover types and Collect options. 

> For usage examples, see [Using REST / API Collectors](/stream/usecase-rest) and our adjacent guides.
>
{.box .success}

## Configuring a REST Collector 

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, select **Data** > **Sources**, then select **Collectors** > **REST** from the **Manage Sources** page's tiles or left nav. Click **New Collector** to open the **REST** > **New Collector** modal, which provides the following options and fields.

> The sections described below are spread across several tabs, some of which contain collapsible accordions. Click the tab links at left to navigate among tabs. Click **Save** when you've configured your Collector.
> 
> See [Formatting Expressions](#expressions) below for guidance on this Collector's specific requirement to enter JavaScript expressions as template literals.
> 
> Collector Sources currently cannot be selected or enabled in the QuickConnect UI.
>
{.box .info}

## Collector Settings

The Collector Settings determine how data is collected before processing.

**Collector ID**: Unique ID for this Collector. E.g., `rest42json`.

### Discover Settings {#discover-settings}

Within the **Discover** accordion, the **Discover type** drop-down provides four options, corresponding to different use cases. Each **Discover type** selection will expose a different set of **Collector Settings** fields. Below, we cover the **Discover type**s from simplest to most-complex. 

- **[Discover type: None](#common)** matches cases where one simple API call will retrieve all the data you need. This default option suppresses the Discover stage. Use the modal's **Collect** section to specify the endpoint and other details. Cribl Stream will assign the Collect task to a single Worker. (Example: Collect a list of configured Cribl Stream Pipelines.)

- **[Discover type: Item List](#list)** matches cases where you want to enumerate a known list of **Discover items** to retrieve. (Examples: Collect network traffic data that's tagged with specific subnets; or collect weather data for a known list of ZIP codes.) Discovery will return one Collect task per item in the list, and Cribl Stream will spread each of those Collect tasks across all available Workers.

- **[Discover type: JSON Response](#json)** provides a **Discover result** field where you can (optionally) define Discover tasks as a JSON array of objects. Each entry returned by Discover will generate a Collect task, and Cribl Stream will spread each of those Collect tasks across all available Workers. (Example: Collect data for specific geo locations in the [National Weather Service API](https://www.weather.gov/documentation/services-web-api)'s stream of worldwide weather data. This particular API requires multiple parameters in the request URL – latitude, longitude, etc. – so **Item List** discovery would not work.)

- **[Discover type: HTTP Request](#http)** matches cases where you need to dynamically discover what you can collect from a REST endpoint. This Discover type most fully exploits Cribl Stream's Discover-and-then-Collect architecture. (Example: Make a REST call to get a list of available log files, then run Collect against each of those files.) Each item returned will generate a Collect task, and Cribl Stream will spread each of those Collect tasks across all available Workers. As of Cribl Stream 3.0.2, this Discover type supports XML responses. 

### Collect Settings (Common) {#common}

Within the **Collect** accordion, the following options appear for **Discover type**: `None`, as well as for all other **Discover type** selections: 

**Collect URL**: URL (constant or JavaScript expression) to use for the Collect operation.

> Where a URL (path or parameters) includes variables that might contain unsafe ASCII characters, encode these variables using `C.Encode.uri(paramName)`. (Examples of unsafe characters are: space, `$`, `/`, `=`.) Example URL with encoding: ``'http://localhost:9000/api/v1/system/logs/' + C.Encode.uri(`${id}`)``. 
> 
> Request parameters are not contained directly in the URL are automatically encoded. Cribl Stream URLs/expressions specified in the **Collect URL** field follow redirects.
>
{.box .info}

**Collect method**: Select the HTTP verb to use for the Collect operation – `GET`, `POST`, `POST with body`, or `Other`.

**Collect POST body**: Template for POST body to send with the Collect request. (This field is displayed only when you set the **Collect method** to `POST with body`.) You can reference parameters from the Discover response using template params of the form: `` `${variable}` ``.

**Collect verb**: Custom [HTTP method](https://www.iana.org/assignments/http-methods/http-methods.xhtml) to use for the Collect operation. (This field is displayed only when you set the **Collect method** to `Other`.)

**Collect parameters**: Optional HTTP request parameters to append to the request URL. These refine or narrow the request. Click **+ Add Parameter** to add parameters as key-value pairs:

  * **Name**: Field name.
  * **Value**: JavaScript expression to compute the field's value, normally enclosed in backticks (e.g., `` `${earliest}` ``). Can also be a constant, enclosed in single quotes (`'earliest'`). Values without delimiters (e.g., `earliest`) are evaluated as strings.

**Collect headers**:: Click **+ Add Header** to (optionally) add collection request headers as key-value pairs:

  * **Name**: Header name.
  * **Value**: JavaScript expression to compute the header's value, normally enclosed in backticks (e.g., `` `${earliest}` ``). Can also be a constant, enclosed in single quotes (`'earliest'`). Values without delimiters (e.g., `earliest`) are evaluated as strings.

> By adding the appropriate **Collect headers**, you can specify authentication based on API keys, as an alternative to the **Authentication**: `Basic` or `Login` options below.
>
{.box .success}

**Pagination**: For the options exposed by this drop-down list, see the [Pagination](#pagination) section below.

**Authentication**: For the options within this accordion, see the [Authentication Settings](#authentication) section below.

> ##### Time Range Variables {{< id `earliest-latest` >}}
>
> The following fields accept `` `${earliest}` `` and `` `${latest}` `` variables, which reference any **Time Range** values that have been set in manual or scheduled [collection jobs](collectors-schedule-run#time-range): 
> - **Collect URL**, **Collect parameters**, **Collect headers**
> - **Discover URL**, **Discover parameters**, **Discover headers**. 
> 
> As an example, here is a **Collect URL** entry using these variables:  
> `http://localhost/path?from=${earliest}&to=${latest}`
> 
> Both variables are formatted as UNIX epoch time, in seconds units. When using them in contexts that require milliseconds resolution, multiply them by 1,000 to convert to ms.
>
{.box .info}

### Pagination {#pagination}

Use this drop-down list to select the pagination scheme for collection results. Defaults to `None`. The other options, expanded below, are:

- [Response Body Attribute / Response Header Attribute](#response-attribute)
- [RFC 5988 – Web Linking](#rfc-5988)
- [Offset/Limit](#offset-limit)
- [Page/Size](#page-size)

#### Response Body Attribute / Response Header Attribute  {#response-attribute}

Select `Response Body Attribute` to extract a value from the response body that identifies the next page of data to retrieve. 

Select `Response Header Attribute` to extract this next-page value from the response header. Either of these selections exposes these additional fields:

 - **Response attribute**: Name(s) of attribute(s) in the response payload or header that contain next-page information. If you have multiple attributes with the same name within the response body, you can provide the path to the one you need, e.g.: `foo.0.bar`. If Cribl Stream cannot find an exact match for a specified attribute name, it will fallback to the last-found partial match.

 - **Max pages**: The maximum number of pages to retrieve per Collect task. Defaults to `50` pages. Set to `0` to retrieve all pages.

Selecting `Response Body Attribute` exposes one more field:
- **Last-page expression**: Optionally, enter a JavaScript expression that Cribl Stream will use to determine when the last page has been reached. The values tested by this expression must be present in the **Response attributes** section above. Example expression: `morePages === false`.

> Use this expression with APIs like [Smartsheet Event Reporting](https://smartsheet-platform.github.io/event-reporting-docs/), which return an attribute that signals whether more pages are present, rather than simply returning an empty result set beyond the last page.
>
{.box .success}

####  RFC 5988 – Web Linking  {#rfc-5988}

Select this option with APIs that follow [RFC 5988](https://tools.ietf.org/html/rfc5988) conventions to provide the next-page link in a header. This selection exposes three additional fields:

**Next page relation name**: Header substring that refers to the next page in the result set. Defaults to `next`, corresponding to the following example link header:  
`<https://myHost/curPage>; rel="self" <https://myHost/nextPage>; rel="next"`

**Current page relation name**: Optionally, specify the relation name within the link header that refers to the current result set. In this same example, `rel="self"` refers to the current page of results:  
`<https://myHost/curPage>; rel="self" <https://myHost/nextPage>; rel="next"`

**Max pages**: The maximum number of pages to retrieve. Defaults to `50` pages. Set to `0` to retrieve all pages.

####  Offset/Limit  {#offset-limit}

Select this option to receive data from APIs that use offset/limit pagination. This selection exposes several additional fields:

**Offset field name**: Query string parameter that sets the index from which to begin returning records. E.g.: `/api/v1/query?term=cribl&limit=100&offset=0`. Defaults to `offset`.

**Starting offset**: (Optional) offset index from which to start request. Defaults to undefined, which will start collection from the first record.

**Limit field name**: Query string parameter to set the number of records retrieved per request. E.g.:  
`/api/v1/query?term=cribl&limit=100&offset=0`. Defaults to `limit`.

**Limit**: Maximum number of records to collect per request. Defaults to `50`.

**Total record count field name**: Identifies the attribute, within the response, that contains the total number of records for the query.

**Max pages**: The maximum number of pages to retrieve. Defaults to `50`. Set to `0` to retrieve all pages. 

**Zero-based index**: Toggle to `Yes` to indicate that the requested data's first record is at index `0`. The default (No) indicates that the first record is at index `1`.

####  Page/Size  {#page-size}

Select this option to receive data from APIs that use page/size pagination. This selection exposes several additional fields:

**Page number field name**: Query string parameter that sets the page index to be returned. E.g.:  
`/api/v1/query?term=cribl&page_size=100&page_number=0`. Defaults to `page`.

**Starting page number**: (Optional) page number from which to start request. Defaults to undefined, which will start collection from the first page.

**Page size field name**: Query string parameter to set the number of records retrieved per request. E.g.:  
`/api/v1/query?term=cribl&page_size=100&page_number=0`. Defaults to `size`.

**Page size**: Maximum number of records to collect per page. Defaults to `50`.

**Total page count field name**: Identifies the attribute, within the response, that contains the total number of pages for the query.

**Total record count field name**: Identifies the attribute, within the response, that contains the total number of records for the query.

**Max pages**: The maximum number of pages to retrieve. Defaults to `50`. Set to `0` to retrieve all pages. 

**Zero-based index**: Toggle to `Yes` to indicate that the requested data's first record is at index `0`. The default (No) indicates that the first record is at index `1`.

### Authentication Settings {#authentication}

In the **Authentication** accordion, use the buttons to select one of these options:

- **None**: Don't use authentication. Compatible with REST servers like AWS, where you embed a secret directly in the request URL.

- **Basic**: Displays **Username** and **Password** fields for you to enter HTTP Basic authentication credentials.

- **Basic (credentials secret)**: Provide username and password credentials referenced by a secret. Select a stored text secret in the resulting **Credentials secret** drop-down, or click **Create** to configure a new secret.

- **Login**: Enables you to specify several credentials, then perform a POST to an endpoint during the Discover operation. The POST response returns a token, which Cribl Stream uses for later Collect operations. Exposes multiple extra fields – see  [Login Authentication](#login-auth) below.

- **Login (credentials secret)**: Like **Login**, except that you specify a secret referencing the credentials, rather than the credentials themselves. Exposes multiple extra fields – see  [Login Secret Authentication](#login-secret-auth) below.

- **OAuth**: Enables you to directly enter credentials for authentication via the OAuth protocol. Exposes multiple extra fields – see [OAuth Authentication](#oauth-auth) below.

- **OAuth (text secret)**: Like the **OAuth** option, except that you specify a stored secret referencing the credentials. Exposes multiple extra fields – see [OAuth Secret Authentication](#oauth-secret-auth) below.

#### Login Authentication {#login-auth}

Selecting **Login** exposes the following additional fields:

- **Login URL**: URL for the login API call, which is expected to be a POST call.

- **Username**: Login username.

- **Password**: Login password.

- **POST body**: Template for POST body to send with the login request. The `${username}` and `${password}` variables specify the corresponding credentials' locations in the message. You can also use the `C.Secret()` method within the POST body for dynamic values.

- **Authorization header**: Authorization header key to pass in Discover and Collect calls. Defaults to the literal name `Authorization`. (Cribl Stream 4.0 and later.)

- **Authorize expression**: JavaScript expression used to compute an Authorization header to pass in Discover and Collect calls. Uses `${token}` to reference the token obtained from the login POST request.

- **Authentication headers**: Optionally, click **+ Add Header** for each custom auth header you want to define. In the resulting table, enter each header row's **Name**. 

  The corresponding **Value** is a JavaScript expression to compute the header's value. This can also evaluate to a constant. Values not formatted as expressions – e.g., `earliest` instead of `` `${earliest}` `` – will be evaluated as strings.

#### Login Secret Authentication {#login-secret-auth}

Selecting **Login (credentials secret)** exposes the following additional fields:

- **Login URL**: URL for the login API call, which is expected to be a POST call.

- **Credentials secret**: Select a stored text secret in this drop-down, or click **Create** to configure a new secret.

- **POST body**: Template for POST body to send with the login request. The `${username}` and `${password}` variables specify the corresponding credentials' locations in the message. You can also use the `C.Secret()` method within the POST body for dynamic values.

- **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes.

- **Authorization header**: Authorization header key to pass in Discover and Collect calls. Defaults to the literal name `Authorization`. (Cribl Stream 4.0 and later.)

- **Authorize expression**: JavaScript expression used to compute an Authorization header to pass in Discover and Collect calls. Uses `${token}` to reference the token obtained from the login POST request.

- **Authentication headers**: Optionally, click **+ Add Header** for each custom auth header you want to define. In the resulting table, enter each header row's **Name**. 

  The corresponding **Value** is a JavaScript expression to compute the header's value. This can also evaluate to a constant. Values not formatted as expressions – e.g., `earliest` instead of `` `${earliest}` `` – will be evaluated as strings.

#### OAuth Authentication {#oauth-auth}

Selecting **OAuth** exposes the following additional fields:

- **Login URL**: Endpoint for the OAuth API call, which is expected to be a POST call.

- **Client secret parameter**: The name of the parameter to send with the **Client secret value**.

- **Client secret value**: The OAuth access token to authorize requests.

- **Extra authentication parameters**: Optionally, click **+ Add parameter** for each additional OAuth request parameter you want to send in the body of POST requests. Automatically sets the `Content‑Type` header to `application/x‑www‑form‑urlencoded`. 

  In the resulting table, enter each parameter row's **Name**. The corresponding **Value** is a JavaScript expression to compute the parameter's value. This can also evaluate to a constant. Values not formatted as expressions – e.g., `earliest` instead of `` `${earliest}` `` – will be evaluated as strings.

- **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes.

- **Authorization header**: Authorization header key to pass in Discover and Collect calls. Defaults to the literal name `Authorization`. (Cribl Stream 4.0 and later.)

- **Authorize expression**: JavaScript expression used to compute an Authorization header to pass in Discover and Collect calls. Uses `${token}` to reference the token obtained from the login POST request.

- **Authentication headers**: Optionally, click **+ Add Header** for each custom auth header you want to define. In the resulting table, enter each header row's **Name**. 

  The corresponding **Value** is a JavaScript expression to compute the header's value. This can also evaluate to a constant. Values not formatted as expressions – e.g., `earliest` instead of `` `${earliest}` `` – will be evaluated as strings.

#### OAuth Secret Authentication {#oauth-secret-auth}

Selecting **OAuth (text secret)** exposes the following additional fields:

- **Login URL**: Endpoint for the OAuth API call, which is expected to be a POST call.

- **Client secret parameter**: The name of the parameter to send with the **Client secret value**.

- **Client secret value (text secret)**: Select a stored text secret in this drop-down, or click **Create** to configure a new secret.

- **Extra authentication parameters**: Optionally, click **+ Add parameter** for each additional OAuth request parameter you want to send in the body of POST requests. Automatically sets the `Content‑Type` header to `application/x‑www‑form‑urlencoded`. 

  In the resulting table, enter each parameter row's **Name**. The corresponding **Value** is a JavaScript expression to compute the parameter's value. This can also evaluate to a constant. Values not formatted as expressions – e.g., `earliest` instead of `` `${earliest}` `` – will be evaluated as strings.

- **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes.

- **Authorization header**: Authorization header key to pass in Discover and Collect calls. Defaults to the literal name `Authorization`. (Cribl Stream 4.0 and later.)

- **Authorize expression**: JavaScript expression used to compute an Authorization header to pass in Discover and Collect calls. Uses `${token}` to reference the token obtained from the login POST request.

- **Authentication headers**: Optionally, click **+ Add Header** for each custom auth header you want to define. In the resulting table, enter each header row's **Name**. 

  The corresponding **Value** is a JavaScript expression to compute the header's value. This can also evaluate to a constant. Values not formatted as expressions – e.g., `earliest` instead of `` `${earliest}` `` – will be evaluated as strings.

### Optional Settings {#optional}

**Request Timeout (secs)**: Here, you can set a maximum time period (in seconds) for an HTTP request to complete before Cribl Stream treats it as timed out. Defaults to `0`, which disables timeout metering.

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Stream will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.

**Disable time filter**: Toggle to `Yes` if your Run or Schedule configuration specifies a date range and no events are being collected. This will disable the Collector's event time filtering to prevent timestamp conflicts.

**Safe headers**: Optionally, list headers that you consider safe to log in plain text. Separate the header names with tabs or hard returns.

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Stream. Use a tab or hard return between (arbitrary) tag names.

### Settings By Discover Type {#settings-by-discover-type}

For [Discover types](#discover-settings) other than `None`, Cribl Stream exposes additional fields specific to the selected Discover type.

#### Discover Type: Item List {#list}

Setting the **Discover type** to `Item List` exposes this additional field above the [Common Collector Settings](#common):

**Discover items**: List of items to return from the Discover task. Each returned item will generate a Collect task, and can be referenced using `${id}` in the **Collect URL**, the **Collect parameters**, or the **Collect headers**.


#### Discover Type: JSON Response {#json}

Setting the **Discover type** to `JSON Response` exposes these additional fields above the [Common Collector Settings](#common):

**Discover result**: Allows hard-coding the Discover result. Must be a JSON object. Works with the Discover data field.

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

Setting the **Discover type** to `HTTP Request` exposes these additional fields above the [Common Collector Settings](#common):

**Discover URL**: Enter the URL to use for the Discover operation. This can be a constant URL, or a JavaScript expression to derive the URL.

> Where a URL (path or parameters) includes variables that might contain unsafe ASCII characters, encode these variables using `C.Encode.uri(paramName)`. (Examples of unsafe characters are: space, `$`, `/`, `=`.) Example URL with encoding:  
> ``'http://localhost:9000/api/v1/system/logs/' + C.Encode.uri(`${id}`)``. 
> 
> Request parameters are not contained directly in the URL are automatically encoded. Cribl Stream URLs/expressions specified in the **Discover URL** field follow redirects.
>
{.box .info}

**Discover method**: Select the HTTP verb to use for the Discover operation – `GET`, `POST`, `POST with body`, or `Other`.

**Discover POST body**: Template for POST body to send with the Discover request. (This field is displayed only when you set the **Discover method** to `POST with body`.)

**Discover verb**: Custom [HTTP method](https://www.iana.org/assignments/http-methods/http-methods.xhtml) to use for the Discover operation. (This field is displayed only when you set the **Discover method** to `Other`.)

**Discover parameters**:  Optional HTTP request parameters to append to the Discover request URL. These refine or narrow the request. Click **+ Add Parameter** to add parameters as key-value pairs:

  * **Name**: Parameter name.
  * **Value**: JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g., `` `${earliest}` ``). Can also be a constant, enclosed in single quotes (`'earliest'`). Values without delimiters (e.g., `earliest`) are evaluated as strings.

**Discover headers**: Optional Discover request headers.: Click **+ Add Header** to add headers as key-value pairs:

  * **Name**: Header name.
  * **Value**: JavaScript expression to compute the header's value, normally enclosed in backticks (e.g., `` `${earliest}` ``). Can also be a constant, enclosed in single quotes (`'earliest'`). Values without delimiters (e.g., `earliest`) are evaluated as strings.

**Discover data field**: Within the response JSON, name of the field  that contains Discover results. Leave blank if the result is an array.

> The following sections describe the Collector Settings' remaining tabs, whose settings and content apply equally to all **Discover type** selections.
>
{.box .success}

## Result Settings

The Result Settings determine how Cribl Stream transforms and routes the collected data. 

#### Custom Command

In this section, you can pass the data from this input to an external command for processing, before the data continues downstream.

**Enabled**: Defaults to `No`. Toggle to `Yes` to enable the custom command.

**Command**: Enter the command that will consume the data (via `stdin`) and will process its output (via `stdout`).

**Arguments**: Click **+ Add Argument** to add each argument to the command. You can drag arguments vertically to resequence them.

#### Event Breakers

In this section, you can apply event breaking rules to convert data streams to discrete events.

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied, in order, to the input data stream. Defaults to `System Default Rule`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute the field's value (can be a constant).

#### Result Routing

**Send to Routes**: If set to `Yes` (the default), Cribl Stream will send events to normal routing and event processing. Toggle to `No` to select a specific Pipeline/Destination combination. The `No` setting exposes these two additional fields:
- **Pipeline**:  Select a Pipeline to process results.
- **Destination**: Select a Destination to receive results.

The default `Yes` setting instead exposes this field: 
- **Pre-processing Pipeline**: Pipeline to process results before sending to Routes. Optional.

This field is always exposed:
- **Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, `GB`, etc. (Example: `42 MB`.) Default value of `0` indicates no throttling.

> You might disable **Send to Routes** when configuring a Collector that will connect data from a specific Source to a specific Pipeline and Destination. One use case might be a REST Collector that gathers a known, simple type of data from a single endpoint. This approach keeps the Collector's configuration self‑contained and separate from Cribl Stream's routing table for live data – potentially simplifying the Routes structure.
>
{.box .info}

**Pre-processing Pipeline**: Pipeline to process results before sending to Routes. Optional, and available only when **Send to Routes** is toggled to `Yes`.

**Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, `GB`, etc. (Example: `42 MB`.) Default value of `0` indicates no throttling.

## Advanced Settings

Advanced Settings enable you to customize post-processing and administrative options.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Remove Discover fields** : List of fields to remove from the Discover results. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface. You can specify wildcards (such as `aws*`).

**Resume job on boot**: Toggle to `Yes` to resume ad hoc collection jobs if Cribl Stream restarts during the jobs' execution.

## Formatting Expressions {#expressions}

JavaScript expression fields in this REST/API Collector behave differently from those elsewhere in Cribl Stream. Here, you must enter expressions as template literals, with placeholders referencing variables and JS functions. Here are a few examples: 
- To reference the earliest time variable in a URL, header, or parameter (etc.), you would write the variable as `` `${earliest}` ``. 
- To reference a function call: `` `${Date.now()}` ``. 
- To call a function that references a variable: `` `${Math.floor(earliest)}` ``.

## Response Errors

The Collector treats all non-200 responses from configured URL endpoints as errors. This includes 1xx, 3xx, 4xx, and 5xx responses.

On Discover, Preview, and most Collect jobs, it interprets these as fatal errors. On Collect jobs, a few exceptions are treated as non-fatal:

- Where a Collect job launches multiple tasks, and only a subset of those tasks fail, Cribl Stream places the job in failed status, but treats the error as non-fatal. (Note that Cribl Stream does not retry the failed tasks.)

- Where a Collect job receives a `3xx` redirection error code, it follows the error's treatment by the underlying library, and does not necessarily treat the error as fatal.


