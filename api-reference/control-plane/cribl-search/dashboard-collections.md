
<h1 id="cribl-search-api-dashboard-collections">Cribl Search API - Dashboard Collections v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-search-api-dashboard-collections-dashboard-collections">Dashboard Collections</h1>

## Get a list of DashboardCategory objects

<a id="opIdlistDashboardCategory"></a>

> Code samples

`GET /search/dashboard-categories`

Get a list of DashboardCategory objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": "string",
      "id": "string",
      "isPack": true,
      "name": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-dashboardcategory-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DashboardCategory objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-dashboardcategory-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DashboardCategory](#schemadashboardcategory)]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» isPack|boolean|false|none|none|
|»» name|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create DashboardCategory

<a id="opIdcreateDashboardCategory"></a>

> Code samples

`POST /search/dashboard-categories`

Create DashboardCategory

> Body parameter

```json
{
  "description": "string",
  "id": "string",
  "isPack": true,
  "name": "string"
}
```

<h3 id="create-dashboardcategory-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[DashboardCategory](#schemadashboardcategory)|true|New DashboardCategory object|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» isPack|body|boolean|false|none|
|» name|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": "string",
      "id": "string",
      "isPack": true,
      "name": "string"
    }
  ]
}
```

<h3 id="create-dashboardcategory-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DashboardCategory objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-dashboardcategory-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DashboardCategory](#schemadashboardcategory)]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» isPack|boolean|false|none|none|
|»» name|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete DashboardCategory

<a id="opIddeleteDashboardCategoryById"></a>

> Code samples

`DELETE /search/dashboard-categories/{id}`

Delete DashboardCategory

<h3 id="delete-dashboardcategory-parameters">Parameters</h3>

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
      "isPack": true,
      "name": "string"
    }
  ]
}
```

<h3 id="delete-dashboardcategory-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DashboardCategory objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-dashboardcategory-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DashboardCategory](#schemadashboardcategory)]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» isPack|boolean|false|none|none|
|»» name|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get DashboardCategory by ID

<a id="opIdgetDashboardCategoryById"></a>

> Code samples

`GET /search/dashboard-categories/{id}`

Get DashboardCategory by ID

<h3 id="get-dashboardcategory-by-id-parameters">Parameters</h3>

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
      "isPack": true,
      "name": "string"
    }
  ]
}
```

<h3 id="get-dashboardcategory-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DashboardCategory objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-dashboardcategory-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DashboardCategory](#schemadashboardcategory)]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» isPack|boolean|false|none|none|
|»» name|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update DashboardCategory

<a id="opIdupdateDashboardCategoryById"></a>

> Code samples

`PATCH /search/dashboard-categories/{id}`

Update DashboardCategory

> Body parameter

```json
{
  "description": "string",
  "id": "string",
  "isPack": true,
  "name": "string"
}
```

<h3 id="update-dashboardcategory-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[DashboardCategory](#schemadashboardcategory)|true|DashboardCategory object to be updated|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» isPack|body|boolean|false|none|
|» name|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": "string",
      "id": "string",
      "isPack": true,
      "name": "string"
    }
  ]
}
```

<h3 id="update-dashboardcategory-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DashboardCategory objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-dashboardcategory-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DashboardCategory](#schemadashboardcategory)]|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» isPack|boolean|false|none|none|
|»» name|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_DashboardCategory">DashboardCategory</h2>
<!-- backwards compatibility -->
<a id="schemadashboardcategory"></a>
<a id="schema_DashboardCategory"></a>
<a id="tocSdashboardcategory"></a>
<a id="tocsdashboardcategory"></a>

```json
{
  "description": "string",
  "id": "string",
  "isPack": true,
  "name": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|description|string|false|none|none|
|id|string|true|none|none|
|isPack|boolean|false|none|none|
|name|string|true|none|none|

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

