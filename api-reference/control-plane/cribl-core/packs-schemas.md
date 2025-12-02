
<h1 id="cribl-core-api-packs-schemas-">Cribl Core API - Packs (Schemas) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-packs-schemas--packs-schemas-">Packs (Schemas)</h1>

## Get a list of Schema objects within a Pack

<a id="opIdgetSchemaLibByPack"></a>

> Code samples

`GET /p/{pack}/lib/schemas`

Get a list of Schema objects within the specified Pack.

<h3 id="get-a-list-of-schema-objects-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|pack|path|string|true|pack ID to GET|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "description": "string",
      "schema": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-schema-objects-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Schema objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-schema-objects-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SchemaLibEntry](#schemaschemalibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» schema|string|true|none|JSON schema matching standards of draft version 2019-09|

<aside class="success">
This operation does not require authentication
</aside>

## Create Schema within a Pack

<a id="opIdcreateSchemaLibByPack"></a>

> Code samples

`POST /p/{pack}/lib/schemas`

Create Schema within the specified Pack.

> Body parameter

```json
{
  "id": "string",
  "description": "string",
  "schema": "string"
}
```

<h3 id="create-schema-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|pack|path|string|true|pack ID to POST|
|body|body|[SchemaLibEntry](#schemaschemalibentry)|true|New Schema object|
|» id|body|string|true|none|
|» description|body|string|false|none|
|» schema|body|string|true|JSON schema matching standards of draft version 2019-09|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "description": "string",
      "schema": "string"
    }
  ]
}
```

<h3 id="create-schema-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Schema objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-schema-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SchemaLibEntry](#schemaschemalibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» schema|string|true|none|JSON schema matching standards of draft version 2019-09|

<aside class="success">
This operation does not require authentication
</aside>

## Delete Schema within a Pack

<a id="opIddeleteSchemaLibByPackAndId"></a>

> Code samples

`DELETE /p/{pack}/lib/schemas/{id}`

Delete Schema within the specified Pack.

<h3 id="delete-schema-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to DELETE|
|pack|path|string|true|pack ID to DELETE|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "description": "string",
      "schema": "string"
    }
  ]
}
```

<h3 id="delete-schema-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Schema objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-schema-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SchemaLibEntry](#schemaschemalibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» schema|string|true|none|JSON schema matching standards of draft version 2019-09|

<aside class="success">
This operation does not require authentication
</aside>

## Get Schema by ID within a Pack

<a id="opIdgetSchemaLibByPackAndId"></a>

> Code samples

`GET /p/{pack}/lib/schemas/{id}`

Get Schema by ID within the specified Pack.

<h3 id="get-schema-by-id-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to GET|
|pack|path|string|true|pack ID to GET|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "description": "string",
      "schema": "string"
    }
  ]
}
```

<h3 id="get-schema-by-id-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Schema objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-schema-by-id-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SchemaLibEntry](#schemaschemalibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» schema|string|true|none|JSON schema matching standards of draft version 2019-09|

<aside class="success">
This operation does not require authentication
</aside>

## Update Schema within a Pack

<a id="opIdupdateSchemaLibByPackAndId"></a>

> Code samples

`PATCH /p/{pack}/lib/schemas/{id}`

Update Schema within the specified Pack.

> Body parameter

```json
{
  "id": "string",
  "description": "string",
  "schema": "string"
}
```

<h3 id="update-schema-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|pack|path|string|true|pack ID to PATCH|
|body|body|[SchemaLibEntry](#schemaschemalibentry)|true|Schema object to be updated|
|» id|body|string|true|none|
|» description|body|string|false|none|
|» schema|body|string|true|JSON schema matching standards of draft version 2019-09|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "description": "string",
      "schema": "string"
    }
  ]
}
```

<h3 id="update-schema-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Schema objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-schema-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SchemaLibEntry](#schemaschemalibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» schema|string|true|none|JSON schema matching standards of draft version 2019-09|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_SchemaLibEntry">SchemaLibEntry</h2>
<!-- backwards compatibility -->
<a id="schemaschemalibentry"></a>
<a id="schema_SchemaLibEntry"></a>
<a id="tocSschemalibentry"></a>
<a id="tocsschemalibentry"></a>

```json
{
  "id": "string",
  "description": "string",
  "schema": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|description|string|false|none|none|
|schema|string|true|none|JSON schema matching standards of draft version 2019-09|

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

