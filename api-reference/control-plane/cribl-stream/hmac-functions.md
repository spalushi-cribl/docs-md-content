
<h1 id="cribl-stream-api-hmac-functions">Cribl Stream API - HMAC Functions v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-hmac-functions-hmac-functions">HMAC Functions</h1>

## Get a list of HmacFunction objects

<a id="opIdlistHmacFunction"></a>

> Code samples

`GET /lib/hmac-functions`

Get a list of HmacFunction objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": "string",
      "headerExpression": "string",
      "headerName": "string",
      "id": "string",
      "lib": "cribl",
      "stringBuilders": [
        "string"
      ],
      "stringDelim": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-hmacfunction-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of HmacFunction objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-hmacfunction-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[HmacFunction](#schemahmacfunction)]|false|none|none|
|»» description|string|false|none|none|
|»» headerExpression|string|true|none|none|
|»» headerName|string|true|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» stringBuilders|[string]|true|none|none|
|»» stringDelim|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Create HmacFunction

<a id="opIdcreateHmacFunction"></a>

> Code samples

`POST /lib/hmac-functions`

Create HmacFunction

> Body parameter

```json
{
  "description": "string",
  "headerExpression": "string",
  "headerName": "string",
  "id": "string",
  "lib": "cribl",
  "stringBuilders": [
    "string"
  ],
  "stringDelim": "string"
}
```

<h3 id="create-hmacfunction-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[HmacFunction](#schemahmacfunction)|true|New HmacFunction object|
|» description|body|string|false|none|
|» headerExpression|body|string|true|none|
|» headerName|body|string|true|none|
|» id|body|string|true|none|
|» lib|body|[CriblLib](#schemacribllib)|true|none|
|» stringBuilders|body|[string]|true|none|
|» stringDelim|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» lib|cribl|
|» lib|cribl-custom|
|» lib|custom|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": "string",
      "headerExpression": "string",
      "headerName": "string",
      "id": "string",
      "lib": "cribl",
      "stringBuilders": [
        "string"
      ],
      "stringDelim": "string"
    }
  ]
}
```

<h3 id="create-hmacfunction-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of HmacFunction objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-hmacfunction-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[HmacFunction](#schemahmacfunction)]|false|none|none|
|»» description|string|false|none|none|
|»» headerExpression|string|true|none|none|
|»» headerName|string|true|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» stringBuilders|[string]|true|none|none|
|»» stringDelim|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Delete HmacFunction

<a id="opIddeleteHmacFunctionById"></a>

> Code samples

`DELETE /lib/hmac-functions/{id}`

Delete HmacFunction

<h3 id="delete-hmacfunction-parameters">Parameters</h3>

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
      "description": "string",
      "headerExpression": "string",
      "headerName": "string",
      "id": "string",
      "lib": "cribl",
      "stringBuilders": [
        "string"
      ],
      "stringDelim": "string"
    }
  ]
}
```

<h3 id="delete-hmacfunction-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of HmacFunction objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-hmacfunction-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[HmacFunction](#schemahmacfunction)]|false|none|none|
|»» description|string|false|none|none|
|»» headerExpression|string|true|none|none|
|»» headerName|string|true|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» stringBuilders|[string]|true|none|none|
|»» stringDelim|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Get HmacFunction by ID

<a id="opIdgetHmacFunctionById"></a>

> Code samples

`GET /lib/hmac-functions/{id}`

Get HmacFunction by ID

<h3 id="get-hmacfunction-by-id-parameters">Parameters</h3>

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
      "description": "string",
      "headerExpression": "string",
      "headerName": "string",
      "id": "string",
      "lib": "cribl",
      "stringBuilders": [
        "string"
      ],
      "stringDelim": "string"
    }
  ]
}
```

<h3 id="get-hmacfunction-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of HmacFunction objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-hmacfunction-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[HmacFunction](#schemahmacfunction)]|false|none|none|
|»» description|string|false|none|none|
|»» headerExpression|string|true|none|none|
|»» headerName|string|true|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» stringBuilders|[string]|true|none|none|
|»» stringDelim|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Update HmacFunction

<a id="opIdupdateHmacFunctionById"></a>

> Code samples

`PATCH /lib/hmac-functions/{id}`

Update HmacFunction

> Body parameter

```json
{
  "description": "string",
  "headerExpression": "string",
  "headerName": "string",
  "id": "string",
  "lib": "cribl",
  "stringBuilders": [
    "string"
  ],
  "stringDelim": "string"
}
```

<h3 id="update-hmacfunction-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[HmacFunction](#schemahmacfunction)|true|HmacFunction object to be updated|
|» description|body|string|false|none|
|» headerExpression|body|string|true|none|
|» headerName|body|string|true|none|
|» id|body|string|true|none|
|» lib|body|[CriblLib](#schemacribllib)|true|none|
|» stringBuilders|body|[string]|true|none|
|» stringDelim|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» lib|cribl|
|» lib|cribl-custom|
|» lib|custom|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": "string",
      "headerExpression": "string",
      "headerName": "string",
      "id": "string",
      "lib": "cribl",
      "stringBuilders": [
        "string"
      ],
      "stringDelim": "string"
    }
  ]
}
```

<h3 id="update-hmacfunction-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of HmacFunction objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-hmacfunction-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[HmacFunction](#schemahmacfunction)]|false|none|none|
|»» description|string|false|none|none|
|»» headerExpression|string|true|none|none|
|»» headerName|string|true|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» stringBuilders|[string]|true|none|none|
|»» stringDelim|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_HmacFunction">HmacFunction</h2>
<!-- backwards compatibility -->
<a id="schemahmacfunction"></a>
<a id="schema_HmacFunction"></a>
<a id="tocShmacfunction"></a>
<a id="tocshmacfunction"></a>

```json
{
  "description": "string",
  "headerExpression": "string",
  "headerName": "string",
  "id": "string",
  "lib": "cribl",
  "stringBuilders": [
    "string"
  ],
  "stringDelim": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|description|string|false|none|none|
|headerExpression|string|true|none|none|
|headerName|string|true|none|none|
|id|string|true|none|none|
|lib|[CriblLib](#schemacribllib)|true|none|none|
|stringBuilders|[string]|true|none|none|
|stringDelim|string|false|none|none|

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

<h2 id="tocS_CriblLib">CriblLib</h2>
<!-- backwards compatibility -->
<a id="schemacribllib"></a>
<a id="schema_CriblLib"></a>
<a id="tocScribllib"></a>
<a id="tocscribllib"></a>

```json
"cribl"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|cribl|
|*anonymous*|cribl-custom|
|*anonymous*|custom|

