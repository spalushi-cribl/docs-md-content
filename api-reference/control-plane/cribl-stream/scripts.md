
<h1 id="cribl-stream-api-scripts">Cribl Stream API - Scripts v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-scripts-scripts">Scripts</h1>

## Get a list of Script objects

<a id="opIdlistScript"></a>

> Code samples

`GET /system/scripts`

Get a list of Script objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "command": "string",
      "description": "string",
      "args": [
        "string"
      ],
      "env": {
        "property1": "string",
        "property2": "string"
      }
    }
  ]
}
```

<h3 id="get-a-list-of-script-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Script objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-script-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ScriptLibEntry](#schemascriptlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» command|string|true|none|Command to execute for this script|
|»» description|string|false|none|none|
|»» args|[string]|false|none|Arguments to pass when executing this script|
|»» env|object|false|none|Extra environment variables to set when executing script|
|»»» **additionalProperties**|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create Script

<a id="opIdcreateScript"></a>

> Code samples

`POST /system/scripts`

Create Script

> Body parameter

```json
{
  "id": "string",
  "command": "string",
  "description": "string",
  "args": [
    "string"
  ],
  "env": {
    "property1": "string",
    "property2": "string"
  }
}
```

<h3 id="create-script-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[ScriptLibEntry](#schemascriptlibentry)|true|New Script object|
|» id|body|string|true|none|
|» command|body|string|true|Command to execute for this script|
|» description|body|string|false|none|
|» args|body|[string]|false|Arguments to pass when executing this script|
|» env|body|object|false|Extra environment variables to set when executing script|
|»» **additionalProperties**|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "command": "string",
      "description": "string",
      "args": [
        "string"
      ],
      "env": {
        "property1": "string",
        "property2": "string"
      }
    }
  ]
}
```

<h3 id="create-script-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Script objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-script-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ScriptLibEntry](#schemascriptlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» command|string|true|none|Command to execute for this script|
|»» description|string|false|none|none|
|»» args|[string]|false|none|Arguments to pass when executing this script|
|»» env|object|false|none|Extra environment variables to set when executing script|
|»»» **additionalProperties**|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete Script

<a id="opIddeleteScriptById"></a>

> Code samples

`DELETE /system/scripts/{id}`

Delete Script

<h3 id="delete-script-parameters">Parameters</h3>

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
      "command": "string",
      "description": "string",
      "args": [
        "string"
      ],
      "env": {
        "property1": "string",
        "property2": "string"
      }
    }
  ]
}
```

<h3 id="delete-script-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Script objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-script-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ScriptLibEntry](#schemascriptlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» command|string|true|none|Command to execute for this script|
|»» description|string|false|none|none|
|»» args|[string]|false|none|Arguments to pass when executing this script|
|»» env|object|false|none|Extra environment variables to set when executing script|
|»»» **additionalProperties**|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get Script by ID

<a id="opIdgetScriptById"></a>

> Code samples

`GET /system/scripts/{id}`

Get Script by ID

<h3 id="get-script-by-id-parameters">Parameters</h3>

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
      "command": "string",
      "description": "string",
      "args": [
        "string"
      ],
      "env": {
        "property1": "string",
        "property2": "string"
      }
    }
  ]
}
```

<h3 id="get-script-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Script objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-script-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ScriptLibEntry](#schemascriptlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» command|string|true|none|Command to execute for this script|
|»» description|string|false|none|none|
|»» args|[string]|false|none|Arguments to pass when executing this script|
|»» env|object|false|none|Extra environment variables to set when executing script|
|»»» **additionalProperties**|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update Script

<a id="opIdupdateScriptById"></a>

> Code samples

`PATCH /system/scripts/{id}`

Update Script

> Body parameter

```json
{
  "id": "string",
  "command": "string",
  "description": "string",
  "args": [
    "string"
  ],
  "env": {
    "property1": "string",
    "property2": "string"
  }
}
```

<h3 id="update-script-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[ScriptLibEntry](#schemascriptlibentry)|true|Script object to be updated|
|» id|body|string|true|none|
|» command|body|string|true|Command to execute for this script|
|» description|body|string|false|none|
|» args|body|[string]|false|Arguments to pass when executing this script|
|» env|body|object|false|Extra environment variables to set when executing script|
|»» **additionalProperties**|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "command": "string",
      "description": "string",
      "args": [
        "string"
      ],
      "env": {
        "property1": "string",
        "property2": "string"
      }
    }
  ]
}
```

<h3 id="update-script-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Script objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-script-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ScriptLibEntry](#schemascriptlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» command|string|true|none|Command to execute for this script|
|»» description|string|false|none|none|
|»» args|[string]|false|none|Arguments to pass when executing this script|
|»» env|object|false|none|Extra environment variables to set when executing script|
|»»» **additionalProperties**|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_ScriptLibEntry">ScriptLibEntry</h2>
<!-- backwards compatibility -->
<a id="schemascriptlibentry"></a>
<a id="schema_ScriptLibEntry"></a>
<a id="tocSscriptlibentry"></a>
<a id="tocsscriptlibentry"></a>

```json
{
  "id": "string",
  "command": "string",
  "description": "string",
  "args": [
    "string"
  ],
  "env": {
    "property1": "string",
    "property2": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|command|string|true|none|Command to execute for this script|
|description|string|false|none|none|
|args|[string]|false|none|Arguments to pass when executing this script|
|env|object|false|none|Extra environment variables to set when executing script|
|» **additionalProperties**|string|false|none|none|

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

