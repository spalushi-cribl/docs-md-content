
<h1 id="cribl-core-api-authentication-multi-factor-">Cribl Core API - Authentication (Multi-Factor) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-authentication-multi-factor--authentication-multi-factor-">Authentication (Multi-Factor)</h1>

## Get PIV configuration

<a id="opIdgetAuthMultiFactor"></a>

> Code samples

`GET /auth/multi-factor`

Get PIV configuration

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "accessControlAllowOrigin": "string",
      "allowLogin": true,
      "apiServerUrl": "string",
      "disabled": true,
      "type": "piv",
      "usernameField": "string",
      "usernameRegex": "string"
    }
  ]
}
```

<h3 id="get-piv-configuration-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of MultiFactorAuthSchema objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-piv-configuration-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[MultiFactorAuthSchema](#schemamultifactorauthschema)]|false|none|none|
|»» accessControlAllowOrigin|string|true|none|none|
|»» allowLogin|boolean|true|none|none|
|»» apiServerUrl|string|true|none|none|
|»» disabled|boolean|true|none|none|
|»» type|string|true|none|none|
|»» usernameField|string|true|none|none|
|»» usernameRegex|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|piv|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_MultiFactorAuthSchema">MultiFactorAuthSchema</h2>
<!-- backwards compatibility -->
<a id="schemamultifactorauthschema"></a>
<a id="schema_MultiFactorAuthSchema"></a>
<a id="tocSmultifactorauthschema"></a>
<a id="tocsmultifactorauthschema"></a>

```json
{
  "accessControlAllowOrigin": "string",
  "allowLogin": true,
  "apiServerUrl": "string",
  "disabled": true,
  "type": "piv",
  "usernameField": "string",
  "usernameRegex": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|accessControlAllowOrigin|string|true|none|none|
|allowLogin|boolean|true|none|none|
|apiServerUrl|string|true|none|none|
|disabled|boolean|true|none|none|
|type|string|true|none|none|
|usernameField|string|true|none|none|
|usernameRegex|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|piv|

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

