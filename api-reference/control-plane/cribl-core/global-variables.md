
<h1 id="cribl-core-api-global-variables">Cribl Core API - Global Variables v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-global-variables-global-variables">Global Variables</h1>

## List all GlobalVars with references

<a id="opIdlistGlobalVariable"></a>

> Code samples

`GET /lib/vars`

List all GlobalVars with references

<h3 id="list-all-globalvars-with-references-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|with|query|string|false|Pass "refs" to include references to fields the variable is used in|

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

<h3 id="list-all-globalvars-with-references-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Global Variable objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="list-all-globalvars-with-references-responseschema">Response Schema</h3>

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

## Create Global Variable

<a id="opIdcreateGlobalVariable"></a>

> Code samples

`POST /lib/vars`

Create Global Variable

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

<h3 id="create-global-variable-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
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

<h3 id="create-global-variable-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Global Variable objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-global-variable-responseschema">Response Schema</h3>

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

## Delete Global Variable

<a id="opIddeleteGlobalVariableById"></a>

> Code samples

`DELETE /lib/vars/{id}`

Delete Global Variable

<h3 id="delete-global-variable-parameters">Parameters</h3>

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
      "type": "string",
      "value": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="delete-global-variable-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Global Variable objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-global-variable-responseschema">Response Schema</h3>

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

## Get Global Variable by ID

<a id="opIdgetGlobalVariableById"></a>

> Code samples

`GET /lib/vars/{id}`

Get Global Variable by ID

<h3 id="get-global-variable-by-id-parameters">Parameters</h3>

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
      "type": "string",
      "value": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="get-global-variable-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Global Variable objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-global-variable-by-id-responseschema">Response Schema</h3>

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

## Update Global Variable

<a id="opIdupdateGlobalVariableById"></a>

> Code samples

`PATCH /lib/vars/{id}`

Update Global Variable

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

<h3 id="update-global-variable-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
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

<h3 id="update-global-variable-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Global Variable objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-global-variable-responseschema">Response Schema</h3>

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

