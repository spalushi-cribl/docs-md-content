
<h1 id="cribl-stream-api-functions">Cribl Stream API - Functions v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-functions-functions">Functions</h1>

## Get a list of Function objects

<a id="opIdlistFunction"></a>

> Code samples

`GET /functions`

Get a list of Function objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "__conf": {},
      "__filename": "string",
      "disabled": true,
      "group": "string",
      "id": "string",
      "initTime": 0,
      "loadTime": 0,
      "modTime": 0,
      "name": "string",
      "uischema": {},
      "version": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-function-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Function objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-function-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Function](#schemafunction)]|false|none|none|
|»» __conf|object|true|none|none|
|»» __filename|string|true|none|none|
|»» disabled|boolean|true|none|none|
|»» group|string|true|none|none|
|»» id|string|true|none|none|
|»» initTime|number|true|none|none|
|»» loadTime|number|true|none|none|
|»» modTime|number|true|none|none|
|»» name|string|true|none|none|
|»» uischema|object|true|none|none|
|»» version|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get Function by ID

<a id="opIdgetFunctionById"></a>

> Code samples

`GET /functions/{id}`

Get Function by ID

<h3 id="get-function-by-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to GET|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "__conf": {},
      "__filename": "string",
      "disabled": true,
      "group": "string",
      "id": "string",
      "initTime": 0,
      "loadTime": 0,
      "modTime": 0,
      "name": "string",
      "uischema": {},
      "version": "string"
    }
  ]
}
```

<h3 id="get-function-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Function objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-function-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Function](#schemafunction)]|false|none|none|
|»» __conf|object|true|none|none|
|»» __filename|string|true|none|none|
|»» disabled|boolean|true|none|none|
|»» group|string|true|none|none|
|»» id|string|true|none|none|
|»» initTime|number|true|none|none|
|»» loadTime|number|true|none|none|
|»» modTime|number|true|none|none|
|»» name|string|true|none|none|
|»» uischema|object|true|none|none|
|»» version|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Function">Function</h2>
<!-- backwards compatibility -->
<a id="schemafunction"></a>
<a id="schema_Function"></a>
<a id="tocSfunction"></a>
<a id="tocsfunction"></a>

```json
{
  "__conf": {},
  "__filename": "string",
  "disabled": true,
  "group": "string",
  "id": "string",
  "initTime": 0,
  "loadTime": 0,
  "modTime": 0,
  "name": "string",
  "uischema": {},
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|__conf|object|true|none|none|
|__filename|string|true|none|none|
|disabled|boolean|true|none|none|
|group|string|true|none|none|
|id|string|true|none|none|
|initTime|number|true|none|none|
|loadTime|number|true|none|none|
|modTime|number|true|none|none|
|name|string|true|none|none|
|uischema|object|true|none|none|
|version|string|true|none|none|

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

