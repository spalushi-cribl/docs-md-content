# REST / API Endpoint Collector



Cribl Stream supports collecting data from REST endpoints. This Collector provides multiple Discover types and Collect options. 

> For usage examples, see [Using REST / API Collectors](/stream/usecase-rest) and our adjacent guides.
>
{.box .success}

## Configuring a REST Collector 

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, select **Data** > **Sources**, then select **Collectors** > **REST** from the **Manage Sources** page's tiles or left nav. Click **Add Collector** to open the **REST** > **New Collector** modal, which provides the following options and fields.

Alternatively, you can create a REST Collector by [importing its configuration](collectors#creating-from-template) and entering desired values for any placeholders.

Whichever method you use, click **Save** once you have configured your Collector.

> The sections described below are spread across several tabs, some of which contain collapsible accordions. Click the tab links at left to navigate among tabs.
> 
> See [Formatting Expressions](#expressions), below, for guidance on this Collector's specific requirement to enter JavaScript expressions as template literals.
> 
> Collector Sources currently cannot be selected or enabled in the QuickConnect UI.
>
{.box .info}

## Collector Settings

The Collector Settings determine how data is collected before processing.

**Collector ID**: Unique ID for this Collector. For example, `rest42json`. If you clone this Collector, Cribl Stream will add `-CLONE` to the original **Collector ID**.

### Discover Settings {#discover-settings}

Within the **Discover** accordion, the **Discover type** drop-down provides four options, corresponding to different use cases. Each **Discover type** selection will expose a different set of **Collector Settings** fields. Below, we cover the **Discover type**s from simplest to most-complex. 

- **[Discover type: None](#common)** matches cases where one simple API call will retrieve all the data you need. This default option suppresses the Discover stage. Use the modal's **Collect** section to specify the endpoint and other details. Cribl Stream will assign the Collect task to a single Worker. (Example: Collect a list of configured Cribl Stream Pipelines.)

- **[Discover type: Item List](#list)** matches cases where you want to enumerate a known list of **Discover items** to retrieve. (Examples: Collect network traffic data that's tagged with specific subnets; or collect weather data for a known list of ZIP codes.) Discovery will return one Collect task per item in the list, and Cribl Stream will spread each of those Collect tasks across all available Workers.

- **[Discover type: JSON Response](#json)** provides a **Discover result** field where you can (optionally) define Discover tasks as a JSON array of objects. Each entry returned by Discover will generate a Collect task, and Cribl Stream will spread each of those Collect tasks across all available Workers. (Example: Collect data for specific geo locations in the [National Weather Service API](https://www.weather.gov/documentation/services-web-api)'s stream of worldwide weather data. This particular API requires multiple parameters in the request URL – latitude, longitude, and so forth – so **Item List** discovery would not work.)

- **[Discover type: HTTP Request](#http)** matches cases where you need to dynamically discover what you can collect from a REST endpoint. This Discover type most fully exploits Cribl Stream's Discover-and-then-Collect architecture. (Example: Make a REST call to get a list of available log files, then run Collect against each of those files.) Each item returned will generate a Collect task, and Cribl Stream will spread each of those Collect tasks across all available Workers. As of Cribl Stream 3.0.2, this Discover type supports XML responses. 

### Collect Settings (Common) {#common}

One source of insight about Collect requests and the behavior of the endpoints where they are sent is the [internal fields](#internal-fields) that the Collector emits. For example, to observe how slowly or quickly an endpoint is responding, you can monitor the value of the `elapsedMS` element nested in the `__collectStats` internal field.

Within the **Collect** accordion, the following options appear for **Discover type**: `None`, as well as for all other **Discover type** selections: 

{{<id collect-url >}}**Collect URL**: URL (constant or JavaScript expression) to use for the Collect operation. This URL can use an `authToken` variable to access an auth token. For example, if `1234567890` is defined as an auth token and you use `http://myurl.com?authToken=${authToken}` as the Collect URL, the Collect operation will use `http://myurl.com?authToken=1234567890`.

> Where a URL (path or parameters) includes variables that might contain unsafe ASCII characters, encode these variables using `C.Encode.uri(paramName)`. (Examples of unsafe characters are: space, `$`, `/`, `=`.) Example URL with encoding: ``'http://localhost:9000/api/v1/system/logs/' + C.Encode.uri(`${id}`)``. 
> 
> Request parameters are not contained directly in the URL are automatically encoded. Cribl Stream URLs/expressions specified in the **Collect URL** field follow redirects.
>
{.box .info}

**Collect method**: Select the HTTP verb to use for the Collect operation – `GET`, `POST`, `POST with body`, or `Other`.

{{<id collect-post-body >}}**Collect POST body**: Template for POST body to send with the Collect request. (This field is displayed only when you set the **Collect method** to `POST with body`.) You can reference parameters from the Discover response using template literals of the form: `` `${variable}` ``. This field can reference the auth token variable obtained during authentication, using the variable `authToken`. For example: `` `{ "token": ${authToken}, "param1": "value1" }` ``.

**Collect verb**: Custom [HTTP method](https://www.iana.org/assignments/http-methods/http-methods.xhtml) to use for the Collect operation. (This field is displayed only when you set the **Collect method** to `Other`.)

**Collect parameters**: Optional HTTP request parameters to append to the request URL. These refine or narrow the request. Click **Add Parameter** to add parameters as key-value pairs:

  * **Name**: Field name.
  * **Value**: JavaScript expression to compute the field's value, normally enclosed in backticks (for example, `` `${earliest}` ``). Can also be a constant, enclosed in single quotes (`'earliest'`). Values without delimiters (for example, `earliest`) are evaluated as strings.
  > If both the **Collect URL** and **Collect parameters** use the same **Name** (key), the value from the **Collect URL** takes precedence.
  {.box .warning}

{{<id collect-header-value >}}**Collect headers**: Click **Add Header** to (optionally) add collection request headers as key-value pairs:

  * **Name**: Header name.
  * **Value**: JavaScript expression to compute the header’s value, normally enclosed in single quotes (`'someConstantValue'`). Can also be a template literal, enclosed in backticks (for example, `` `${new Date().getFullYear()}` ``). Values without delimiters (for example, `someConstantValue`) are evaluated as strings.

> By adding the appropriate **Collect headers**, you can specify authentication
> based on API keys using [secrets](#collect-headers-auth) as an alternative to the **Authentication**:
> `Basic`, `Login`, and `OAuth` options below.
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

 - **Response attribute**: Name(s) of attribute(s) in the response payload or header that contain next-page information. If you have multiple attributes with the same name within the response body, you can provide the path to the one you need, for example: `foo.0.bar`. If Cribl Stream cannot find an exact match for a specified attribute name, it will fallback to the last-found partial match.

 - **Page limit**: The maximum number of pages to retrieve per Collect task. Defaults to `50` pages. Set to `0` to retrieve all pages.

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

**Page limit**: The maximum number of pages to retrieve. Defaults to `50` pages. Set to `0` to retrieve all pages.

####  Offset/Limit  {#offset-limit}

Select this option to receive data from APIs that use offset/limit pagination. This selection exposes several additional fields:

**Offset field name**: Query string parameter that sets the index from which to begin returning records. For example: `/api/v1/query?term=cribl&limit=100&offset=0`. Defaults to `offset`.

**Starting offset**: (Optional) offset index from which to start request. Defaults to undefined, which will start collection from the first record.

**Limit field name**: Query string parameter to set the number of records retrieved per request. For example:  
`/api/v1/query?term=cribl&limit=100&offset=0`. Defaults to `limit`.

**Record limit**: Maximum number of records to collect per request. Defaults to `50`.

**Total record count field name**: Identifies the attribute, within the response, that contains the total number of records for the query.

**Page limit**: The maximum number of pages to retrieve. Defaults to `50`. Set to `0` to retrieve all pages. 

**Zero-based index**: Toggle to `Yes` to indicate that the requested data's first record is at index `0`. The default (No) indicates that the first record is at index `1`.

####  Page/Size  {#page-size}

Select this option to receive data from APIs that use page/size pagination. This selection exposes several additional fields:

**Page number field name**: Query string parameter that sets the page index to be returned. For example:  
`/api/v1/query?term=cribl&page_size=100&page_number=0`. Defaults to `page`.

**Starting page number**: (Optional) page number from which to start request. Defaults to undefined, which will start collection from the first page.

**Page size field name**: Query string parameter to set the number of records retrieved per request. For example:  
`/api/v1/query?term=cribl&page_size=100&page_number=0`. Defaults to `size`.

**Record limit**: Maximum number of records to collect per page. Defaults to `50`.

**Total page count field name**: Identifies the attribute, within the response, that contains the total number of pages for the query.

**Total record count field name**: Identifies the attribute, within the response, that contains the total number of records for the query.

**Page limit**: The maximum number of pages to retrieve. Defaults to `50`. Set to `0` to retrieve all pages. 

**Zero-based index**: Toggle to `Yes` to indicate that the requested data's first record is at index `0`. The default (No) indicates that the first record is at index `1`.

### Authentication Settings {#authentication}

In the **Authentication** accordion, use the **Authentication** drop-down to select one of these options:

- **None**: Don't use authentication. Compatible with REST servers like AWS, where you embed a secret directly in the request URL.

- **Basic**: Displays **Username** and **Password** fields for you to enter HTTP Basic authentication credentials.

- **Basic (credentials secret)**: Provide username and password credentials referenced by a secret. Select a stored text secret in the resulting **Credentials secret** drop-down, or click **Create** to configure a new secret.

- **Login**: Enables you to specify several credentials, then perform a POST to an endpoint during the Discover operation. The POST response returns a token, which Cribl Stream uses for later Collect operations. Exposes multiple extra fields – see  [Login Authentication](#login-auth) below.

- **Login (credentials secret)**: Like **Login**, except that you specify a secret referencing the credentials, rather than the credentials themselves. Exposes multiple extra fields – see  [Login Secret Authentication](#login-secret-auth) below.

- **OAuth**: Enables you to directly enter credentials for authentication via the OAuth protocol. Exposes multiple extra fields – see [OAuth Authentication](#oauth-auth) below.

- **OAuth (text secret)**: Like the **OAuth** option, except that you specify a stored secret referencing the credentials. Exposes multiple extra fields – see [OAuth Secret Authentication](#oauth-secret-auth) below.

> {{<id collect-headers-auth >}} You can also use [**Collect Headers**](#collect-header-value) as an alternative way of authenticating a request.
> Select `None` in the **Authentication** drop-down
> and use the [`C.Secret`](expressions-other#csecret) expression method to provide the value for the `Authentication` header.
> For example, depending on the requirements of your application, you could use:
> 
> `` `Bearer: ${C.Secret('<secretId>', 'text').value}` ``
>
> or:
> 
> `` `Token: ${C.Secret('<secretId>', 'text').value}` ``
{.box .success}

#### Login Authentication {#login-auth}

**Login** is the best authentication type to use for all services that return a JSON web token (JWT) or other authentication token whose content type is `application/json` or `content/plain`.

Selecting **Login** exposes the following additional fields:

- **Login URL**: URL for the login API call, which is expected to be a POST call.

- **Login username**: Login username.

- **Login password**: Login password.

- **POST body**: Template for POST body to send with the login request. The `${username}` and `${password}` variables specify the corresponding credentials' locations in the message. You can also use the [`C.Secret()` method](expressions-other#csecret) within the POST body for dynamic values.

- **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes. If the response from the auth request provides the JWT as `plain/text`, leave this field blank.

- **Authorization header**: Authorization header key to pass in Discover and Collect calls. Defaults to the literal name `Authorization`. (Cribl Stream 4.0 and later.)

- **Authorize expression**: JavaScript expression used to compute an Authorization header to pass in Discover and Collect calls. Uses `${token}` to reference the token obtained from a Login or OAuth authorization request. If you need to pass the token in a Collector or Discover request parameter, URL, or request body, you can access it with `${authToken}`.

- **Authentication headers**: Optionally, click **Add Header** for each custom auth header you want to define. In the resulting table, enter each header row's **Name**. 

  The corresponding **Value** is a JavaScript expression to compute the header's value. This can also evaluate to a constant. Values not formatted as expressions – for example, `earliest` instead of `` `${earliest}` `` – will be evaluated as strings.

#### Login Secret Authentication {#login-secret-auth}

**Login** is the best authentication type to use for all services that return a JSON web token (JWT) or other authentication token whose content type is `application/json` or `content/plain`.

Selecting **Login (credentials secret)** exposes the following additional fields:

- **Login URL**: URL for the login API call, which is expected to be a POST call.

- **Credentials secret**: Select a stored text secret in this drop-down, or click **Create** to configure a new secret.

- **POST body**: Template for POST body to send with the login request. The `${username}` and `${password}` variables specify the corresponding credentials' locations in the message. You can also use the [`C.Secret()` method](expressions-other#csecret) within the POST body for dynamic values.

- **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes. If the response from the auth request provides the JWT as `plain/text`, leave this field blank.

- **Authorization header**: Authorization header key to pass in Discover and Collect calls. Defaults to the literal name `Authorization`. (Cribl Stream 4.0 and later.)

- **Authorize expression**: JavaScript expression used to compute an Authorization header to pass in Discover and Collect calls. Uses `${token}` to reference the token obtained from a Login or OAuth authorization request. If you need to pass the token in a Collector or Discover request parameter, URL, or request body, you can access it with `${authToken}`.

- **Authentication headers**: Optionally, click **Add Header** for each custom auth header you want to define. In the resulting table, enter each header row's **Name**. 

  The corresponding **Value** is a JavaScript expression to compute the header's value. This can also evaluate to a constant. Values not formatted as expressions – for example, `earliest` instead of `` `${earliest}` `` – will be evaluated as strings.

#### OAuth Authentication {#oauth-auth}

Selecting **OAuth** exposes the following additional fields:

- **Login URL**: Endpoint for the OAuth API call, which is expected to be a POST call.

- **Client secret parameter**: The name of the parameter to send with the **Client secret value**.

- **Client secret value**: The OAuth access token to authorize requests.

- **Extra authentication parameters**: Optionally, click **Add parameter** for each additional OAuth request parameter you want to send in the body of POST requests. Automatically sets the `Content‑Type` header to `application/x‑www‑form‑urlencoded`. 

  In the resulting table, enter each parameter row's **Name**. The corresponding **Value** is a JavaScript expression to compute the parameter's value. This can also evaluate to a constant. Values not formatted as expressions – for example, `earliest` instead of `` `${earliest}` `` – will be evaluated as strings.

- **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes.

- **Authorization header**: Authorization header key to pass in Discover and Collect calls. Defaults to the literal name `Authorization`. (Cribl Stream 4.0 and later.)

- **Authorize expression**: JavaScript expression used to compute an Authorization header to pass in Discover and Collect calls. Uses `${token}` to reference the token obtained from a Login or OAuth authorization request. If you need to pass the token in a Collector or Discover request parameter, URL, or request body, you can access it with `${authToken}`.

- **Authentication headers**: Optionally, click **Add Header** for each custom auth header you want to define. In the resulting table, enter each header row's **Name**. 

  The corresponding **Value** is a JavaScript expression to compute the header's value. This can also evaluate to a constant. Values not formatted as expressions – for example, `earliest` instead of `` `${earliest}` `` – will be evaluated as strings.

#### OAuth Secret Authentication {#oauth-secret-auth}

Selecting **OAuth (text secret)** exposes the following additional fields:

- **Login URL**: Endpoint for the OAuth API call, which is expected to be a POST call.

- **Client secret parameter**: The name of the parameter to send with the **Client secret value**.

- **Client secret value (text secret)**: Select a stored text secret in this drop-down, or click **Create** to configure a new secret.

- **Extra authentication parameters**: Optionally, click **Add parameter** for each additional OAuth request parameter you want to send in the body of POST requests. Automatically sets the `Content‑Type` header to `application/x‑www‑form‑urlencoded`. 

  In the resulting table, enter each parameter row's **Name**. The corresponding **Value** is a JavaScript expression to compute the parameter's value. This can also evaluate to a constant. Values not formatted as expressions – for example, `earliest` instead of `` `${earliest}` `` – will be evaluated as strings.

- **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes.

- **Authorization header**: Authorization header key to pass in Discover and Collect calls. Defaults to the literal name `Authorization`. (Cribl Stream 4.0 and later.)

- **Authorize expression**: JavaScript expression used to compute an Authorization header to pass in Discover and Collect calls. Uses `${token}` to reference the token obtained from a Login or OAuth authorization request. If you need to pass the token in a Collector or Discover request parameter, URL, or request body, you can access it with `${authToken}`.

- **Authentication headers**: Optionally, click **Add Header** for each custom auth header you want to define. In the resulting table, enter each header row's **Name**. 

  The corresponding **Value** is a JavaScript expression to compute the header's value. This can also evaluate to a constant. Values not formatted as expressions – for example, `earliest` instead of `` `${earliest}` `` – will be evaluated as strings.

  #### Google Service Account OAuth Authentication {#gsa-oauth}

- **Scopes**: OAuth 2.0 scopes used to request access to Google APIs during authentication. See the Google page [OAuth 2.0 Scopes](https://developers.google.com/identity/protocols/oauth2/scopes) for more information. Click **Add Scope** to add a scope.

- **Service account credentials**: Contents of the Google Cloud service account credentials (JSON keys) file. To upload a file, click the upload icon in this field's upper right corner.

- **Impersonated account's email address**: Email address of a user account with super admin permissions to the resources the Collector will retrieve.

  #### Google Service Account OAuth Secret Authentication {#gsa-oauth-secret}

- **Scopes**: OAuth 2.0 scopes used to request access to Google APIs during authentication. See the Google page [OAuth 2.0 Scopes](https://developers.google.com/identity/protocols/oauth2/scopes) for more information. Click **Add Scope** to add a scope.

- **Service account credentials (text secret)**: Text secret containing the value of the Google service account credentials. Select an existing text secret or click **Create** to create a new secret.

- **Impersonated account's email address**: Email address of a user account with super admin permissions to the resources the Collector will retrieve.

### Retries {#retries}

**Retry type**: The algorithm to use when performing HTTP retries. Options include `Backoff` (the default), `Static`, and `Disabled`.

**Initial retry interval (ms)**: Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute). A value of `0` means retry immediately until reaching the retry limit in **Max retries**. 

**Max retries**: Maximum number of times to retry a failed HTTP request. Defaults to `5`. Maximum: `20`. A value of `0` means don't retry at all.

**Backoff multiplier**: Base for exponential backoff. A value of `2` (default) means that Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, and so forth.

**Longest interval between retries (ms)**: Maximum time to wait between retry attempts. This is useful for APIs with high backoff timeouts, enabling you to configure longer wait times to avoid excessive retry attempts. Defaults to `20000` ms (20 seconds). Supports values up to `900000` ms (15 minutes) to handle longer backoff requirements. Setting a long retry interval can lead to significantly extended job execution times, which could prevent other jobs from running on the Worker because of concurrent [task limits](collectors-job-limits#task-limits).

**Retry HTTP codes**: List of HTTP codes that trigger a retry. Leave empty to use the defaults (`429` and `503`). Cribl Stream does not retry codes in the `200` series.

**Honor Retry-After header**: When toggled to `Yes` (the default) and the `retry-after` [header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) is present, Cribl Stream honors any `retry-after` header that specifies a delay, up to a maximum of 20 seconds. Cribl Stream always ignores `retry-after` headers that specify a delay longer than 20 seconds. Cribl Stream will log a warning message with the delay value retrieved from the `retry-after` header (converted to ms). When toggled to `No`, Cribl Stream ignores all `retry-after` headers.

**Retry-After header name**: Optionally, if the service uses a different header name for conveying retry information, specify that custom name here. For example, a service might use `x-ratelimit-retry-after` instead.

### Optional Settings {#optional}

**Request timeout (secs)**: Here, you can set a maximum time period (in seconds) for an HTTP request to complete before Cribl Stream treats it as timed out. Defaults to `0`, which disables timeout metering.

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Stream will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.

**Disable time filter**: Toggle to `Yes` if your Run or Schedule configuration specifies a date range and no events are being collected. This will disable the Collector's event time filtering to prevent timestamp conflicts.

**Decode URL**: When toggled to `No`, Cribl Stream will not decode URLs before sending the HTTP request. This setting applies to collect, authentication, and pagination requests. Defaults to `Yes`.

**Reject unauthorized certificates**: Whether to accept certificates that cannot be verified against a valid Certificate Authority (for example, self-signed certificates). Defaults to `No`.

**Capture response headers**: Toggle to `Yes` to include the headers from the API response of a collect call under a `resHeaders` key in the `__collectible` object that is added to [collected events](#internal-fields).

**Safe headers**: Optionally, list headers that you consider safe to log in plain text. Separate the header names with tabs or hard returns.
These headers will not be redacted when exporting the Collector configuration as JSON.

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Settings By Discover Type {#settings-by-discover-type}

For [Discover types](#discover-settings) other than `None`, Cribl Stream exposes additional fields specific to the selected Discover type.

#### Discover Type: Item List {#list}

Setting the **Discover type** to `Item List` exposes this additional field above the [Common Collector Settings](#common):

**Discover items**: List of items to return from the Discover task. Each returned item will generate a Collect task, and can be referenced using `${id}` in the **Collect URL**, the **Collect parameters**, or the **Collect headers**.

#### Discover Type: JSON Response {#json}

Setting the **Discover type** to `JSON Response` exposes these additional fields above the [Common Collector Settings](#common):

**Discover result**: Allows hard-coding the Discover result. Must be a JSON object. Works with the **Discover data field**.

**Discover data field**: Within the response, this is the name of the field that contains discovery results. 
* If the response is a JSON object that contains an array, and you want to generate a separate Collect task for each item in the array, set the **Discover data field** to the key of the array in the JSON object. For example, if the response is `{ "logFiles": ["cribl.log", "cribl.log.1", "cribl.log.2", "audit.log"] }`, setting **Discover data field** to `logFiles` would result in four Collect tasks, one for each log file.
* If the response itself is a JSON array, like `["cribl.log", "cribl.log.1", "cribl.log.2", "audit.log"]`, leave the **Discover data field** blank for each item in the array to become a collection task.

Sample JSON entry:

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

**Discover parameters**:  Optional HTTP request parameters to append to the Discover request URL. These refine or narrow the request. Click **Add Parameter** to add parameters as key-value pairs:

  * **Name**: Parameter name.
  * **Value**: JavaScript expression to compute the parameter's value, normally enclosed in backticks (for example, `` `${earliest}` ``). Can also be a constant, enclosed in single quotes (`'earliest'`). Values without delimiters (for example, `earliest`) are evaluated as strings.

**Discover headers**: Optional Discover request headers.: Click **Add Header** to add headers as key-value pairs:

  * **Name**: Header name.
  * **Value**: JavaScript expression to compute the header's value, normally enclosed in backticks (for example, `` `${earliest}` ``). Can also be a constant, enclosed in single quotes (`'earliest'`). Values without delimiters (for example, `earliest`) are evaluated as strings.

**Discover data field**: Within the response, this is the name of the field that contains discovery results. 
* If the response is a JSON object that contains an array, and you want to generate a separate Collect task for each item in the array, set the **Discover data field** to the key of the array in the JSON object. For example, if the response is `{ "logFiles": ["cribl.log", "cribl.log.1", "cribl.log.2", "audit.log"] }`, setting **Discover data field** to `logFiles` would result in four Collect tasks, one for each log file.
* If the response itself is a JSON array, like `["cribl.log", "cribl.log.1", "cribl.log.2", "audit.log"]`, leave the **Discover data field** blank for each item in the array to become a collection task.

**Format discover result with custom code**: Toggle to `Yes` to provide custom JavaScript code, in the **Format discover result** field, that manipulates the data returned by a Discover request before it's used for collection. This functionality allows you to influence the collection process by transforming Discover results into a specific set of collection tasks.

The custom code operates on the `__e` variable, which is a JSON object or array containing the original Discover results. You can modify the contents of `__e` to define tasks for parallel collection. Here are some key points to remember when working with `__e`:
* Mutable object/array: `__e` is mutable, meaning you can directly modify its contents.
* Response structure: The structure of `__e` depends on the specific response from your Discover request.

For example, to split data by user IDs, assume the Discover result contains user IDs:

    `[{ "name": "root", "id": 1}, { "name": "joe", "id": 2 }, { "name": "jane", "id": 3 },{ "name": "jose", "id": 4 }]`

To achieve a collection task that runs for all users except `root`, you'd use:

    `__e['tasks'] = __e.filter(item => item.name != 'root');`

As another example, imagine an API with two endpoints:
* Endpoint that returns the count of objects that match search criteria.
* Endpoint for collection that returns items using search criteria that also supports pagination.

You can leverage the **Format discover results** option to do the following:
1. Run a Discover REST API call to return the number of items that match search criteria, like an SQL count query.
1. Use the count matching items to generate collect tasks, one for each page of data to retrieve.

For example, assume the discover REST API call returns the following:

    `{ "numMatchingItems": 100000 }`

Assume the max page size is 10,000 for the collect API.

The **Format discover result with custom code** option allows you to implement logic that generates an array of collection tasks, one for each page. You can then leverage this array in conjunction with the collection request parameters to retrieve a unique page of data for each collection task.

For example: `http://0.0.0.0:8004/query?sysparm_offset=${offset}&sysparm_limit=${limit}`

Now you'll transform a Discover result of `{ "numMatchingItems": 100000 }` into 10 distinct collect tasks, each retrieving 10,000 results per page. Some of the tasks can be run in parallel depending on job settings and system load.

``` javascript
// Expecting: { \"numMatchingItems\" : xxxxx };
if (Array.isArray(__e)) {
  const count = __e['numMatchingItems'];
  let pageSize=10000;
  let numPages=count/pageSize;
  if (count % pageSize) {
     numPages++; // count not evenly divisiable by page size, add one more collect task for remaining items
  }
  // Add a collect task for each page of data to be retrieved.
  for (let i = 0; i < numPages; i++) {
    __e.push({ offset: i*pageSize, limit: pageSize, count });
  }
} 
else {
  __e['__resultOut'] = {"error": "Unexpected - discover result is NOT an array"}
}
```

See another use case example in [Using REST/API Collectors](/stream/usecase-rest#discovery-code).

> The following sections describe the Collector Settings' remaining tabs, whose settings and content apply equally to all **Discover type** selections.
>
{.box .success}

## Result Settings

The Result Settings determine how Cribl Stream transforms and routes the collected data. 

#### Custom Command

In this section, you can pass the data from this input to an external command for processing, before the data continues downstream.

**Enabled**: Defaults to `No`. Toggle to `Yes` to enable the custom command.

**Command**: Enter the command that will consume the data (via `stdin`) and will process its output (via `stdout`).

**Arguments**: Click **Add Argument** to add each argument to the command. You can drag arguments vertically to resequence them.

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
- **Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, `GB`, and so forth. (Example: `42 MB`.) Default value of `0` indicates no throttling.

> You might disable **Send to Routes** when configuring a Collector that will connect data from a specific Source to a specific Pipeline and Destination. One use case might be a REST Collector that gathers a known, simple type of data from a single endpoint. This approach keeps the Collector's configuration self‑contained and separate from Cribl Stream's routing table for live data – potentially simplifying the Routes structure.
>
{.box .info}

**Pre-processing Pipeline**: Pipeline to process results before sending to Routes. Optional, and available only when **Send to Routes** is toggled to `Yes`.

**Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, `GB`, and so forth. (Example: `42 MB`.) Default value of `0` indicates no throttling.

## Advanced Settings

Advanced Settings enable you to customize post-processing and administrative options.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Remove Discover fields** : List of fields to remove from the Discover results. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface. You can specify wildcards (such as `aws*`).

**Resume job on boot**: Toggle to `Yes` to resume ad hoc collection jobs if Cribl Stream restarts during the jobs' execution.

## Working with State Tracking {#state-tracking}

You can configure the Collector to track state, either by time or another arbitrary value. This can help prevent overlaps between jobs, where subsequent Collector runs may return some of the same results as previous runs. Similarly, it can help prevent gaps in data by allowing a Collector run to pick up from where the last run ended.

State tracking can be particularly useful for tracking the greatest `_time` value for events returned from a collection run, but you can also use it to derive arbitrary state for custom use cases. You can access saved state values in any Collector field that supports JavaScript expressions.

For a hands-on example of setting up a Collector with state tracking enabled, see [Using REST/API Collectors](usecase-rest#state-tracking-time).

### Tracking State for Collector Runs {#tracking-state-collector-runs}

You can enable state tracking under the `State Tracking` section when [scheduling](collectors-schedule-run#schedule-config) or performing a [full run](collectors-schedule-run#full-run) of the Collector (Preview and Discover runs do not support state tracking). Once enabled, two more fields will appear for state tracking configuration.

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

### Managing Collector State

Click **Manage State** to view, modify, or delete Collector state. For more information, see [Manage Collector State](collectors-manage-state). 

## Importing a Collector Configuration

You can create a REST Collector (or any other Collector) by importing a JSON file that defines a Collector configuration, entering desired values for any placeholders, and saving the new Collector.

[Creating a New Collector from a Template](collectors#creating-from-template) explains how to do this, and Cribl's Collector Templates [repository](https://github.com/criblio/collector-templates/tree/main/collectors/rest) provides configurations for many popular Collectors (with companion Event Breakers, and event samples in some cases). You can import these REST Collectors and save the time you'd have spent building them yourself.

## Formatting Expressions {#expressions}

JavaScript expression fields in this REST/API Collector behave differently from those elsewhere in Cribl Stream. Here, you must enter expressions as template literals, with placeholders referencing variables and JS functions. Here are a few examples: 
- To reference the earliest time variable in a URL, header, or parameter (and so forth), you would write the variable as `` `${earliest}` ``. 
- To reference a function call: `` `${Date.now()}` ``. 
- To call a function that references a variable: `` `${Math.floor(earliest)}` ``.

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## Internal Fields {#internal-fields}

Cribl Stream uses a set of internal fields to assist in data handling. These "meta" fields are **not** part of an event, but they are accessible, and you can use them in [Functions](functions) to make processing decisions.

Relevant fields for this Collector:

* `__collectible` – This object's nested fields contain metadata about each collection job.
  * `collectorType`: Indicates the type of Collector used for the job.
  * `collectorId`: Represents the **Collector ID** of the Collector, as configured during setup.
  * `resHeaders`: Added only when **Advanced Settings** > **Capture response headers** is toggled on.
* `__collectStats` – This object's nested fields are:
    * `method`: The value you set for **Collect** > **Collect method**.
    * `url`: The value you set for **Collect Settings (Common)** > **Collect URL**.
    * `elapsedMS`: The time elapsed from the moment the Collector's request was sent until the response was received.
* `__inputId` – Uniquely identifies the origin of data for a [collection job](collectors-schedule-run). Its format varies depending on whether the job is ad hoc or scheduled:
  * Ad hoc jobs are formatted as `collection:<timestamp>.<randomId>.adhoc.<Collector ID>`.
    * `<timestamp>`: The Unix timestamp when the job was initiated.
    * `<randomId>`: A random identifier to ensure uniqueness.
    * `adhoc`: Indicates the job was manually triggered.
    * `<Collector ID>`: The ID of the Collector.
  * Scheduled jobs are formatted as `collection:<Collector ID>`.
    * `<Collector ID>`: The ID of the Collector.

## Response Errors

The Collector treats all non-`200` responses from configured URL endpoints as errors. This includes `1xx`, `3xx`, `4xx`, and `5xx` responses.

On Discovery, Preview, and most Collect jobs, it interprets these as fatal errors. On Collect jobs, a few exceptions are treated as non-fatal:

- Where a Collect job launches multiple tasks, and only a subset of those tasks fail, Cribl Stream places the job in failed status, but treats the error as non-fatal. (Note that Cribl Stream does not retry the failed tasks.)

- Where a Collect job receives a `3xx` redirection error code, it follows the error's treatment by the underlying library, and does not necessarily treat the error as fatal.

## Mitigating Stuck-Job Problems

Occasionally, a scheduled job fails, but continues running for hours or even days, until someone intervenes and cancels it. If left alone, such a "stuck", "orphaned," or "zombie" job will never complete. This can cause missing events in downstream receivers, along with `HTTP timeout` or similar errors in Cribl Stream's logs.

To keep stuck jobs from running excessively long:

* First, try setting **Optional Settings** > **Request timeout (secs)** – whose default of `0` means "wait forever" – to a desired maximum duration.
* If adjusting **Timeout (secs)** does not fix the problem, try the global setting **Job Timeout** – whose default of `0` allows a job to run indefinitely – to a desired maximum duration. You'll find **Job Timeout** among the [task manifest and buffering limits](collectors-job-limits#task-manifest-and-buffering-limits). 

Using these settings in tandem works like this:

* **Timeout (secs)** limits the time that Cribl Stream will wait for an HTTP request to complete. 
* Then, if a job gets stuck and keeps running beyond that limit, **Job Timeout** can catch and terminate the job, because it monitors the overall time the job has been running.

## Troubleshooting

### "reason.stack == `TypeError [ERR_INVALID_URL]..."

**Ful text**: "reason.stack == `TypeError [ERR_INVALID_URL]: Invalid URL: https%3A%2F%2Ftype.fit%2Fapi%2Fquotes" at onParseError (internal/url.js:258:9)"

This is due to unnecessarily encoding the Discover/Collect URL.

##### Recommendation

Remove the encoding function for the URL. URLs will rarely need text to be encoded and when they do it's only the parts that need it that should be encoded, otherwise if the entire URL is encoded unnecessarily then errors like this will occur.

![Invalid URL](Screen_Shot_2021-04-13_at_5.53.56_PM.e4e38e435c.png)
{scale="40%"}

### "statusCode: 429...Too many requests"

This response is triggered by rapidly repeated requests from the Collector's Discover and Collect phases. It's especially likely when different Workers run multiple Collect tasks.

##### Recommendation

Gradually increase the **Login rate limit** threshold until you no longer receive this error response. On the Leader or a Single-instance deployment, access this option at **Global Settings** > **General Settings** > **API Server Settings** > **Advanced**. In a Distributed deployment, select **Manage**, select the Worker Group, and then navigate to **Group Settings** > **General Settings** > **API Server Settings** > **Advanced**.
