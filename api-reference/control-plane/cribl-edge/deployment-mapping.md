
<h1 id="cribl-edge-api-deployment-mapping">Cribl Edge API - Deployment Mapping v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-deployment-mapping-deployment-mapping">Deployment Mapping</h1>

## Data for the Map View for Edge Fleets (Leader only)

<a id="opIdcreateProductsEdgeMapQuery"></a>

> Code samples

`POST /products/edge/map/query`

Data for the Map View for Edge Fleets (Leader only)

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "__worker_info": {
        "hostname": "string"
      },
      "__worker_node": "string"
    }
  ]
}
```

<h3 id="data-for-the-map-view-for-edge-fleets-(leader-only)-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of EdgeMapQueryResult objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="data-for-the-map-view-for-edge-fleets-(leader-only)-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[EdgeMapQueryResult](#schemaedgemapqueryresult)]|false|none|none|
|»» __worker_info|object|true|none|none|
|»»» hostname|string|true|none|none|
|»» __worker_node|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get a directory listing of the given path

<a id="opIdgetEdgeLsByPath"></a>

> Code samples

`GET /edge/ls{path}`

Get a directory listing of the given path

<h3 id="get-a-directory-listing-of-the-given-path-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|path|path|string|true|Defaults to empty for the root directory|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "mode": "string",
      "name": "string",
      "stats": {},
      "symLinkInfo": {
        "symLinkTarget": "string",
        "symLinkTargetAbsolutePath": "string",
        "symLinkTargetIsDirectory": true,
        "symLinkTargetIsFile": true
      },
      "type": "string"
    }
  ]
}
```

<h3 id="get-a-directory-listing-of-the-given-path-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of FilesystemEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-directory-listing-of-the-given-path-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[FilesystemEntry](#schemafilesystementry)]|false|none|none|
|»» mode|string|false|none|none|
|»» name|string|true|none|none|
|»» stats|object|false|none|none|
|»» symLinkInfo|[SymLinkInfo](#schemasymlinkinfo)|false|none|none|
|»»» symLinkTarget|string|true|none|none|
|»»» symLinkTargetAbsolutePath|string|true|none|none|
|»»» symLinkTargetIsDirectory|boolean|false|none|none|
|»»» symLinkTargetIsFile|boolean|false|none|none|
|»» type|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_EdgeMapQueryResult">EdgeMapQueryResult</h2>
<!-- backwards compatibility -->
<a id="schemaedgemapqueryresult"></a>
<a id="schema_EdgeMapQueryResult"></a>
<a id="tocSedgemapqueryresult"></a>
<a id="tocsedgemapqueryresult"></a>

```json
{
  "__worker_info": {
    "hostname": "string"
  },
  "__worker_node": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|__worker_info|object|true|none|none|
|» hostname|string|true|none|none|
|__worker_node|string|true|none|none|

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

<h2 id="tocS_FilesystemEntry">FilesystemEntry</h2>
<!-- backwards compatibility -->
<a id="schemafilesystementry"></a>
<a id="schema_FilesystemEntry"></a>
<a id="tocSfilesystementry"></a>
<a id="tocsfilesystementry"></a>

```json
{
  "mode": "string",
  "name": "string",
  "stats": {},
  "symLinkInfo": {
    "symLinkTarget": "string",
    "symLinkTargetAbsolutePath": "string",
    "symLinkTargetIsDirectory": true,
    "symLinkTargetIsFile": true
  },
  "type": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|mode|string|false|none|none|
|name|string|true|none|none|
|stats|object|false|none|none|
|symLinkInfo|[SymLinkInfo](#schemasymlinkinfo)|false|none|none|
|type|string|true|none|none|

<h2 id="tocS_SymLinkInfo">SymLinkInfo</h2>
<!-- backwards compatibility -->
<a id="schemasymlinkinfo"></a>
<a id="schema_SymLinkInfo"></a>
<a id="tocSsymlinkinfo"></a>
<a id="tocssymlinkinfo"></a>

```json
{
  "symLinkTarget": "string",
  "symLinkTargetAbsolutePath": "string",
  "symLinkTargetIsDirectory": true,
  "symLinkTargetIsFile": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|symLinkTarget|string|true|none|none|
|symLinkTargetAbsolutePath|string|true|none|none|
|symLinkTargetIsDirectory|boolean|false|none|none|
|symLinkTargetIsFile|boolean|false|none|none|

