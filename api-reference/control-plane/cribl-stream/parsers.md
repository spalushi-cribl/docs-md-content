
<h1 id="cribl-stream-api-parsers">Cribl Stream API - Parsers v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-parsers-parsers">Parsers</h1>

## Get a list of Parser objects

<a id="opIdlistParser"></a>

> Code samples

`GET /lib/parsers`

Get a list of Parser objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "lib": "string",
      "description": "string",
      "tags": "string",
      "type": "csv"
    }
  ]
}
```

<h3 id="get-a-list-of-parser-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Parser objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-parser-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ParserLibEntry](#schemaparserlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|Optionally, add tags that you can use for filtering|
|»» type|string|true|none|Parser or formatter type to use|

#### Enumerated Values

|Property|Value|
|---|---|
|type|csv|
|type|elff|
|type|clf|
|type|kvp|
|type|json|
|type|delim|
|type|regex|
|type|grok|

<aside class="success">
This operation does not require authentication
</aside>

## Create Parser

<a id="opIdcreateParser"></a>

> Code samples

`POST /lib/parsers`

Create Parser

> Body parameter

```json
{
  "id": "string",
  "lib": "string",
  "description": "string",
  "tags": "string",
  "type": "csv"
}
```

<h3 id="create-parser-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[ParserLibEntry](#schemaparserlibentry)|true|New Parser object|
|» id|body|string|true|none|
|» lib|body|string|false|none|
|» description|body|string|false|none|
|» tags|body|string|false|Optionally, add tags that you can use for filtering|
|» type|body|string|true|Parser or formatter type to use|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» type|csv|
|» type|elff|
|» type|clf|
|» type|kvp|
|» type|json|
|» type|delim|
|» type|regex|
|» type|grok|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "lib": "string",
      "description": "string",
      "tags": "string",
      "type": "csv"
    }
  ]
}
```

<h3 id="create-parser-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Parser objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-parser-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ParserLibEntry](#schemaparserlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|Optionally, add tags that you can use for filtering|
|»» type|string|true|none|Parser or formatter type to use|

#### Enumerated Values

|Property|Value|
|---|---|
|type|csv|
|type|elff|
|type|clf|
|type|kvp|
|type|json|
|type|delim|
|type|regex|
|type|grok|

<aside class="success">
This operation does not require authentication
</aside>

## Delete Parser

<a id="opIddeleteParserById"></a>

> Code samples

`DELETE /lib/parsers/{id}`

Delete Parser

<h3 id="delete-parser-parameters">Parameters</h3>

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
      "lib": "string",
      "description": "string",
      "tags": "string",
      "type": "csv"
    }
  ]
}
```

<h3 id="delete-parser-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Parser objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-parser-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ParserLibEntry](#schemaparserlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|Optionally, add tags that you can use for filtering|
|»» type|string|true|none|Parser or formatter type to use|

#### Enumerated Values

|Property|Value|
|---|---|
|type|csv|
|type|elff|
|type|clf|
|type|kvp|
|type|json|
|type|delim|
|type|regex|
|type|grok|

<aside class="success">
This operation does not require authentication
</aside>

## Get Parser by ID

<a id="opIdgetParserById"></a>

> Code samples

`GET /lib/parsers/{id}`

Get Parser by ID

<h3 id="get-parser-by-id-parameters">Parameters</h3>

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
      "id": "string",
      "lib": "string",
      "description": "string",
      "tags": "string",
      "type": "csv"
    }
  ]
}
```

<h3 id="get-parser-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Parser objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-parser-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ParserLibEntry](#schemaparserlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|Optionally, add tags that you can use for filtering|
|»» type|string|true|none|Parser or formatter type to use|

#### Enumerated Values

|Property|Value|
|---|---|
|type|csv|
|type|elff|
|type|clf|
|type|kvp|
|type|json|
|type|delim|
|type|regex|
|type|grok|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_ParserLibEntry">ParserLibEntry</h2>
<!-- backwards compatibility -->
<a id="schemaparserlibentry"></a>
<a id="schema_ParserLibEntry"></a>
<a id="tocSparserlibentry"></a>
<a id="tocsparserlibentry"></a>

```json
{
  "id": "string",
  "lib": "string",
  "description": "string",
  "tags": "string",
  "type": "csv"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|lib|string|false|none|none|
|description|string|false|none|none|
|tags|string|false|none|Optionally, add tags that you can use for filtering|
|type|string|true|none|Parser or formatter type to use|

#### Enumerated Values

|Property|Value|
|---|---|
|type|csv|
|type|elff|
|type|clf|
|type|kvp|
|type|json|
|type|delim|
|type|regex|
|type|grok|

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

