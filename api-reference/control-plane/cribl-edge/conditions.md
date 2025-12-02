
<h1 id="cribl-edge-api-conditions">Cribl Edge API - Conditions v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-conditions-conditions">Conditions</h1>

## Get a list of Condition objects

<a id="opIdlistCondition"></a>

> Code samples

`GET /conditions`

Get a list of Condition objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "__conf": {},
      "__filename": "string",
      "disabled": true,
      "group": "string",
      "id": "string",
      "initTime": 0,
      "loadTime": 0,
      "modTime": 0,
      "name": "string",
      "uischema": {},
      "version": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-condition-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Condition objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-condition-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Function](#schemafunction)]|false|none|none|
|»» __conf|object|true|none|none|
|»» __filename|string|true|none|none|
|»» disabled|boolean|true|none|none|
|»» group|string|true|none|none|
|»» id|string|true|none|none|
|»» initTime|number|true|none|none|
|»» loadTime|number|true|none|none|
|»» modTime|number|true|none|none|
|»» name|string|true|none|none|
|»» uischema|object|true|none|none|
|»» version|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get Condition by ID

<a id="opIdgetConditionById"></a>

> Code samples

`GET /conditions/{id}`

Get Condition by ID

<h3 id="get-condition-by-id-parameters">Parameters</h3>

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
      "__conf": {},
      "__filename": "string",
      "disabled": true,
      "group": "string",
      "id": "string",
      "initTime": 0,
      "loadTime": 0,
      "modTime": 0,
      "name": "string",
      "uischema": {},
      "version": "string"
    }
  ]
}
```

<h3 id="get-condition-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Condition objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-condition-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Function](#schemafunction)]|false|none|none|
|»» __conf|object|true|none|none|
|»» __filename|string|true|none|none|
|»» disabled|boolean|true|none|none|
|»» group|string|true|none|none|
|»» id|string|true|none|none|
|»» initTime|number|true|none|none|
|»» loadTime|number|true|none|none|
|»» modTime|number|true|none|none|
|»» name|string|true|none|none|
|»» uischema|object|true|none|none|
|»» version|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Condition">Condition</h2>
<!-- backwards compatibility -->
<a id="schemacondition"></a>
<a id="schema_Condition"></a>
<a id="tocScondition"></a>
<a id="tocscondition"></a>

```json
{
  "__conf": {},
  "__filename": "string",
  "disabled": true,
  "group": "string",
  "id": "string",
  "initTime": 0,
  "loadTime": 0,
  "modTime": 0,
  "name": "string",
  "uischema": {},
  "version": "string"
}

```

### Properties

*None*

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

<h2 id="tocS_Function">Function</h2>
<!-- backwards compatibility -->
<a id="schemafunction"></a>
<a id="schema_Function"></a>
<a id="tocSfunction"></a>
<a id="tocsfunction"></a>

```json
{
  "__conf": {},
  "__filename": "string",
  "disabled": true,
  "group": "string",
  "id": "string",
  "initTime": 0,
  "loadTime": 0,
  "modTime": 0,
  "name": "string",
  "uischema": {},
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|__conf|object|true|none|none|
|__filename|string|true|none|none|
|disabled|boolean|true|none|none|
|group|string|true|none|none|
|id|string|true|none|none|
|initTime|number|true|none|none|
|loadTime|number|true|none|none|
|modTime|number|true|none|none|
|name|string|true|none|none|
|uischema|object|true|none|none|
|version|string|true|none|none|

