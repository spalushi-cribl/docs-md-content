
<h1 id="cribl-core-api-access-management-policies-">Cribl Core API - Access Management (Policies) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-access-management-policies--access-management-policies-">Access Management (Policies)</h1>

## Get a list of PolicyRule objects

<a id="opIdlistPolicyRule"></a>

> Code samples

`GET /system/policies`

Get a list of PolicyRule objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "args": [
        "string"
      ],
      "description": "string",
      "id": "string",
      "template": [
        "string"
      ],
      "title": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-policyrule-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PolicyRule objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-policyrule-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PolicyRule](#schemapolicyrule)]|false|none|none|
|»» args|[string]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» template|[string]|true|none|none|
|»» title|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create PolicyRule

<a id="opIdcreatePolicyRule"></a>

> Code samples

`POST /system/policies`

Create PolicyRule

> Body parameter

```json
{
  "args": [
    "string"
  ],
  "description": "string",
  "id": "string",
  "template": [
    "string"
  ],
  "title": "string"
}
```

<h3 id="create-policyrule-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[PolicyRule](#schemapolicyrule)|true|New PolicyRule object|
|» args|body|[string]|false|none|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» template|body|[string]|true|none|
|» title|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "args": [
        "string"
      ],
      "description": "string",
      "id": "string",
      "template": [
        "string"
      ],
      "title": "string"
    }
  ]
}
```

<h3 id="create-policyrule-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PolicyRule objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-policyrule-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PolicyRule](#schemapolicyrule)]|false|none|none|
|»» args|[string]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» template|[string]|true|none|none|
|»» title|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete PolicyRule

<a id="opIddeletePolicyRuleById"></a>

> Code samples

`DELETE /system/policies/{id}`

Delete PolicyRule

<h3 id="delete-policyrule-parameters">Parameters</h3>

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
      "args": [
        "string"
      ],
      "description": "string",
      "id": "string",
      "template": [
        "string"
      ],
      "title": "string"
    }
  ]
}
```

<h3 id="delete-policyrule-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PolicyRule objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-policyrule-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PolicyRule](#schemapolicyrule)]|false|none|none|
|»» args|[string]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» template|[string]|true|none|none|
|»» title|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get PolicyRule by ID

<a id="opIdgetPolicyRuleById"></a>

> Code samples

`GET /system/policies/{id}`

Get PolicyRule by ID

<h3 id="get-policyrule-by-id-parameters">Parameters</h3>

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
      "args": [
        "string"
      ],
      "description": "string",
      "id": "string",
      "template": [
        "string"
      ],
      "title": "string"
    }
  ]
}
```

<h3 id="get-policyrule-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PolicyRule objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-policyrule-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PolicyRule](#schemapolicyrule)]|false|none|none|
|»» args|[string]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» template|[string]|true|none|none|
|»» title|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update PolicyRule

<a id="opIdupdatePolicyRuleById"></a>

> Code samples

`PATCH /system/policies/{id}`

Update PolicyRule

> Body parameter

```json
{
  "args": [
    "string"
  ],
  "description": "string",
  "id": "string",
  "template": [
    "string"
  ],
  "title": "string"
}
```

<h3 id="update-policyrule-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[PolicyRule](#schemapolicyrule)|true|PolicyRule object to be updated|
|» args|body|[string]|false|none|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» template|body|[string]|true|none|
|» title|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "args": [
        "string"
      ],
      "description": "string",
      "id": "string",
      "template": [
        "string"
      ],
      "title": "string"
    }
  ]
}
```

<h3 id="update-policyrule-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PolicyRule objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-policyrule-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PolicyRule](#schemapolicyrule)]|false|none|none|
|»» args|[string]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» template|[string]|true|none|none|
|»» title|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_PolicyRule">PolicyRule</h2>
<!-- backwards compatibility -->
<a id="schemapolicyrule"></a>
<a id="schema_PolicyRule"></a>
<a id="tocSpolicyrule"></a>
<a id="tocspolicyrule"></a>

```json
{
  "args": [
    "string"
  ],
  "description": "string",
  "id": "string",
  "template": [
    "string"
  ],
  "title": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|args|[string]|false|none|none|
|description|string|false|none|none|
|id|string|true|none|none|
|template|[string]|true|none|none|
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

