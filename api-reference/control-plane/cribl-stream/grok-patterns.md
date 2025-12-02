
<h1 id="cribl-stream-api-grok-patterns">Cribl Stream API - Grok Patterns v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-grok-patterns-grok-patterns">Grok Patterns</h1>

## Get a list of GrokFile objects

<a id="opIdlistGrokFile"></a>

> Code samples

`GET /lib/grok`

Get a list of GrokFile objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "content": "string",
      "id": "string",
      "size": 0,
      "tags": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-grokfile-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GrokFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-grokfile-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GrokFile](#schemagrokfile)]|false|none|none|
|»» content|string|true|none|none|
|»» id|string|true|none|none|
|»» size|number|true|none|none|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create GrokFile

<a id="opIdcreateGrokFile"></a>

> Code samples

`POST /lib/grok`

Create GrokFile

> Body parameter

```json
{
  "content": "string",
  "id": "string",
  "size": 0,
  "tags": "string"
}
```

<h3 id="create-grokfile-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[GrokFile](#schemagrokfile)|true|New GrokFile object|
|» content|body|string|true|none|
|» id|body|string|true|none|
|» size|body|number|true|none|
|» tags|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "content": "string",
      "id": "string",
      "size": 0,
      "tags": "string"
    }
  ]
}
```

<h3 id="create-grokfile-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GrokFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-grokfile-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GrokFile](#schemagrokfile)]|false|none|none|
|»» content|string|true|none|none|
|»» id|string|true|none|none|
|»» size|number|true|none|none|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete GrokFile

<a id="opIddeleteGrokFileById"></a>

> Code samples

`DELETE /lib/grok/{id}`

Delete GrokFile

<h3 id="delete-grokfile-parameters">Parameters</h3>

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
      "content": "string",
      "id": "string",
      "size": 0,
      "tags": "string"
    }
  ]
}
```

<h3 id="delete-grokfile-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GrokFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-grokfile-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GrokFile](#schemagrokfile)]|false|none|none|
|»» content|string|true|none|none|
|»» id|string|true|none|none|
|»» size|number|true|none|none|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get GrokFile by ID

<a id="opIdgetGrokFileById"></a>

> Code samples

`GET /lib/grok/{id}`

Get GrokFile by ID

<h3 id="get-grokfile-by-id-parameters">Parameters</h3>

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
      "content": "string",
      "id": "string",
      "size": 0,
      "tags": "string"
    }
  ]
}
```

<h3 id="get-grokfile-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GrokFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-grokfile-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GrokFile](#schemagrokfile)]|false|none|none|
|»» content|string|true|none|none|
|»» id|string|true|none|none|
|»» size|number|true|none|none|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update GrokFile

<a id="opIdupdateGrokFileById"></a>

> Code samples

`PATCH /lib/grok/{id}`

Update GrokFile

> Body parameter

```json
{
  "content": "string",
  "id": "string",
  "size": 0,
  "tags": "string"
}
```

<h3 id="update-grokfile-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[GrokFile](#schemagrokfile)|true|GrokFile object to be updated|
|» content|body|string|true|none|
|» id|body|string|true|none|
|» size|body|number|true|none|
|» tags|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "content": "string",
      "id": "string",
      "size": 0,
      "tags": "string"
    }
  ]
}
```

<h3 id="update-grokfile-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GrokFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-grokfile-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GrokFile](#schemagrokfile)]|false|none|none|
|»» content|string|true|none|none|
|»» id|string|true|none|none|
|»» size|number|true|none|none|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_GrokFile">GrokFile</h2>
<!-- backwards compatibility -->
<a id="schemagrokfile"></a>
<a id="schema_GrokFile"></a>
<a id="tocSgrokfile"></a>
<a id="tocsgrokfile"></a>

```json
{
  "content": "string",
  "id": "string",
  "size": 0,
  "tags": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|content|string|true|none|none|
|id|string|true|none|none|
|size|number|true|none|none|
|tags|string|false|none|none|

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

