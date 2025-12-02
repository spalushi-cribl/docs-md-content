
<h1 id="cribl-core-api-regular-expressions">Cribl Core API - Regular Expressions v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-regular-expressions-regular-expressions">Regular Expressions</h1>

## Get a list of RegexLibEntry objects

<a id="opIdlistRegexLibEntry"></a>

> Code samples

`GET /lib/regex`

Get a list of RegexLibEntry objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "lib": "string",
      "description": "string",
      "regex": "string",
      "sampleData": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-regexlibentry-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of RegexLibEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-regexlibentry-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[RegexLibEntry](#schemaregexlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» regex|string|true|none|none|
|»» sampleData|string|false|none|Optionally, paste in sample data to match against this regex|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create RegexLibEntry

<a id="opIdcreateRegexLibEntry"></a>

> Code samples

`POST /lib/regex`

Create RegexLibEntry

> Body parameter

```json
{
  "id": "string",
  "lib": "string",
  "description": "string",
  "regex": "string",
  "sampleData": "string",
  "tags": "string"
}
```

<h3 id="create-regexlibentry-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[RegexLibEntry](#schemaregexlibentry)|true|New RegexLibEntry object|
|» id|body|string|true|none|
|» lib|body|string|false|none|
|» description|body|string|false|none|
|» regex|body|string|true|none|
|» sampleData|body|string|false|Optionally, paste in sample data to match against this regex|
|» tags|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "lib": "string",
      "description": "string",
      "regex": "string",
      "sampleData": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="create-regexlibentry-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of RegexLibEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-regexlibentry-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[RegexLibEntry](#schemaregexlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» regex|string|true|none|none|
|»» sampleData|string|false|none|Optionally, paste in sample data to match against this regex|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete RegexLibEntry

<a id="opIddeleteRegexLibEntryById"></a>

> Code samples

`DELETE /lib/regex/{id}`

Delete RegexLibEntry

<h3 id="delete-regexlibentry-parameters">Parameters</h3>

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
      "id": "string",
      "lib": "string",
      "description": "string",
      "regex": "string",
      "sampleData": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="delete-regexlibentry-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of RegexLibEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-regexlibentry-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[RegexLibEntry](#schemaregexlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» regex|string|true|none|none|
|»» sampleData|string|false|none|Optionally, paste in sample data to match against this regex|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get RegexLibEntry by ID

<a id="opIdgetRegexLibEntryById"></a>

> Code samples

`GET /lib/regex/{id}`

Get RegexLibEntry by ID

<h3 id="get-regexlibentry-by-id-parameters">Parameters</h3>

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
      "id": "string",
      "lib": "string",
      "description": "string",
      "regex": "string",
      "sampleData": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="get-regexlibentry-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of RegexLibEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-regexlibentry-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[RegexLibEntry](#schemaregexlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» regex|string|true|none|none|
|»» sampleData|string|false|none|Optionally, paste in sample data to match against this regex|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update RegexLibEntry

<a id="opIdupdateRegexLibEntryById"></a>

> Code samples

`PATCH /lib/regex/{id}`

Update RegexLibEntry

> Body parameter

```json
{
  "id": "string",
  "lib": "string",
  "description": "string",
  "regex": "string",
  "sampleData": "string",
  "tags": "string"
}
```

<h3 id="update-regexlibentry-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[RegexLibEntry](#schemaregexlibentry)|true|RegexLibEntry object to be updated|
|» id|body|string|true|none|
|» lib|body|string|false|none|
|» description|body|string|false|none|
|» regex|body|string|true|none|
|» sampleData|body|string|false|Optionally, paste in sample data to match against this regex|
|» tags|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "lib": "string",
      "description": "string",
      "regex": "string",
      "sampleData": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="update-regexlibentry-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of RegexLibEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-regexlibentry-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[RegexLibEntry](#schemaregexlibentry)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» regex|string|true|none|none|
|»» sampleData|string|false|none|Optionally, paste in sample data to match against this regex|
|»» tags|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_RegexLibEntry">RegexLibEntry</h2>
<!-- backwards compatibility -->
<a id="schemaregexlibentry"></a>
<a id="schema_RegexLibEntry"></a>
<a id="tocSregexlibentry"></a>
<a id="tocsregexlibentry"></a>

```json
{
  "id": "string",
  "lib": "string",
  "description": "string",
  "regex": "string",
  "sampleData": "string",
  "tags": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|lib|string|false|none|none|
|description|string|false|none|none|
|regex|string|true|none|none|
|sampleData|string|false|none|Optionally, paste in sample data to match against this regex|
|tags|string|false|none|none|

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

