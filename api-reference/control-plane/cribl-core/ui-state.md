
<h1 id="cribl-core-api-ui-state">Cribl Core API - UI State v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-ui-state-ui-state">UI State</h1>

## Get UI state by key

<a id="opIdgetUiByKey"></a>

> Code samples

`GET /ui/{key}`

Get UI state by key

<h3 id="get-ui-state-by-key-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|key|path|string|true|UI state key|

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

<h3 id="get-ui-state-by-key-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-ui-state-by-key-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update UI state by key

<a id="opIdupdateUiByKey"></a>

> Code samples

`PATCH /ui/{key}`

Update UI state by key

> Body parameter

```json
{
  "op": "set",
  "value": "string",
  "args": {}
}
```

<h3 id="update-ui-state-by-key-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|key|path|string|true|UI state key|
|body|body|[UiStatePatch](#schemauistatepatch)|true|UI State Patch object|
|» op|body|string|true|none|
|» value|body|any|true|none|
|»» *anonymous*|body|string|false|none|
|»» *anonymous*|body|object|false|none|
|»» *anonymous*|body|[oneOf]|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|object|false|none|
|» args|body|object|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» op|set|
|» op|push-recent|

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

<h3 id="update-ui-state-by-key-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-ui-state-by-key-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

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

<h2 id="tocS_UiStatePatch">UiStatePatch</h2>
<!-- backwards compatibility -->
<a id="schemauistatepatch"></a>
<a id="schema_UiStatePatch"></a>
<a id="tocSuistatepatch"></a>
<a id="tocsuistatepatch"></a>

```json
{
  "op": "set",
  "value": "string",
  "args": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|op|string|true|none|none|
|value|any|true|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|[oneOf]|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|args|object|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|op|set|
|op|push-recent|

