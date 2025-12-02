
<h1 id="cribl-core-api-notification-targets">Cribl Core API - Notification Targets v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-notification-targets-notification-targets">Notification Targets</h1>

## Get a list of NotificationTarget objects

<a id="opIdlistNotificationTarget"></a>

> Code samples

`GET /notification-targets`

Get a list of NotificationTarget objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "type": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-notificationtarget-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of NotificationTarget objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-notificationtarget-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[NotificationTarget](#schemanotificationtarget)]|false|none|none|
|»» id|string|true|none|none|
|»» type|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create NotificationTarget

<a id="opIdcreateNotificationTarget"></a>

> Code samples

`POST /notification-targets`

Create NotificationTarget

> Body parameter

```json
{
  "id": "string",
  "type": "string"
}
```

<h3 id="create-notificationtarget-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[NotificationTarget](#schemanotificationtarget)|true|New NotificationTarget object|
|» id|body|string|true|none|
|» type|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "type": "string"
    }
  ]
}
```

<h3 id="create-notificationtarget-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of NotificationTarget objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-notificationtarget-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[NotificationTarget](#schemanotificationtarget)]|false|none|none|
|»» id|string|true|none|none|
|»» type|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete NotificationTarget

<a id="opIddeleteNotificationTargetById"></a>

> Code samples

`DELETE /notification-targets/{id}`

Delete NotificationTarget

<h3 id="delete-notificationtarget-parameters">Parameters</h3>

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
      "type": "string"
    }
  ]
}
```

<h3 id="delete-notificationtarget-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of NotificationTarget objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-notificationtarget-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[NotificationTarget](#schemanotificationtarget)]|false|none|none|
|»» id|string|true|none|none|
|»» type|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get NotificationTarget by ID

<a id="opIdgetNotificationTargetById"></a>

> Code samples

`GET /notification-targets/{id}`

Get NotificationTarget by ID

<h3 id="get-notificationtarget-by-id-parameters">Parameters</h3>

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
      "type": "string"
    }
  ]
}
```

<h3 id="get-notificationtarget-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of NotificationTarget objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-notificationtarget-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[NotificationTarget](#schemanotificationtarget)]|false|none|none|
|»» id|string|true|none|none|
|»» type|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update NotificationTarget

<a id="opIdupdateNotificationTargetById"></a>

> Code samples

`PATCH /notification-targets/{id}`

Update NotificationTarget

> Body parameter

```json
{
  "id": "string",
  "type": "string"
}
```

<h3 id="update-notificationtarget-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[NotificationTarget](#schemanotificationtarget)|true|NotificationTarget object to be updated|
|» id|body|string|true|none|
|» type|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "type": "string"
    }
  ]
}
```

<h3 id="update-notificationtarget-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of NotificationTarget objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-notificationtarget-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[NotificationTarget](#schemanotificationtarget)]|false|none|none|
|»» id|string|true|none|none|
|»» type|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_NotificationTarget">NotificationTarget</h2>
<!-- backwards compatibility -->
<a id="schemanotificationtarget"></a>
<a id="schema_NotificationTarget"></a>
<a id="tocSnotificationtarget"></a>
<a id="tocsnotificationtarget"></a>

```json
{
  "id": "string",
  "type": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|type|string|true|none|none|

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

