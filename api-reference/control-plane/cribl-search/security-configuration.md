
<h1 id="cribl-search-api-security-configuration">Cribl Search API - Security Configuration v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-search-api-security-configuration-security-configuration">Security Configuration</h1>

## Get a list of TrustPolicy objects

<a id="opIdlistTrustPolicy"></a>

> Code samples

`GET /search/trust-policies`

Get a list of TrustPolicy objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "policy": {
        "Statement": [
          {
            "Action": "string",
            "Condition": {
              "StringEquals": {}
            },
            "Effect": "string",
            "Principal": {
              "AWS": "string"
            }
          }
        ],
        "Version": "string"
      }
    }
  ]
}
```

<h3 id="get-a-list-of-trustpolicy-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of TrustPolicy objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-trustpolicy-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[TrustPolicy](#schematrustpolicy)]|false|none|none|
|»» id|string|true|none|none|
|»» policy|[AMTrustPolicy](#schemaamtrustpolicy)|true|none|none|
|»»» Statement|[object]|true|none|none|
|»»»» Action|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|[string]|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» Condition|object|false|none|none|
|»»»»» StringEquals|object|false|none|none|
|»»»» Effect|string|true|none|none|
|»»»» Principal|object|true|none|none|
|»»»»» AWS|string|true|none|none|
|»»» Version|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_TrustPolicy">TrustPolicy</h2>
<!-- backwards compatibility -->
<a id="schematrustpolicy"></a>
<a id="schema_TrustPolicy"></a>
<a id="tocStrustpolicy"></a>
<a id="tocstrustpolicy"></a>

```json
{
  "id": "string",
  "policy": {
    "Statement": [
      {
        "Action": "string",
        "Condition": {
          "StringEquals": {}
        },
        "Effect": "string",
        "Principal": {
          "AWS": "string"
        }
      }
    ],
    "Version": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|policy|[AMTrustPolicy](#schemaamtrustpolicy)|true|none|none|

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

<h2 id="tocS_AMTrustPolicy">AMTrustPolicy</h2>
<!-- backwards compatibility -->
<a id="schemaamtrustpolicy"></a>
<a id="schema_AMTrustPolicy"></a>
<a id="tocSamtrustpolicy"></a>
<a id="tocsamtrustpolicy"></a>

```json
{
  "Statement": [
    {
      "Action": "string",
      "Condition": {
        "StringEquals": {}
      },
      "Effect": "string",
      "Principal": {
        "AWS": "string"
      }
    }
  ],
  "Version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Statement|[object]|true|none|none|
|» Action|any|true|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[string]|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» Condition|object|false|none|none|
|»» StringEquals|object|false|none|none|
|» Effect|string|true|none|none|
|» Principal|object|true|none|none|
|»» AWS|string|true|none|none|
|Version|string|true|none|none|

