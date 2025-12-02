
<h1 id="cribl-core-api-access-management-permissions-">Cribl Core API - Access Management (Permissions) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-access-management-permissions--access-management-permissions-">Access Management (Permissions)</h1>

## get the client's authorization policy

<a id="opIdgetAuthorizePolicy"></a>

> Code samples

`GET /authorize/policy`

get the client's authorization policy

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ]
}
```

<h3 id="get-the-client's-authorization-policy-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of AuthPolicyEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-client's-authorization-policy-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[AuthPolicyEntry](#schemaauthpolicyentry)]|false|none|none|
|»» actions|[string]|true|none|none|
|»» object|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## get the client's roles

<a id="opIdgetAuthorizeRoles"></a>

> Code samples

`GET /authorize/roles`

get the client's roles

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    "string"
  ]
}
```

<h3 id="get-the-client's-roles-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of string objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-client's-roles-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_AuthPolicyEntry">AuthPolicyEntry</h2>
<!-- backwards compatibility -->
<a id="schemaauthpolicyentry"></a>
<a id="schema_AuthPolicyEntry"></a>
<a id="tocSauthpolicyentry"></a>
<a id="tocsauthpolicyentry"></a>

```json
{
  "actions": [
    "string"
  ],
  "object": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|actions|[string]|true|none|none|
|object|string|true|none|none|

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

