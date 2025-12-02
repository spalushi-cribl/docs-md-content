
<h1 id="cribl-core-api-feature-management">Cribl Core API - Feature Management v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-feature-management-feature-management">Feature Management</h1>

## List all features

<a id="opIdgetFeaturesEntry"></a>

> Code samples

`GET /settings/features`

List all features

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "disabled": true,
      "id": "string"
    }
  ]
}
```

<h3 id="list-all-features-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of FeaturesEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="list-all-features-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[FeaturesEntry](#schemafeaturesentry)]|false|none|none|
|»» disabled|boolean|true|none|none|
|»» id|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get feature by id (i.e. 'type-name`)

<a id="opIdgetFeaturesEntryById"></a>

> Code samples

`GET /settings/features/{id}`

Get feature by id (i.e. 'type-name`)

<h3 id="get-feature-by-id-(i.e.-'type-name`)-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Feature id|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "disabled": true,
      "id": "string"
    }
  ]
}
```

<h3 id="get-feature-by-id-(i.e.-'type-name`)-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of FeaturesEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-feature-by-id-(i.e.-'type-name`)-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[FeaturesEntry](#schemafeaturesentry)]|false|none|none|
|»» disabled|boolean|true|none|none|
|»» id|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_FeaturesEntry">FeaturesEntry</h2>
<!-- backwards compatibility -->
<a id="schemafeaturesentry"></a>
<a id="schema_FeaturesEntry"></a>
<a id="tocSfeaturesentry"></a>
<a id="tocsfeaturesentry"></a>

```json
{
  "disabled": true,
  "id": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|disabled|boolean|true|none|none|
|id|string|true|none|none|

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

