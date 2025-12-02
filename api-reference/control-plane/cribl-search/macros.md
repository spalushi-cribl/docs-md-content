
<h1 id="cribl-search-api-macros">Cribl Search API - Macros v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-search-api-macros-macros">Macros</h1>

## Get a list of SearchMacro objects

<a id="opIdlistSearchMacro"></a>

> Code samples

`GET /search/macros`

Get a list of SearchMacro objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "created": 0,
      "createdBy": "string",
      "description": "string",
      "id": "string",
      "modified": 0,
      "replacement": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-searchmacro-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchMacro objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-searchmacro-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchMacro](#schemasearchmacro)]|false|none|none|
|»» created|number|false|none|none|
|»» createdBy|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» modified|number|false|none|none|
|»» replacement|string|true|none|none|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create SearchMacro

<a id="opIdcreateSearchMacro"></a>

> Code samples

`POST /search/macros`

Create SearchMacro

> Body parameter

```json
{
  "created": 0,
  "createdBy": "string",
  "description": "string",
  "id": "string",
  "modified": 0,
  "replacement": "string",
  "tags": "string"
}
```

<h3 id="create-searchmacro-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[SearchMacro](#schemasearchmacro)|true|New SearchMacro object|
|» created|body|number|false|none|
|» createdBy|body|string|false|none|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» modified|body|number|false|none|
|» replacement|body|string|true|none|
|» tags|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "created": 0,
      "createdBy": "string",
      "description": "string",
      "id": "string",
      "modified": 0,
      "replacement": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="create-searchmacro-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchMacro objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-searchmacro-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchMacro](#schemasearchmacro)]|false|none|none|
|»» created|number|false|none|none|
|»» createdBy|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» modified|number|false|none|none|
|»» replacement|string|true|none|none|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete SearchMacro

<a id="opIddeleteSearchMacroById"></a>

> Code samples

`DELETE /search/macros/{id}`

Delete SearchMacro

<h3 id="delete-searchmacro-parameters">Parameters</h3>

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
      "created": 0,
      "createdBy": "string",
      "description": "string",
      "id": "string",
      "modified": 0,
      "replacement": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="delete-searchmacro-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchMacro objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-searchmacro-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchMacro](#schemasearchmacro)]|false|none|none|
|»» created|number|false|none|none|
|»» createdBy|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» modified|number|false|none|none|
|»» replacement|string|true|none|none|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get SearchMacro by ID

<a id="opIdgetSearchMacroById"></a>

> Code samples

`GET /search/macros/{id}`

Get SearchMacro by ID

<h3 id="get-searchmacro-by-id-parameters">Parameters</h3>

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
      "created": 0,
      "createdBy": "string",
      "description": "string",
      "id": "string",
      "modified": 0,
      "replacement": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="get-searchmacro-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchMacro objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-searchmacro-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchMacro](#schemasearchmacro)]|false|none|none|
|»» created|number|false|none|none|
|»» createdBy|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» modified|number|false|none|none|
|»» replacement|string|true|none|none|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update SearchMacro

<a id="opIdupdateSearchMacroById"></a>

> Code samples

`PATCH /search/macros/{id}`

Update SearchMacro

> Body parameter

```json
{
  "created": 0,
  "createdBy": "string",
  "description": "string",
  "id": "string",
  "modified": 0,
  "replacement": "string",
  "tags": "string"
}
```

<h3 id="update-searchmacro-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[SearchMacro](#schemasearchmacro)|true|SearchMacro object to be updated|
|» created|body|number|false|none|
|» createdBy|body|string|false|none|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» modified|body|number|false|none|
|» replacement|body|string|true|none|
|» tags|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "created": 0,
      "createdBy": "string",
      "description": "string",
      "id": "string",
      "modified": 0,
      "replacement": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="update-searchmacro-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchMacro objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-searchmacro-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchMacro](#schemasearchmacro)]|false|none|none|
|»» created|number|false|none|none|
|»» createdBy|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» modified|number|false|none|none|
|»» replacement|string|true|none|none|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_SearchMacro">SearchMacro</h2>
<!-- backwards compatibility -->
<a id="schemasearchmacro"></a>
<a id="schema_SearchMacro"></a>
<a id="tocSsearchmacro"></a>
<a id="tocssearchmacro"></a>

```json
{
  "created": 0,
  "createdBy": "string",
  "description": "string",
  "id": "string",
  "modified": 0,
  "replacement": "string",
  "tags": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created|number|false|none|none|
|createdBy|string|false|none|none|
|description|string|false|none|none|
|id|string|true|none|none|
|modified|number|false|none|none|
|replacement|string|true|none|none|
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

