
<h1 id="cribl-core-api-packs-variables-">Cribl Core API - Packs (Variables) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-packs-variables--packs-variables-">Packs (Variables)</h1>

## List all GlobalVars with references within a Pack

<a id="opIdgetGlobalVariableLibVarsByPack"></a>

> Code samples

`GET /p/{pack}/lib/vars`

List all GlobalVars with references within the specified Pack.

<h3 id="list-all-globalvars-with-references-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|with|query|string|false|Pass "refs" to include references to fields the variable is used in|
|pack|path|string|true|pack ID to GET|

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
      "type": "string",
      "value": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="list-all-globalvars-with-references-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Global Variable objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="list-all-globalvars-with-references-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GlobalVar](#schemaglobalvar)]|false|none|none|
|»» id|string|true|none|Global variable name|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» type|string|true|none|none|
|»» value|string|false|none|none|
|»» tags|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|string|
|type|number|
|type|encryptedString|
|type|boolean|
|type|array|
|type|object|
|type|expression|
|type|any|

<aside class="success">
This operation does not require authentication
</aside>

## Create Global Variable within a Pack

<a id="opIdcreateGlobalVariableLibVarsByPack"></a>

> Code samples

`POST /p/{pack}/lib/vars`

Create Global Variable within the specified Pack.

> Body parameter

```json
{
  "id": "string",
  "lib": "string",
  "description": "string",
  "type": "string",
  "value": "string",
  "tags": "string"
}
```

<h3 id="create-global-variable-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|pack|path|string|true|pack ID to POST|
|body|body|[GlobalVar](#schemaglobalvar)|true|New Global Variable object|
|» id|body|string|true|Global variable name|
|» lib|body|string|false|none|
|» description|body|string|false|none|
|» type|body|string|true|none|
|» value|body|string|false|none|
|» tags|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» type|string|
|» type|number|
|» type|encryptedString|
|» type|boolean|
|» type|array|
|» type|object|
|» type|expression|
|» type|any|

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
      "type": "string",
      "value": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="create-global-variable-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Global Variable objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-global-variable-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GlobalVar](#schemaglobalvar)]|false|none|none|
|»» id|string|true|none|Global variable name|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» type|string|true|none|none|
|»» value|string|false|none|none|
|»» tags|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|string|
|type|number|
|type|encryptedString|
|type|boolean|
|type|array|
|type|object|
|type|expression|
|type|any|

<aside class="success">
This operation does not require authentication
</aside>

## Delete Global Variable within a Pack

<a id="opIddeleteGlobalVariableLibVarsByPackAndId"></a>

> Code samples

`DELETE /p/{pack}/lib/vars/{id}`

Delete Global Variable within the specified Pack.

<h3 id="delete-global-variable-within-a-pack-parameters">Parameters</h3>

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
      "lib": "string",
      "description": "string",
      "type": "string",
      "value": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="delete-global-variable-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Global Variable objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-global-variable-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GlobalVar](#schemaglobalvar)]|false|none|none|
|»» id|string|true|none|Global variable name|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» type|string|true|none|none|
|»» value|string|false|none|none|
|»» tags|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|string|
|type|number|
|type|encryptedString|
|type|boolean|
|type|array|
|type|object|
|type|expression|
|type|any|

<aside class="success">
This operation does not require authentication
</aside>

## Get Global Variable by ID within a Pack

<a id="opIdgetGlobalVariableLibVarsByPackAndId"></a>

> Code samples

`GET /p/{pack}/lib/vars/{id}`

Get Global Variable by ID within the specified Pack.

<h3 id="get-global-variable-by-id-within-a-pack-parameters">Parameters</h3>

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
      "lib": "string",
      "description": "string",
      "type": "string",
      "value": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="get-global-variable-by-id-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Global Variable objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-global-variable-by-id-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GlobalVar](#schemaglobalvar)]|false|none|none|
|»» id|string|true|none|Global variable name|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» type|string|true|none|none|
|»» value|string|false|none|none|
|»» tags|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|string|
|type|number|
|type|encryptedString|
|type|boolean|
|type|array|
|type|object|
|type|expression|
|type|any|

<aside class="success">
This operation does not require authentication
</aside>

## Update Global Variable within a Pack

<a id="opIdupdateGlobalVariableLibVarsByPackAndId"></a>

> Code samples

`PATCH /p/{pack}/lib/vars/{id}`

Update Global Variable within the specified Pack.

> Body parameter

```json
{
  "id": "string",
  "lib": "string",
  "description": "string",
  "type": "string",
  "value": "string",
  "tags": "string"
}
```

<h3 id="update-global-variable-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|pack|path|string|true|pack ID to PATCH|
|body|body|[GlobalVar](#schemaglobalvar)|true|Global Variable object to be updated|
|» id|body|string|true|Global variable name|
|» lib|body|string|false|none|
|» description|body|string|false|none|
|» type|body|string|true|none|
|» value|body|string|false|none|
|» tags|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» type|string|
|» type|number|
|» type|encryptedString|
|» type|boolean|
|» type|array|
|» type|object|
|» type|expression|
|» type|any|

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
      "type": "string",
      "value": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="update-global-variable-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Global Variable objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-global-variable-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GlobalVar](#schemaglobalvar)]|false|none|none|
|»» id|string|true|none|Global variable name|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» type|string|true|none|none|
|»» value|string|false|none|none|
|»» tags|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|string|
|type|number|
|type|encryptedString|
|type|boolean|
|type|array|
|type|object|
|type|expression|
|type|any|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_GlobalVar">GlobalVar</h2>
<!-- backwards compatibility -->
<a id="schemaglobalvar"></a>
<a id="schema_GlobalVar"></a>
<a id="tocSglobalvar"></a>
<a id="tocsglobalvar"></a>

```json
{
  "id": "string",
  "lib": "string",
  "description": "string",
  "type": "string",
  "value": "string",
  "tags": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|Global variable name|
|lib|string|false|none|none|
|description|string|false|none|none|
|type|string|true|none|none|
|value|string|false|none|none|
|tags|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|string|
|type|number|
|type|encryptedString|
|type|boolean|
|type|array|
|type|object|
|type|expression|
|type|any|

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

