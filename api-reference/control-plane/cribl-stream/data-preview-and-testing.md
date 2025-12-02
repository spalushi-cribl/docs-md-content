
<h1 id="cribl-stream-api-data-preview-and-testing">Cribl Stream API - Data Preview and Testing v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-data-preview-and-testing-data-preview-and-testing">Data Preview and Testing</h1>

## Sends sample events through a pipeline and returns the results

<a id="opIdcreatePreview"></a>

> Code samples

`POST /preview`

Sends sample events through a pipeline and returns the results

> Body parameter

```json
{
  "cpuProfile": true,
  "dropped": true,
  "enhanceMetricsOutput": true,
  "events": [
    {}
  ],
  "inputId": "string",
  "level": 0,
  "memory": 0,
  "mode": "pipe",
  "pipelineId": "string",
  "sampleId": "string",
  "samplePipelineId": "string",
  "timeout": 0
}
```

<h3 id="sends-sample-events-through-a-pipeline-and-returns-the-results-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[PreviewDataParams](#schemapreviewdataparams)|true|PreviewDataParams object|
|» cpuProfile|body|boolean|false|none|
|» dropped|body|boolean|false|none|
|» enhanceMetricsOutput|body|boolean|false|none|
|» events|body|[object]|false|none|
|» inputId|body|string|false|none|
|» level|body|number|false|none|
|» memory|body|number|false|none|
|» mode|body|string|true|none|
|» pipelineId|body|string|true|none|
|» sampleId|body|string|true|none|
|» samplePipelineId|body|string|false|none|
|» timeout|body|number|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» mode|pipe|
|» mode|route|
|» mode|routeAndSend|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="sends-sample-events-through-a-pipeline-and-returns-the-results-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="sends-sample-events-through-a-pipeline-and-returns-the-results-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Capture live incoming data

<a id="opIdcreateSystemCapture"></a>

> Code samples

`POST /system/capture`

Capture live incoming data

> Body parameter

```json
{
  "duration": 0,
  "filter": "string",
  "level": 0,
  "maxEvents": 0,
  "stepDuration": 0,
  "workerId": "string",
  "workerThreshold": 0
}
```

<h3 id="capture-live-incoming-data-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[CaptureParams](#schemacaptureparams)|true|CaptureParams object|
|» duration|body|number|true|none|
|» filter|body|string|true|none|
|» level|body|[CaptureLevel](#schemacapturelevel)|true|none|
|» maxEvents|body|number|true|none|
|» stepDuration|body|number|false|none|
|» workerId|body|string|false|none|
|» workerThreshold|body|number|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» level|0|
|» level|1|
|» level|2|
|» level|3|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="capture-live-incoming-data-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="capture-live-incoming-data-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Capture live incoming data from a particular project and subscription at the subscription

<a id="opIdcreateSystemProjectsSubscriptionsCaptureByProjectIdAndSubscriptionId"></a>

> Code samples

`POST /system/projects/{projectId}/subscriptions/{subscriptionId}/capture`

Capture live incoming data from a particular project and subscription at the subscription

> Body parameter

```json
{
  "duration": 0,
  "filter": "string",
  "level": 0,
  "maxEvents": 0,
  "stepDuration": 0,
  "workerId": "string",
  "workerThreshold": 0
}
```

<h3 id="capture-live-incoming-data-from-a-particular-project-and-subscription-at-the-subscription-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|projectId|path|string|true|Project Id|
|subscriptionId|path|string|true|SubscriptionId Id|
|body|body|[CaptureParams](#schemacaptureparams)|true|CaptureParams object|
|» duration|body|number|true|none|
|» filter|body|string|true|none|
|» level|body|[CaptureLevel](#schemacapturelevel)|true|none|
|» maxEvents|body|number|true|none|
|» stepDuration|body|number|false|none|
|» workerId|body|string|false|none|
|» workerThreshold|body|number|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» level|0|
|» level|1|
|» level|2|
|» level|3|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="capture-live-incoming-data-from-a-particular-project-and-subscription-at-the-subscription-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="capture-live-incoming-data-from-a-particular-project-and-subscription-at-the-subscription-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Capture live incoming data from a particular project and subscription at the destination

<a id="opIdcreateSystemProjectsCaptureByProjectId"></a>

> Code samples

`POST /system/projects/{projectId}/capture`

Capture live incoming data from a particular project and subscription at the destination

> Body parameter

```json
{
  "duration": 0,
  "filter": "string",
  "level": 0,
  "maxEvents": 0,
  "stepDuration": 0,
  "workerId": "string",
  "workerThreshold": 0
}
```

<h3 id="capture-live-incoming-data-from-a-particular-project-and-subscription-at-the-destination-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|projectId|path|string|true|Project ID|
|body|body|[CaptureParams](#schemacaptureparams)|true|CaptureParams object|
|» duration|body|number|true|none|
|» filter|body|string|true|none|
|» level|body|[CaptureLevel](#schemacapturelevel)|true|none|
|» maxEvents|body|number|true|none|
|» stepDuration|body|number|false|none|
|» workerId|body|string|false|none|
|» workerThreshold|body|number|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» level|0|
|» level|1|
|» level|2|
|» level|3|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="capture-live-incoming-data-from-a-particular-project-and-subscription-at-the-destination-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="capture-live-incoming-data-from-a-particular-project-and-subscription-at-the-destination-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

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

<h2 id="tocS_PreviewDataParams">PreviewDataParams</h2>
<!-- backwards compatibility -->
<a id="schemapreviewdataparams"></a>
<a id="schema_PreviewDataParams"></a>
<a id="tocSpreviewdataparams"></a>
<a id="tocspreviewdataparams"></a>

```json
{
  "cpuProfile": true,
  "dropped": true,
  "enhanceMetricsOutput": true,
  "events": [
    {}
  ],
  "inputId": "string",
  "level": 0,
  "memory": 0,
  "mode": "pipe",
  "pipelineId": "string",
  "sampleId": "string",
  "samplePipelineId": "string",
  "timeout": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cpuProfile|boolean|false|none|none|
|dropped|boolean|false|none|none|
|enhanceMetricsOutput|boolean|false|none|none|
|events|[object]|false|none|none|
|inputId|string|false|none|none|
|level|number|false|none|none|
|memory|number|false|none|none|
|mode|string|true|none|none|
|pipelineId|string|true|none|none|
|sampleId|string|true|none|none|
|samplePipelineId|string|false|none|none|
|timeout|number|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|mode|pipe|
|mode|route|
|mode|routeAndSend|

<h2 id="tocS_CaptureParams">CaptureParams</h2>
<!-- backwards compatibility -->
<a id="schemacaptureparams"></a>
<a id="schema_CaptureParams"></a>
<a id="tocScaptureparams"></a>
<a id="tocscaptureparams"></a>

```json
{
  "duration": 0,
  "filter": "string",
  "level": 0,
  "maxEvents": 0,
  "stepDuration": 0,
  "workerId": "string",
  "workerThreshold": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|duration|number|true|none|none|
|filter|string|true|none|none|
|level|[CaptureLevel](#schemacapturelevel)|true|none|none|
|maxEvents|number|true|none|none|
|stepDuration|number|false|none|none|
|workerId|string|false|none|none|
|workerThreshold|number|false|none|none|

<h2 id="tocS_CaptureLevel">CaptureLevel</h2>
<!-- backwards compatibility -->
<a id="schemacapturelevel"></a>
<a id="schema_CaptureLevel"></a>
<a id="tocScapturelevel"></a>
<a id="tocscapturelevel"></a>

```json
0

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|integer|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|0|
|*anonymous*|1|
|*anonymous*|2|
|*anonymous*|3|

