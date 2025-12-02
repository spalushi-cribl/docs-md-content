
<h1 id="cribl-search-api-health-and-diagnostics-cribl-search-">Cribl Search API - Health and Diagnostics (Cribl Search) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-search-api-health-and-diagnostics-cribl-search--health-and-diagnostics-cribl-search-">Health and Diagnostics (Cribl Search)</h1>

## Get health check metric for search

<a id="opIdgetSearchHealthcheck"></a>

> Code samples

`GET /search/healthcheck`

Get health check metric for search

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "reported_at": 0,
      "status": "green"
    }
  ]
}
```

<h3 id="get-health-check-metric-for-search-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchHealthCheckStatus objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-health-check-metric-for-search-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[oneOf]|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» reported_at|number|true|none|none|
|»»» status|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» reason|string|true|none|none|
|»»» reported_at|number|true|none|none|
|»»» status|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|green|
|reason|QUEUE_NOT_HEALTHY|
|reason|SERVICE_UNAVAILABLE|
|reason|STATUS_CHECK_FAILED|
|status|red|

<aside class="success">
This operation does not require authentication
</aside>

## List metrics for all search jobs

<a id="opIdgetSearchJobMetrics"></a>

> Code samples

`GET /search/job-metrics`

List metrics for all search jobs

> Example responses

> 200 Response

```json
"string"
```

<h3 id="list-metrics-for-all-search-jobs-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|string object|string|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get search logs

<a id="opIdgetSearchJobLogsById"></a>

> Code samples

`GET /search/jobs/{id}/logs`

Get search logs

<h3 id="get-search-logs-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job id|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    "string"
  ]
}
```

<h3 id="get-search-logs-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of string objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-search-logs-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get search job metrics

<a id="opIdgetSearchJobsMetricsById"></a>

> Code samples

`GET /search/jobs/{id}/metrics`

Get search job metrics

<h3 id="get-search-job-metrics-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job id|

> Example responses

> 200 Response

```json
"string"
```

<h3 id="get-search-job-metrics-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|string object|string|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get Search documentation

<a id="opIdgetSearchDocs"></a>

> Code samples

`GET /search/docs`

Get Search documentation

> Example responses

> 200 Response

```json
"string"
```

<h3 id="get-search-documentation-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|string object|string|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Send metric events from the UI endpoint

<a id="opIdcreateSearchUiMetrics"></a>

> Code samples

`POST /search/ui-metrics`

Send metric events from the UI endpoint

> Example responses

> 200 Response

```json
"string"
```

<h3 id="send-metric-events-from-the-ui-endpoint-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|string object|string|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get full search job data

<a id="opIdgetSearchJobsDiagById"></a>

> Code samples

`GET /search/jobs/{id}/diag`

Get full search job data

<h3 id="get-full-search-job-data-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job id|

> Example responses

> 200 Response

```json
"string"
```

<h3 id="get-full-search-job-data-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|string object|string|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get the contents of a log file

<a id="opIdgetSearchJobsLogsByIdAndFilename"></a>

> Code samples

`GET /search/jobs/{id}/logs/{filename}`

Get the contents of a log file

<h3 id="get-the-contents-of-a-log-file-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job id|
|filename|path|string|true|name of the log file to get|

> Example responses

> 200 Response

```json
"string"
```

<h3 id="get-the-contents-of-a-log-file-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|string object|string|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_SearchHealthCheckStatus">SearchHealthCheckStatus</h2>
<!-- backwards compatibility -->
<a id="schemasearchhealthcheckstatus"></a>
<a id="schema_SearchHealthCheckStatus"></a>
<a id="tocSsearchhealthcheckstatus"></a>
<a id="tocssearchhealthcheckstatus"></a>

```json
{
  "reported_at": 0,
  "status": "green"
}

```

### Properties

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» reported_at|number|true|none|none|
|» status|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» reason|string|true|none|none|
|» reported_at|number|true|none|none|
|» status|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|green|
|reason|QUEUE_NOT_HEALTHY|
|reason|SERVICE_UNAVAILABLE|
|reason|STATUS_CHECK_FAILED|
|status|red|

<h2 id="tocS_Error">Error</h2>
<!-- backwards compatibility -->
<a id="schemaerror"></a>
<a id="schema_Error"></a>
<a id="tocSerror"></a>
<a id="tocserror"></a>

```json
{
  "message": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|message|string|false|none|Error message|

