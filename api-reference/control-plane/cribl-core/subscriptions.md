
<h1 id="cribl-core-api-subscriptions">Cribl Core API - Subscriptions v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-subscriptions-subscriptions">Subscriptions</h1>

## Get a list of Subscription objects

<a id="opIdlistSubscription"></a>

> Code samples

`GET /system/subscriptions`

Get a list of Subscription objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "consumer": {
        "connections": [
          {
            "output": "string",
            "pipeline": "string"
          }
        ],
        "disabled": true,
        "type": "string"
      },
      "description": "string",
      "disabled": true,
      "filter": "string",
      "id": "string",
      "pipeline": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-subscription-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Subscription objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-subscription-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Subscription](#schemasubscription)]|false|none|none|
|»» consumer|[SubscriptionConsumer](#schemasubscriptionconsumer)|false|none|none|
|»»» connections|[[Connection](#schemaconnection)]|false|none|none|
|»»»» output|string|true|none|none|
|»»»» pipeline|string|false|none|none|
|»»» disabled|boolean|false|none|none|
|»»» type|string|false|none|none|
|»» description|string|false|none|none|
|»» disabled|boolean|false|none|none|
|»» filter|string|false|none|none|
|»» id|string|true|none|none|
|»» pipeline|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create subscription

<a id="opIdcreateSubscription"></a>

> Code samples

`POST /system/subscriptions`

Create subscription

> Body parameter

```json
{
  "consumer": {
    "connections": [
      {
        "output": "string",
        "pipeline": "string"
      }
    ],
    "disabled": true,
    "type": "string"
  },
  "description": "string",
  "disabled": true,
  "filter": "string",
  "id": "string",
  "pipeline": "string"
}
```

<h3 id="create-subscription-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[Subscription](#schemasubscription)|true|New Subscription object|
|» consumer|body|[SubscriptionConsumer](#schemasubscriptionconsumer)|false|none|
|»» connections|body|[[Connection](#schemaconnection)]|false|none|
|»»» output|body|string|true|none|
|»»» pipeline|body|string|false|none|
|»» disabled|body|boolean|false|none|
|»» type|body|string|false|none|
|» description|body|string|false|none|
|» disabled|body|boolean|false|none|
|» filter|body|string|false|none|
|» id|body|string|true|none|
|» pipeline|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "consumer": {
        "connections": [
          {
            "output": "string",
            "pipeline": "string"
          }
        ],
        "disabled": true,
        "type": "string"
      },
      "description": "string",
      "disabled": true,
      "filter": "string",
      "id": "string",
      "pipeline": "string"
    }
  ]
}
```

<h3 id="create-subscription-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Subscription objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-subscription-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Subscription](#schemasubscription)]|false|none|none|
|»» consumer|[SubscriptionConsumer](#schemasubscriptionconsumer)|false|none|none|
|»»» connections|[[Connection](#schemaconnection)]|false|none|none|
|»»»» output|string|true|none|none|
|»»»» pipeline|string|false|none|none|
|»»» disabled|boolean|false|none|none|
|»»» type|string|false|none|none|
|»» description|string|false|none|none|
|»» disabled|boolean|false|none|none|
|»» filter|string|false|none|none|
|»» id|string|true|none|none|
|»» pipeline|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete subscription

<a id="opIddeleteSubscriptionById"></a>

> Code samples

`DELETE /system/subscriptions/{id}`

Delete subscription

<h3 id="delete-subscription-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Subscription ID|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "consumer": {
        "connections": [
          {
            "output": "string",
            "pipeline": "string"
          }
        ],
        "disabled": true,
        "type": "string"
      },
      "description": "string",
      "disabled": true,
      "filter": "string",
      "id": "string",
      "pipeline": "string"
    }
  ]
}
```

<h3 id="delete-subscription-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Subscription objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-subscription-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Subscription](#schemasubscription)]|false|none|none|
|»» consumer|[SubscriptionConsumer](#schemasubscriptionconsumer)|false|none|none|
|»»» connections|[[Connection](#schemaconnection)]|false|none|none|
|»»»» output|string|true|none|none|
|»»»» pipeline|string|false|none|none|
|»»» disabled|boolean|false|none|none|
|»»» type|string|false|none|none|
|»» description|string|false|none|none|
|»» disabled|boolean|false|none|none|
|»» filter|string|false|none|none|
|»» id|string|true|none|none|
|»» pipeline|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get Subscription by ID

<a id="opIdgetSubscriptionById"></a>

> Code samples

`GET /system/subscriptions/{id}`

Get Subscription by ID

<h3 id="get-subscription-by-id-parameters">Parameters</h3>

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
      "consumer": {
        "connections": [
          {
            "output": "string",
            "pipeline": "string"
          }
        ],
        "disabled": true,
        "type": "string"
      },
      "description": "string",
      "disabled": true,
      "filter": "string",
      "id": "string",
      "pipeline": "string"
    }
  ]
}
```

<h3 id="get-subscription-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Subscription objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-subscription-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Subscription](#schemasubscription)]|false|none|none|
|»» consumer|[SubscriptionConsumer](#schemasubscriptionconsumer)|false|none|none|
|»»» connections|[[Connection](#schemaconnection)]|false|none|none|
|»»»» output|string|true|none|none|
|»»»» pipeline|string|false|none|none|
|»»» disabled|boolean|false|none|none|
|»»» type|string|false|none|none|
|»» description|string|false|none|none|
|»» disabled|boolean|false|none|none|
|»» filter|string|false|none|none|
|»» id|string|true|none|none|
|»» pipeline|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update subscription

<a id="opIdupdateSubscriptionById"></a>

> Code samples

`PATCH /system/subscriptions/{id}`

Update subscription

> Body parameter

```json
{
  "consumer": {
    "connections": [
      {
        "output": "string",
        "pipeline": "string"
      }
    ],
    "disabled": true,
    "type": "string"
  },
  "description": "string",
  "disabled": true,
  "filter": "string",
  "id": "string",
  "pipeline": "string"
}
```

<h3 id="update-subscription-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Subscription ID|
|body|body|[Subscription](#schemasubscription)|true|Subscription object to be updated|
|» consumer|body|[SubscriptionConsumer](#schemasubscriptionconsumer)|false|none|
|»» connections|body|[[Connection](#schemaconnection)]|false|none|
|»»» output|body|string|true|none|
|»»» pipeline|body|string|false|none|
|»» disabled|body|boolean|false|none|
|»» type|body|string|false|none|
|» description|body|string|false|none|
|» disabled|body|boolean|false|none|
|» filter|body|string|false|none|
|» id|body|string|true|none|
|» pipeline|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "consumer": {
        "connections": [
          {
            "output": "string",
            "pipeline": "string"
          }
        ],
        "disabled": true,
        "type": "string"
      },
      "description": "string",
      "disabled": true,
      "filter": "string",
      "id": "string",
      "pipeline": "string"
    }
  ]
}
```

<h3 id="update-subscription-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Subscription objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-subscription-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Subscription](#schemasubscription)]|false|none|none|
|»» consumer|[SubscriptionConsumer](#schemasubscriptionconsumer)|false|none|none|
|»»» connections|[[Connection](#schemaconnection)]|false|none|none|
|»»»» output|string|true|none|none|
|»»»» pipeline|string|false|none|none|
|»»» disabled|boolean|false|none|none|
|»»» type|string|false|none|none|
|»» description|string|false|none|none|
|»» disabled|boolean|false|none|none|
|»» filter|string|false|none|none|
|»» id|string|true|none|none|
|»» pipeline|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get the subscriptions associated with the project

<a id="opIdgetSubscriptionByProjectId"></a>

> Code samples

`GET /system/projects/{projectId}/subscriptions`

Get the subscriptions associated with the project

<h3 id="get-the-subscriptions-associated-with-the-project-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|projectId|path|string|true|Project Id|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "consumer": {
        "connections": [
          {
            "output": "string",
            "pipeline": "string"
          }
        ],
        "disabled": true,
        "type": "string"
      },
      "description": "string",
      "disabled": true,
      "filter": "string",
      "id": "string",
      "pipeline": "string"
    }
  ]
}
```

<h3 id="get-the-subscriptions-associated-with-the-project-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Subscription objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-subscriptions-associated-with-the-project-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Subscription](#schemasubscription)]|false|none|none|
|»» consumer|[SubscriptionConsumer](#schemasubscriptionconsumer)|false|none|none|
|»»» connections|[[Connection](#schemaconnection)]|false|none|none|
|»»»» output|string|true|none|none|
|»»»» pipeline|string|false|none|none|
|»»» disabled|boolean|false|none|none|
|»»» type|string|false|none|none|
|»» description|string|false|none|none|
|»» disabled|boolean|false|none|none|
|»» filter|string|false|none|none|
|»» id|string|true|none|none|
|»» pipeline|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Subscription">Subscription</h2>
<!-- backwards compatibility -->
<a id="schemasubscription"></a>
<a id="schema_Subscription"></a>
<a id="tocSsubscription"></a>
<a id="tocssubscription"></a>

```json
{
  "consumer": {
    "connections": [
      {
        "output": "string",
        "pipeline": "string"
      }
    ],
    "disabled": true,
    "type": "string"
  },
  "description": "string",
  "disabled": true,
  "filter": "string",
  "id": "string",
  "pipeline": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|consumer|[SubscriptionConsumer](#schemasubscriptionconsumer)|false|none|none|
|description|string|false|none|none|
|disabled|boolean|false|none|none|
|filter|string|false|none|none|
|id|string|true|none|none|
|pipeline|string|true|none|none|

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

<h2 id="tocS_SubscriptionConsumer">SubscriptionConsumer</h2>
<!-- backwards compatibility -->
<a id="schemasubscriptionconsumer"></a>
<a id="schema_SubscriptionConsumer"></a>
<a id="tocSsubscriptionconsumer"></a>
<a id="tocssubscriptionconsumer"></a>

```json
{
  "connections": [
    {
      "output": "string",
      "pipeline": "string"
    }
  ],
  "disabled": true,
  "type": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|connections|[[Connection](#schemaconnection)]|false|none|none|
|disabled|boolean|false|none|none|
|type|string|false|none|none|

<h2 id="tocS_Connection">Connection</h2>
<!-- backwards compatibility -->
<a id="schemaconnection"></a>
<a id="schema_Connection"></a>
<a id="tocSconnection"></a>
<a id="tocsconnection"></a>

```json
{
  "output": "string",
  "pipeline": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|output|string|true|none|none|
|pipeline|string|false|none|none|

