
<h1 id="cribl-edge-api-performance-profiling">Cribl Edge API - Performance Profiling v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-performance-profiling-performance-profiling">Performance Profiling</h1>

## Get a list of ProfilerItem objects

<a id="opIdlistProfilerItem"></a>

> Code samples

`GET /system/profiler`

Get a list of ProfilerItem objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "createTime": 0,
      "id": "string",
      "size": 0,
      "workerId": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-profileritem-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ProfilerItem objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-profileritem-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ProfilerItem](#schemaprofileritem)]|false|none|none|
|»» createTime|number|false|none|none|
|»» id|string|true|none|none|
|»» size|number|false|none|none|
|»» workerId|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create ProfilerItem

<a id="opIdcreateProfilerItem"></a>

> Code samples

`POST /system/profiler`

Create ProfilerItem

> Body parameter

```json
{
  "createTime": 0,
  "id": "string",
  "size": 0,
  "workerId": "string"
}
```

<h3 id="create-profileritem-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[ProfilerItem](#schemaprofileritem)|true|New ProfilerItem object|
|» createTime|body|number|false|none|
|» id|body|string|true|none|
|» size|body|number|false|none|
|» workerId|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "createTime": 0,
      "id": "string",
      "size": 0,
      "workerId": "string"
    }
  ]
}
```

<h3 id="create-profileritem-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ProfilerItem objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-profileritem-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ProfilerItem](#schemaprofileritem)]|false|none|none|
|»» createTime|number|false|none|none|
|»» id|string|true|none|none|
|»» size|number|false|none|none|
|»» workerId|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete ProfilerItem

<a id="opIddeleteProfilerItemById"></a>

> Code samples

`DELETE /system/profiler/{id}`

Delete ProfilerItem

<h3 id="delete-profileritem-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to DELETE|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "createTime": 0,
      "id": "string",
      "size": 0,
      "workerId": "string"
    }
  ]
}
```

<h3 id="delete-profileritem-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ProfilerItem objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-profileritem-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ProfilerItem](#schemaprofileritem)]|false|none|none|
|»» createTime|number|false|none|none|
|»» id|string|true|none|none|
|»» size|number|false|none|none|
|»» workerId|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update ProfilerItem

<a id="opIdupdateProfilerItemById"></a>

> Code samples

`PATCH /system/profiler/{id}`

Update ProfilerItem

> Body parameter

```json
{
  "createTime": 0,
  "id": "string",
  "size": 0,
  "workerId": "string"
}
```

<h3 id="update-profileritem-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[ProfilerItem](#schemaprofileritem)|true|ProfilerItem object to be updated|
|» createTime|body|number|false|none|
|» id|body|string|true|none|
|» size|body|number|false|none|
|» workerId|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "createTime": 0,
      "id": "string",
      "size": 0,
      "workerId": "string"
    }
  ]
}
```

<h3 id="update-profileritem-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ProfilerItem objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-profileritem-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ProfilerItem](#schemaprofileritem)]|false|none|none|
|»» createTime|number|false|none|none|
|»» id|string|true|none|none|
|»» size|number|false|none|none|
|»» workerId|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_ProfilerItem">ProfilerItem</h2>
<!-- backwards compatibility -->
<a id="schemaprofileritem"></a>
<a id="schema_ProfilerItem"></a>
<a id="tocSprofileritem"></a>
<a id="tocsprofileritem"></a>

```json
{
  "createTime": 0,
  "id": "string",
  "size": 0,
  "workerId": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|createTime|number|false|none|none|
|id|string|true|none|none|
|size|number|false|none|none|
|workerId|string|false|none|none|

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

