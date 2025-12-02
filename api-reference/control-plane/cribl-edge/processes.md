
<h1 id="cribl-edge-api-processes">Cribl Edge API - Processes v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-processes-processes">Processes</h1>

## Get a detailed list of processes running on the edge host

<a id="opIdgetEdgeProcesses"></a>

> Code samples

`GET /edge/processes`

Get a detailed list of processes running on the edge host

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "pid": 0,
      "ppid": 0
    }
  ]
}
```

<h3 id="get-a-detailed-list-of-processes-running-on-the-edge-host-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Process objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-detailed-list-of-processes-running-on-the-edge-host-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Process](#schemaprocess)]|false|none|none|
|»» id|string|true|none|none|
|»» pid|number|true|none|none|
|»» ppid|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get details of a process running on the edge host

<a id="opIdgetEdgeProcessesByPid"></a>

> Code samples

`GET /edge/processes/{pid}`

Get details of a process running on the edge host

<h3 id="get-details-of-a-process-running-on-the-edge-host-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|pid|path|string|true|PID of the process|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "pid": 0,
      "ppid": 0
    }
  ]
}
```

<h3 id="get-details-of-a-process-running-on-the-edge-host-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Process objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-details-of-a-process-running-on-the-edge-host-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Process](#schemaprocess)]|false|none|none|
|»» id|string|true|none|none|
|»» pid|number|true|none|none|
|»» ppid|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Process">Process</h2>
<!-- backwards compatibility -->
<a id="schemaprocess"></a>
<a id="schema_Process"></a>
<a id="tocSprocess"></a>
<a id="tocsprocess"></a>

```json
{
  "id": "string",
  "pid": 0,
  "ppid": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|pid|number|true|none|none|
|ppid|number|true|none|none|

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

