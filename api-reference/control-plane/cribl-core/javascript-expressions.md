
<h1 id="cribl-core-api-javascript-expressions">Cribl Core API - JavaScript Expressions v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-javascript-expressions-javascript-expressions">JavaScript Expressions</h1>

## Evaluate JavaScript expression

<a id="opIdcreateLibExpression"></a>

> Code samples

`POST /lib/expression`

Evaluate JavaScript expression

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "context": {},
      "evalType": "string",
      "expr": "string",
      "id": "string",
      "pack": "string",
      "result": {},
      "unprotected": true
    }
  ]
}
```

<h3 id="evaluate-javascript-expression-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ExprLibEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="evaluate-javascript-expression-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ExprLibEntry](#schemaexprlibentry)]|false|none|none|
|»» context|object|false|none|none|
|»» evalType|string|false|none|none|
|»» expr|string|true|none|JavaScript expression to evaluate|
|»» id|string|true|none|none|
|»» pack|string|false|none|none|
|»» result|object|false|none|none|
|»» unprotected|boolean|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_ExprLibEntry">ExprLibEntry</h2>
<!-- backwards compatibility -->
<a id="schemaexprlibentry"></a>
<a id="schema_ExprLibEntry"></a>
<a id="tocSexprlibentry"></a>
<a id="tocsexprlibentry"></a>

```json
{
  "context": {},
  "evalType": "string",
  "expr": "string",
  "id": "string",
  "pack": "string",
  "result": {},
  "unprotected": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|context|object|false|none|none|
|evalType|string|false|none|none|
|expr|string|true|none|JavaScript expression to evaluate|
|id|string|true|none|none|
|pack|string|false|none|none|
|result|object|false|none|none|
|unprotected|boolean|false|none|none|

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

