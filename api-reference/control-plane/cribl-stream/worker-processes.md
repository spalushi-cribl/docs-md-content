
<h1 id="cribl-stream-api-worker-processes">Cribl Stream API - Worker Processes v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-worker-processes-worker-processes">Worker Processes</h1>

## Get a list of processes under management

<a id="opIdgetSystemProcesses"></a>

> Code samples

`GET /system/processes`

Get a list of processes under management

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "env": {},
      "id": "string",
      "pid": 0,
      "restartOnExit": true,
      "restarts": 0,
      "startTime": 0,
      "type": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-processes-under-management-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ProcessEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-processes-under-management-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ProcessEntry](#schemaprocessentry)]|false|none|none|
|»» env|object|false|none|none|
|»» id|string|true|none|none|
|»» pid|number|false|none|none|
|»» restartOnExit|boolean|true|none|none|
|»» restarts|number|true|none|none|
|»» startTime|number|true|none|none|
|»» type|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_ProcessEntry">ProcessEntry</h2>
<!-- backwards compatibility -->
<a id="schemaprocessentry"></a>
<a id="schema_ProcessEntry"></a>
<a id="tocSprocessentry"></a>
<a id="tocsprocessentry"></a>

```json
{
  "env": {},
  "id": "string",
  "pid": 0,
  "restartOnExit": true,
  "restarts": 0,
  "startTime": 0,
  "type": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|env|object|false|none|none|
|id|string|true|none|none|
|pid|number|false|none|none|
|restartOnExit|boolean|true|none|none|
|restarts|number|true|none|none|
|startTime|number|true|none|none|
|type|string|true|none|none|

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

