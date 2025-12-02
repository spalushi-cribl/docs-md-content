
<h1 id="cribl-core-api-banners">Cribl Core API - Banners v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-banners-banners">Banners</h1>

## Get a list of BannerMessage objects

<a id="opIdlistBannerMessage"></a>

> Code samples

`GET /system/banners`

Get a list of BannerMessage objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "enabled": true,
      "type": "custom",
      "created": 0,
      "theme": "string",
      "invertFontColor": true,
      "message": "string",
      "link": "string",
      "linkDisplay": "string",
      "customThemes": [
        "string"
      ]
    }
  ]
}
```

<h3 id="get-a-list-of-bannermessage-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of BannerMessage objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-bannermessage-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[BannerMessage](#schemabannermessage)]|false|none|none|
|»» id|string|false|none|none|
|»» enabled|boolean|true|none|Show a banner on top of all pages|
|»» type|string|true|none|none|
|»» created|number|false|none|Time created|
|»» theme|string|true|none|none|
|»» invertFontColor|boolean|false|none|none|
|»» message|string|true|none|Enter a message to display to all your Organization's users, across all Cribl products. Limited to one line and 100 characters; will be truncated as needed.|
|»» link|string|false|none|Optionally, provide a URL to append to the message|
|»» linkDisplay|string|false|none|Optionally, display your link with a short text label instead of the raw URL (100-character limit)|
|»» customThemes|[string]|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|custom|
|type|system|

<aside class="success">
This operation does not require authentication
</aside>

## Create BannerMessage

<a id="opIdcreateBannerMessage"></a>

> Code samples

`POST /system/banners`

Create BannerMessage

> Body parameter

```json
{
  "id": "string",
  "enabled": true,
  "type": "custom",
  "created": 0,
  "theme": "string",
  "invertFontColor": true,
  "message": "string",
  "link": "string",
  "linkDisplay": "string",
  "customThemes": [
    "string"
  ]
}
```

<h3 id="create-bannermessage-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[BannerMessage](#schemabannermessage)|true|New BannerMessage object|
|» id|body|string|false|none|
|» enabled|body|boolean|true|Show a banner on top of all pages|
|» type|body|string|true|none|
|» created|body|number|false|Time created|
|» theme|body|string|true|none|
|» invertFontColor|body|boolean|false|none|
|» message|body|string|true|Enter a message to display to all your Organization's users, across all Cribl products. Limited to one line and 100 characters; will be truncated as needed.|
|» link|body|string|false|Optionally, provide a URL to append to the message|
|» linkDisplay|body|string|false|Optionally, display your link with a short text label instead of the raw URL (100-character limit)|
|» customThemes|body|[string]|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» type|custom|
|» type|system|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "enabled": true,
      "type": "custom",
      "created": 0,
      "theme": "string",
      "invertFontColor": true,
      "message": "string",
      "link": "string",
      "linkDisplay": "string",
      "customThemes": [
        "string"
      ]
    }
  ]
}
```

<h3 id="create-bannermessage-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of BannerMessage objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-bannermessage-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[BannerMessage](#schemabannermessage)]|false|none|none|
|»» id|string|false|none|none|
|»» enabled|boolean|true|none|Show a banner on top of all pages|
|»» type|string|true|none|none|
|»» created|number|false|none|Time created|
|»» theme|string|true|none|none|
|»» invertFontColor|boolean|false|none|none|
|»» message|string|true|none|Enter a message to display to all your Organization's users, across all Cribl products. Limited to one line and 100 characters; will be truncated as needed.|
|»» link|string|false|none|Optionally, provide a URL to append to the message|
|»» linkDisplay|string|false|none|Optionally, display your link with a short text label instead of the raw URL (100-character limit)|
|»» customThemes|[string]|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|custom|
|type|system|

<aside class="success">
This operation does not require authentication
</aside>

## Delete BannerMessage

<a id="opIddeleteBannerMessageById"></a>

> Code samples

`DELETE /system/banners/{id}`

Delete BannerMessage

<h3 id="delete-bannermessage-parameters">Parameters</h3>

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
      "enabled": true,
      "type": "custom",
      "created": 0,
      "theme": "string",
      "invertFontColor": true,
      "message": "string",
      "link": "string",
      "linkDisplay": "string",
      "customThemes": [
        "string"
      ]
    }
  ]
}
```

<h3 id="delete-bannermessage-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of BannerMessage objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-bannermessage-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[BannerMessage](#schemabannermessage)]|false|none|none|
|»» id|string|false|none|none|
|»» enabled|boolean|true|none|Show a banner on top of all pages|
|»» type|string|true|none|none|
|»» created|number|false|none|Time created|
|»» theme|string|true|none|none|
|»» invertFontColor|boolean|false|none|none|
|»» message|string|true|none|Enter a message to display to all your Organization's users, across all Cribl products. Limited to one line and 100 characters; will be truncated as needed.|
|»» link|string|false|none|Optionally, provide a URL to append to the message|
|»» linkDisplay|string|false|none|Optionally, display your link with a short text label instead of the raw URL (100-character limit)|
|»» customThemes|[string]|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|custom|
|type|system|

<aside class="success">
This operation does not require authentication
</aside>

## Get BannerMessage by ID

<a id="opIdgetBannerMessageById"></a>

> Code samples

`GET /system/banners/{id}`

Get BannerMessage by ID

<h3 id="get-bannermessage-by-id-parameters">Parameters</h3>

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
      "enabled": true,
      "type": "custom",
      "created": 0,
      "theme": "string",
      "invertFontColor": true,
      "message": "string",
      "link": "string",
      "linkDisplay": "string",
      "customThemes": [
        "string"
      ]
    }
  ]
}
```

<h3 id="get-bannermessage-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of BannerMessage objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-bannermessage-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[BannerMessage](#schemabannermessage)]|false|none|none|
|»» id|string|false|none|none|
|»» enabled|boolean|true|none|Show a banner on top of all pages|
|»» type|string|true|none|none|
|»» created|number|false|none|Time created|
|»» theme|string|true|none|none|
|»» invertFontColor|boolean|false|none|none|
|»» message|string|true|none|Enter a message to display to all your Organization's users, across all Cribl products. Limited to one line and 100 characters; will be truncated as needed.|
|»» link|string|false|none|Optionally, provide a URL to append to the message|
|»» linkDisplay|string|false|none|Optionally, display your link with a short text label instead of the raw URL (100-character limit)|
|»» customThemes|[string]|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|custom|
|type|system|

<aside class="success">
This operation does not require authentication
</aside>

## Update BannerMessage

<a id="opIdupdateBannerMessageById"></a>

> Code samples

`PATCH /system/banners/{id}`

Update BannerMessage

> Body parameter

```json
{
  "id": "string",
  "enabled": true,
  "type": "custom",
  "created": 0,
  "theme": "string",
  "invertFontColor": true,
  "message": "string",
  "link": "string",
  "linkDisplay": "string",
  "customThemes": [
    "string"
  ]
}
```

<h3 id="update-bannermessage-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[BannerMessage](#schemabannermessage)|true|BannerMessage object to be updated|
|» id|body|string|false|none|
|» enabled|body|boolean|true|Show a banner on top of all pages|
|» type|body|string|true|none|
|» created|body|number|false|Time created|
|» theme|body|string|true|none|
|» invertFontColor|body|boolean|false|none|
|» message|body|string|true|Enter a message to display to all your Organization's users, across all Cribl products. Limited to one line and 100 characters; will be truncated as needed.|
|» link|body|string|false|Optionally, provide a URL to append to the message|
|» linkDisplay|body|string|false|Optionally, display your link with a short text label instead of the raw URL (100-character limit)|
|» customThemes|body|[string]|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» type|custom|
|» type|system|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "enabled": true,
      "type": "custom",
      "created": 0,
      "theme": "string",
      "invertFontColor": true,
      "message": "string",
      "link": "string",
      "linkDisplay": "string",
      "customThemes": [
        "string"
      ]
    }
  ]
}
```

<h3 id="update-bannermessage-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of BannerMessage objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-bannermessage-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[BannerMessage](#schemabannermessage)]|false|none|none|
|»» id|string|false|none|none|
|»» enabled|boolean|true|none|Show a banner on top of all pages|
|»» type|string|true|none|none|
|»» created|number|false|none|Time created|
|»» theme|string|true|none|none|
|»» invertFontColor|boolean|false|none|none|
|»» message|string|true|none|Enter a message to display to all your Organization's users, across all Cribl products. Limited to one line and 100 characters; will be truncated as needed.|
|»» link|string|false|none|Optionally, provide a URL to append to the message|
|»» linkDisplay|string|false|none|Optionally, display your link with a short text label instead of the raw URL (100-character limit)|
|»» customThemes|[string]|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|custom|
|type|system|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_BannerMessage">BannerMessage</h2>
<!-- backwards compatibility -->
<a id="schemabannermessage"></a>
<a id="schema_BannerMessage"></a>
<a id="tocSbannermessage"></a>
<a id="tocsbannermessage"></a>

```json
{
  "id": "string",
  "enabled": true,
  "type": "custom",
  "created": 0,
  "theme": "string",
  "invertFontColor": true,
  "message": "string",
  "link": "string",
  "linkDisplay": "string",
  "customThemes": [
    "string"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|enabled|boolean|true|none|Show a banner on top of all pages|
|type|string|true|none|none|
|created|number|false|none|Time created|
|theme|string|true|none|none|
|invertFontColor|boolean|false|none|none|
|message|string|true|none|Enter a message to display to all your Organization's users, across all Cribl products. Limited to one line and 100 characters; will be truncated as needed.|
|link|string|false|none|Optionally, provide a URL to append to the message|
|linkDisplay|string|false|none|Optionally, display your link with a short text label instead of the raw URL (100-character limit)|
|customThemes|[string]|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|custom|
|type|system|

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

