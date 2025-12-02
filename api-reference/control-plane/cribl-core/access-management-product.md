
<h1 id="cribl-core-api-access-management-product-">Cribl Core API - Access Management (Product) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-access-management-product--access-management-product-">Access Management (Product)</h1>

## Get Users belonging to a product

<a id="opIdgetProductsUsersByProduct"></a>

> Code samples

`GET /products/{product}/users`

Get Users belonging to a product

<h3 id="get-users-belonging-to-a-product-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|[ProductsAll](#schemaproductsall)|true|product by which to filter members|
|groupId|query|string|false|filter to specific group by groupId|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|
|product|search|
|product|lake|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "currentPassword": "string",
      "disabled": true,
      "email": "string",
      "first": "string",
      "id": "string",
      "last": "string",
      "password": "string",
      "roles": [
        "string"
      ],
      "teams": [
        "string"
      ],
      "username": "string"
    }
  ]
}
```

<h3 id="get-users-belonging-to-a-product-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of User objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-users-belonging-to-a-product-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[User](#schemauser)]|false|none|none|
|»» currentPassword|string|false|none|none|
|»» disabled|boolean|true|none|none|
|»» email|string|true|none|none|
|»» first|string|true|none|none|
|»» id|string|true|none|none|
|»» last|string|true|none|none|
|»» password|string|false|none|none|
|»» roles|[string]|false|none|none|
|»» teams|[string]|false|none|none|
|»» username|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Invalidate the members cache for a given product in SaaS deployment.

<a id="opIddeleteProductsUsersCacheByProduct"></a>

> Code samples

`DELETE /products/{product}/users/__cache__`

Invalidate the members cache for a given product in SaaS deployment.

<h3 id="invalidate-the-members-cache-for-a-given-product-in-saas-deployment.-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|[ProductsAll](#schemaproductsall)|true|product by which to filter members|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|
|product|search|
|product|lake|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="invalidate-the-members-cache-for-a-given-product-in-saas-deployment.-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="invalidate-the-members-cache-for-a-given-product-in-saas-deployment.-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get user's Access Control List

<a id="opIdgetProductsUsersAclByProductAndId"></a>

> Code samples

`GET /products/{product}/users/{id}/acl`

Get user's Access Control List

<h3 id="get-user's-access-control-list-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|[ProductsAll](#schemaproductsall)|true|product by which to filter members|
|id|path|string|true|user id|
|type|query|[RbacResource](#schemarbacresource)|false|resource type by which to filter access levels|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|
|product|search|
|product|lake|
|type|groups|
|type|datasets|
|type|dataset-providers|
|type|projects|
|type|dashboards|
|type|macros|
|type|notebooks|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "gid": "string",
      "id": "string",
      "policy": "string",
      "type": "groups"
    }
  ]
}
```

<h3 id="get-user's-access-control-list-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ResourcePolicy objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-user's-access-control-list-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ResourcePolicy](#schemaresourcepolicy)]|false|none|none|
|»» gid|string|true|none|none|
|»» id|string|false|none|none|
|»» policy|string|true|none|none|
|»» type|[RbacResource](#schemarbacresource)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|groups|
|type|datasets|
|type|dataset-providers|
|type|projects|
|type|dashboards|
|type|macros|
|type|notebooks|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_User">User</h2>
<!-- backwards compatibility -->
<a id="schemauser"></a>
<a id="schema_User"></a>
<a id="tocSuser"></a>
<a id="tocsuser"></a>

```json
{
  "currentPassword": "string",
  "disabled": true,
  "email": "string",
  "first": "string",
  "id": "string",
  "last": "string",
  "password": "string",
  "roles": [
    "string"
  ],
  "teams": [
    "string"
  ],
  "username": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|currentPassword|string|false|none|none|
|disabled|boolean|true|none|none|
|email|string|true|none|none|
|first|string|true|none|none|
|id|string|true|none|none|
|last|string|true|none|none|
|password|string|false|none|none|
|roles|[string]|false|none|none|
|teams|[string]|false|none|none|
|username|string|true|none|none|

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

<h2 id="tocS_ProductsAll">ProductsAll</h2>
<!-- backwards compatibility -->
<a id="schemaproductsall"></a>
<a id="schema_ProductsAll"></a>
<a id="tocSproductsall"></a>
<a id="tocsproductsall"></a>

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
|*anonymous*|search|
|*anonymous*|lake|

<h2 id="tocS_ResourcePolicy">ResourcePolicy</h2>
<!-- backwards compatibility -->
<a id="schemaresourcepolicy"></a>
<a id="schema_ResourcePolicy"></a>
<a id="tocSresourcepolicy"></a>
<a id="tocsresourcepolicy"></a>

```json
{
  "gid": "string",
  "id": "string",
  "policy": "string",
  "type": "groups"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|gid|string|true|none|none|
|id|string|false|none|none|
|policy|string|true|none|none|
|type|[RbacResource](#schemarbacresource)|true|none|none|

<h2 id="tocS_RbacResource">RbacResource</h2>
<!-- backwards compatibility -->
<a id="schemarbacresource"></a>
<a id="schema_RbacResource"></a>
<a id="tocSrbacresource"></a>
<a id="tocsrbacresource"></a>

```json
"groups"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|groups|
|*anonymous*|datasets|
|*anonymous*|dataset-providers|
|*anonymous*|projects|
|*anonymous*|dashboards|
|*anonymous*|macros|
|*anonymous*|notebooks|

