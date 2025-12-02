
<h1 id="cribl-stream-api-parquet-schemas">Cribl Stream API - Parquet Schemas v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-parquet-schemas-parquet-schemas">Parquet Schemas</h1>

## Get a list of Schema objects

<a id="opIdlistSchema"></a>

> Code samples

`GET /lib/parquet-schemas`

Get a list of Schema objects

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

<h3 id="get-a-list-of-schema-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Schema objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-schema-objects-responseschema">Response Schema</h3>

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

## Create Schema

<a id="opIdcreateSchema"></a>

> Code samples

`POST /lib/parquet-schemas`

Create Schema

> Body parameter

```json
{
  "id": "string",
  "description": "string",
  "schema": "string"
}
```

<h3 id="create-schema-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
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

<h3 id="create-schema-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Schema objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-schema-responseschema">Response Schema</h3>

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

## Delete Schema

<a id="opIddeleteSchemaById"></a>

> Code samples

`DELETE /lib/parquet-schemas/{id}`

Delete Schema

<h3 id="delete-schema-parameters">Parameters</h3>

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
      "id": "string",
      "description": "string",
      "schema": "string"
    }
  ]
}
```

<h3 id="delete-schema-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Schema objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-schema-responseschema">Response Schema</h3>

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

## Update Schema

<a id="opIdupdateSchemaById"></a>

> Code samples

`PATCH /lib/parquet-schemas/{id}`

Update Schema

> Body parameter

```json
{
  "id": "string",
  "description": "string",
  "schema": "string"
}
```

<h3 id="update-schema-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
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

<h3 id="update-schema-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Schema objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-schema-responseschema">Response Schema</h3>

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

