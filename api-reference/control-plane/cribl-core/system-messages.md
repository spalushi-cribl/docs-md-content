
<h1 id="cribl-core-api-system-messages">Cribl Core API - System Messages v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-system-messages-system-messages">System Messages</h1>

## Get a list of BulletinMessage objects

<a id="opIdlistBulletinMessage"></a>

> Code samples

`GET /system/messages`

Get a list of BulletinMessage objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "severity": "info",
      "title": "string",
      "text": "string",
      "time": 0,
      "group": "string",
      "metadata": [
        {}
      ]
    }
  ]
}
```

<h3 id="get-a-list-of-bulletinmessage-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of BulletinMessage objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-bulletinmessage-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[BulletinMessage](#schemabulletinmessage)]|false|none|none|
|»» id|string|true|none|none|
|»» severity|string|false|none|none|
|»» title|string|false|none|none|
|»» text|string|true|none|none|
|»» time|number|false|none|none|
|»» group|string|false|none|none|
|»» metadata|[object]|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|severity|info|
|severity|warn|
|severity|error|
|severity|fatal|

<aside class="success">
This operation does not require authentication
</aside>

## Create BulletinMessage

<a id="opIdcreateBulletinMessage"></a>

> Code samples

`POST /system/messages`

Create BulletinMessage

> Body parameter

```json
{
  "id": "string",
  "severity": "info",
  "title": "string",
  "text": "string",
  "time": 0,
  "group": "string",
  "metadata": [
    {}
  ]
}
```

<h3 id="create-bulletinmessage-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[BulletinMessage](#schemabulletinmessage)|true|New BulletinMessage object|
|» id|body|string|true|none|
|» severity|body|string|false|none|
|» title|body|string|false|none|
|» text|body|string|true|none|
|» time|body|number|false|none|
|» group|body|string|false|none|
|» metadata|body|[object]|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» severity|info|
|» severity|warn|
|» severity|error|
|» severity|fatal|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "severity": "info",
      "title": "string",
      "text": "string",
      "time": 0,
      "group": "string",
      "metadata": [
        {}
      ]
    }
  ]
}
```

<h3 id="create-bulletinmessage-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of BulletinMessage objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-bulletinmessage-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[BulletinMessage](#schemabulletinmessage)]|false|none|none|
|»» id|string|true|none|none|
|»» severity|string|false|none|none|
|»» title|string|false|none|none|
|»» text|string|true|none|none|
|»» time|number|false|none|none|
|»» group|string|false|none|none|
|»» metadata|[object]|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|severity|info|
|severity|warn|
|severity|error|
|severity|fatal|

<aside class="success">
This operation does not require authentication
</aside>

## Delete BulletinMessage

<a id="opIddeleteBulletinMessageById"></a>

> Code samples

`DELETE /system/messages/{id}`

Delete BulletinMessage

<h3 id="delete-bulletinmessage-parameters">Parameters</h3>

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
      "severity": "info",
      "title": "string",
      "text": "string",
      "time": 0,
      "group": "string",
      "metadata": [
        {}
      ]
    }
  ]
}
```

<h3 id="delete-bulletinmessage-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of BulletinMessage objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-bulletinmessage-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[BulletinMessage](#schemabulletinmessage)]|false|none|none|
|»» id|string|true|none|none|
|»» severity|string|false|none|none|
|»» title|string|false|none|none|
|»» text|string|true|none|none|
|»» time|number|false|none|none|
|»» group|string|false|none|none|
|»» metadata|[object]|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|severity|info|
|severity|warn|
|severity|error|
|severity|fatal|

<aside class="success">
This operation does not require authentication
</aside>

## Get BulletinMessage by ID

<a id="opIdgetBulletinMessageById"></a>

> Code samples

`GET /system/messages/{id}`

Get BulletinMessage by ID

<h3 id="get-bulletinmessage-by-id-parameters">Parameters</h3>

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
      "severity": "info",
      "title": "string",
      "text": "string",
      "time": 0,
      "group": "string",
      "metadata": [
        {}
      ]
    }
  ]
}
```

<h3 id="get-bulletinmessage-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of BulletinMessage objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-bulletinmessage-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[BulletinMessage](#schemabulletinmessage)]|false|none|none|
|»» id|string|true|none|none|
|»» severity|string|false|none|none|
|»» title|string|false|none|none|
|»» text|string|true|none|none|
|»» time|number|false|none|none|
|»» group|string|false|none|none|
|»» metadata|[object]|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|severity|info|
|severity|warn|
|severity|error|
|severity|fatal|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_BulletinMessage">BulletinMessage</h2>
<!-- backwards compatibility -->
<a id="schemabulletinmessage"></a>
<a id="schema_BulletinMessage"></a>
<a id="tocSbulletinmessage"></a>
<a id="tocsbulletinmessage"></a>

```json
{
  "id": "string",
  "severity": "info",
  "title": "string",
  "text": "string",
  "time": 0,
  "group": "string",
  "metadata": [
    {}
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|severity|string|false|none|none|
|title|string|false|none|none|
|text|string|true|none|none|
|time|number|false|none|none|
|group|string|false|none|none|
|metadata|[object]|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|severity|info|
|severity|warn|
|severity|error|
|severity|fatal|

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

