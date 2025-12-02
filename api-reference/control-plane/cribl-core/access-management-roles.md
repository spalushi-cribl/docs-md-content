
<h1 id="cribl-core-api-access-management-roles-">Cribl Core API - Access Management (Roles) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-access-management-roles--access-management-roles-">Access Management (Roles)</h1>

## Get a list of Role objects

<a id="opIdlistRole"></a>

> Code samples

`GET /system/roles`

Get a list of Role objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": "string",
      "id": "string",
      "policy": [
        "string"
      ],
      "tags": [
        "string"
      ],
      "title": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-role-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Role objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-role-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Role](#schemarole)]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» policy|[string]|true|none|none|
|»» tags|[string]|false|none|none|
|»» title|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create Role

<a id="opIdcreateRole"></a>

> Code samples

`POST /system/roles`

Create Role

> Body parameter

```json
{
  "description": "string",
  "id": "string",
  "policy": [
    "string"
  ],
  "tags": [
    "string"
  ],
  "title": "string"
}
```

<h3 id="create-role-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[Role](#schemarole)|true|New Role object|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» policy|body|[string]|true|none|
|» tags|body|[string]|false|none|
|» title|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": "string",
      "id": "string",
      "policy": [
        "string"
      ],
      "tags": [
        "string"
      ],
      "title": "string"
    }
  ]
}
```

<h3 id="create-role-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Role objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-role-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Role](#schemarole)]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» policy|[string]|true|none|none|
|»» tags|[string]|false|none|none|
|»» title|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete Role

<a id="opIddeleteRoleById"></a>

> Code samples

`DELETE /system/roles/{id}`

Delete Role

<h3 id="delete-role-parameters">Parameters</h3>

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
      "description": "string",
      "id": "string",
      "policy": [
        "string"
      ],
      "tags": [
        "string"
      ],
      "title": "string"
    }
  ]
}
```

<h3 id="delete-role-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Role objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-role-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Role](#schemarole)]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» policy|[string]|true|none|none|
|»» tags|[string]|false|none|none|
|»» title|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get Role by ID

<a id="opIdgetRoleById"></a>

> Code samples

`GET /system/roles/{id}`

Get Role by ID

<h3 id="get-role-by-id-parameters">Parameters</h3>

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
      "description": "string",
      "id": "string",
      "policy": [
        "string"
      ],
      "tags": [
        "string"
      ],
      "title": "string"
    }
  ]
}
```

<h3 id="get-role-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Role objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-role-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Role](#schemarole)]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» policy|[string]|true|none|none|
|»» tags|[string]|false|none|none|
|»» title|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update Role

<a id="opIdupdateRoleById"></a>

> Code samples

`PATCH /system/roles/{id}`

Update Role

> Body parameter

```json
{
  "description": "string",
  "id": "string",
  "policy": [
    "string"
  ],
  "tags": [
    "string"
  ],
  "title": "string"
}
```

<h3 id="update-role-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[Role](#schemarole)|true|Role object to be updated|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» policy|body|[string]|true|none|
|» tags|body|[string]|false|none|
|» title|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": "string",
      "id": "string",
      "policy": [
        "string"
      ],
      "tags": [
        "string"
      ],
      "title": "string"
    }
  ]
}
```

<h3 id="update-role-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Role objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-role-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Role](#schemarole)]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» policy|[string]|true|none|none|
|»» tags|[string]|false|none|none|
|»» title|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Role">Role</h2>
<!-- backwards compatibility -->
<a id="schemarole"></a>
<a id="schema_Role"></a>
<a id="tocSrole"></a>
<a id="tocsrole"></a>

```json
{
  "description": "string",
  "id": "string",
  "policy": [
    "string"
  ],
  "tags": [
    "string"
  ],
  "title": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|description|string|false|none|none|
|id|string|true|none|none|
|policy|[string]|true|none|none|
|tags|[string]|false|none|none|
|title|string|false|none|none|

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

