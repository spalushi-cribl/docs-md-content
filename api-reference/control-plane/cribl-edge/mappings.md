
<h1 id="cribl-edge-api-mappings">Cribl Edge API - Mappings v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-mappings-mappings">Mappings</h1>

## Set a Mapping Ruleset as the active configuration for the specified Cribl product

<a id="opIdcreateAdminProductsMappingsActivateByProduct"></a>

> Code samples

`POST /admin/products/{product}/mappings/activate`

Set a specific Mapping Ruleset as the currently active configuration for the specified Cribl product.

> Body parameter

```json
undefined
```

<h3 id="set-a-mapping-ruleset-as-the-active-configuration-for-the-specified-cribl-product-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|[ProductsCore](#schemaproductscore)|true|Name of the Cribl product to activate the Mapping Ruleset for|
|body|body|[RulesetId](#schemarulesetid)|true|RulesetId object|
|» id|body|string|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string"
    }
  ]
}
```

<h3 id="set-a-mapping-ruleset-as-the-active-configuration-for-the-specified-cribl-product-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The <code>id</code> of the Mapping Ruleset that has been successfully activated|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="set-a-mapping-ruleset-as-the-active-configuration-for-the-specified-cribl-product-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[RulesetId](#schemarulesetid)]|false|none|none|
|»» id|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## List all Mapping Rulesets for the specified Cribl product

<a id="opIdgetAdminProductsMappingsByProduct"></a>

> Code samples

`GET /admin/products/{product}/mappings`

Retrieve a list of all existing Mapping Rulesets for the specified Cribl product.

<h3 id="list-all-mapping-rulesets-for-the-specified-cribl-product-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|[ProductsCore](#schemaproductscore)|true|Name of the Cribl product to list the Mapping Rulesets for|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "conf": {
        "functions": [
          {}
        ]
      },
      "active": true
    }
  ]
}
```

<h3 id="list-all-mapping-rulesets-for-the-specified-cribl-product-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of MappingRuleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="list-all-mapping-rulesets-for-the-specified-cribl-product-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[MappingRuleset](#schemamappingruleset)]|false|none|none|
|»» id|string|true|none|none|
|»» conf|object|false|none|none|
|»»» functions|[object]|false|none|List of functions to pass data through|
|»» active|boolean|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create a new Mapping Ruleset for the specified Cribl product

<a id="opIdcreateAdminProductsMappingsByProduct"></a>

> Code samples

`POST /admin/products/{product}/mappings`

Create and save a new Mapping Ruleset for the specified Cribl product.

> Body parameter

```json
undefined
```

<h3 id="create-a-new-mapping-ruleset-for-the-specified-cribl-product-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|[ProductsCore](#schemaproductscore)|true|Name of the Cribl product to create the Mapping Ruleset for|
|body|body|[MappingRuleset](#schemamappingruleset)|true|MappingRuleset object|
|» id|body|string|true|none|
|» conf|body|object|false|none|
|»» functions|body|[object]|false|List of functions to pass data through|
|» active|body|boolean|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "conf": {
        "functions": [
          {}
        ]
      },
      "active": true
    }
  ]
}
```

<h3 id="create-a-new-mapping-ruleset-for-the-specified-cribl-product-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A list containing the newly created Mapping Ruleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-a-new-mapping-ruleset-for-the-specified-cribl-product-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[MappingRuleset](#schemamappingruleset)]|false|none|none|
|»» id|string|true|none|none|
|»» conf|object|false|none|none|
|»»» functions|[object]|false|none|List of functions to pass data through|
|»» active|boolean|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Retrieve a Specific Mapping Ruleset

<a id="opIdgetAdminProductsMappingsByProductAndId"></a>

> Code samples

`GET /admin/products/{product}/mappings/{id}`

Get the details for a single Mapping Ruleset, identified by its id, for a Worker Group or Edge Fleet.

<h3 id="retrieve-a-specific-mapping-ruleset-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|[ProductsCore](#schemaproductscore)|true|Name of the Cribl product to get the Mappings for|
|id|path|string|true|The <code>id</code> of the Worker Group or Edge Fleet Mapping Ruleset to get|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "conf": {
        "functions": [
          {}
        ]
      },
      "active": true
    }
  ]
}
```

<h3 id="retrieve-a-specific-mapping-ruleset-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of MappingRuleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="retrieve-a-specific-mapping-ruleset-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[MappingRuleset](#schemamappingruleset)]|false|none|none|
|»» id|string|true|none|none|
|»» conf|object|false|none|none|
|»»» functions|[object]|false|none|List of functions to pass data through|
|»» active|boolean|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update an existing Mapping Ruleset for a Worker Group or Edge Fleet

<a id="opIdupdateAdminProductsMappingsByProductAndId"></a>

> Code samples

`PATCH /admin/products/{product}/mappings/{id}`

Modify the configuration of the specified Mapping Ruleset for a Worker Group or Edge Fleet.

> Body parameter

```json
undefined
```

<h3 id="update-an-existing-mapping-ruleset-for-a-worker-group-or-edge-fleet-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|[ProductsCore](#schemaproductscore)|true|Name of the Cribl product to update the Mapping Ruleset for|
|id|path|string|true|The <code>id</code> of the Mapping Ruleset to update.|
|body|body|[MappingRuleset](#schemamappingruleset)|true|MappingRuleset object|
|» id|body|string|true|none|
|» conf|body|object|false|none|
|»» functions|body|[object]|false|List of functions to pass data through|
|» active|body|boolean|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "conf": {
        "functions": [
          {}
        ]
      },
      "active": true
    }
  ]
}
```

<h3 id="update-an-existing-mapping-ruleset-for-a-worker-group-or-edge-fleet-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A list containing the updated Mapping Ruleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-an-existing-mapping-ruleset-for-a-worker-group-or-edge-fleet-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[MappingRuleset](#schemamappingruleset)]|false|none|none|
|»» id|string|true|none|none|
|»» conf|object|false|none|none|
|»»» functions|[object]|false|none|List of functions to pass data through|
|»» active|boolean|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete the specified Mapping Ruleset from the Worker Group or Edge Fleet

<a id="opIddeleteAdminProductsMappingsByProductAndId"></a>

> Code samples

`DELETE /admin/products/{product}/mappings/{id}`

Permanently remove the specified Mapping Ruleset from the Worker Group or Edge Fleet.

<h3 id="delete-the-specified-mapping-ruleset-from-the-worker-group-or-edge-fleet-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|[ProductsCore](#schemaproductscore)|true|Name of the Cribl product to delete the Mapping Ruleset for|
|id|path|string|true|The <code>id</code> of the Mapping Ruleset to delete.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "conf": {
        "functions": [
          {}
        ]
      },
      "active": true
    }
  ]
}
```

<h3 id="delete-the-specified-mapping-ruleset-from-the-worker-group-or-edge-fleet-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A list containing the deleted Mapping Ruleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-the-specified-mapping-ruleset-from-the-worker-group-or-edge-fleet-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[MappingRuleset](#schemamappingruleset)]|false|none|none|
|»» id|string|true|none|none|
|»» conf|object|false|none|none|
|»»» functions|[object]|false|none|List of functions to pass data through|
|»» active|boolean|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_RulesetId">RulesetId</h2>
<!-- backwards compatibility -->
<a id="schemarulesetid"></a>
<a id="schema_RulesetId"></a>
<a id="tocSrulesetid"></a>
<a id="tocsrulesetid"></a>

```json
{
  "id": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
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

<h2 id="tocS_ProductsCore">ProductsCore</h2>
<!-- backwards compatibility -->
<a id="schemaproductscore"></a>
<a id="schema_ProductsCore"></a>
<a id="tocSproductscore"></a>
<a id="tocsproductscore"></a>

```json
"stream"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|stream|
|*anonymous*|edge|

<h2 id="tocS_MappingRuleset">MappingRuleset</h2>
<!-- backwards compatibility -->
<a id="schemamappingruleset"></a>
<a id="schema_MappingRuleset"></a>
<a id="tocSmappingruleset"></a>
<a id="tocsmappingruleset"></a>

```json
{
  "id": "string",
  "conf": {
    "functions": [
      {}
    ]
  },
  "active": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|conf|object|false|none|none|
|» functions|[object]|false|none|List of functions to pass data through|
|active|boolean|false|none|none|

