
<h1 id="cribl-core-api-notifications">Cribl Core API - Notifications v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-notifications-notifications">Notifications</h1>

## Get a list of Notification objects

<a id="opIdlistNotification"></a>

> Code samples

`GET /notifications`

Get a list of Notification objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "disabled": false,
      "condition": "string",
      "targets": [],
      "targetConfigs": [
        {
          "id": "string",
          "conf": {
            "subject": "string",
            "body": "string",
            "emailRecipient": {
              "to": "string",
              "cc": "string",
              "bcc": "string"
            }
          }
        }
      ],
      "conf": {},
      "metadata": [
        {
          "name": "string",
          "value": "string"
        }
      ]
    }
  ]
}
```

<h3 id="get-a-list-of-notification-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Notification objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-notification-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Notification](#schemanotification)]|false|none|none|
|»» id|string|true|none|none|
|»» disabled|boolean|false|none|none|
|»» condition|string|true|none|none|
|»» targets|[string]|false|none|Targets to send any Notifications to|
|»» targetConfigs|[anyOf]|false|none|none|
|»»» id|string|true|none|none|
|»» conf|object|false|none|none|
|»» metadata|[object]|false|none|Fields to add to events from this input|
|»»» name|string|true|none|none|
|»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|

<aside class="success">
This operation does not require authentication
</aside>

## Create Notification

<a id="opIdcreateNotification"></a>

> Code samples

`POST /notifications`

Create Notification

> Body parameter

```json
{
  "id": "string",
  "disabled": false,
  "condition": "string",
  "targets": [],
  "targetConfigs": [
    {
      "id": "string",
      "conf": {
        "subject": "string",
        "body": "string",
        "emailRecipient": {
          "to": "string",
          "cc": "string",
          "bcc": "string"
        }
      }
    }
  ],
  "conf": {},
  "metadata": [
    {
      "name": "string",
      "value": "string"
    }
  ]
}
```

<h3 id="create-notification-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[Notification](#schemanotification)|true|New Notification object|
|» id|body|string|true|none|
|» disabled|body|boolean|false|none|
|» condition|body|string|true|none|
|» targets|body|[string]|false|Targets to send any Notifications to|
|» targetConfigs|body|[anyOf]|false|none|
|»» id|body|string|true|none|
|» conf|body|object|false|none|
|» metadata|body|[object]|false|Fields to add to events from this input|
|»» name|body|string|true|none|
|»» value|body|string|true|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "disabled": false,
      "condition": "string",
      "targets": [],
      "targetConfigs": [
        {
          "id": "string",
          "conf": {
            "subject": "string",
            "body": "string",
            "emailRecipient": {
              "to": "string",
              "cc": "string",
              "bcc": "string"
            }
          }
        }
      ],
      "conf": {},
      "metadata": [
        {
          "name": "string",
          "value": "string"
        }
      ]
    }
  ]
}
```

<h3 id="create-notification-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Notification objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-notification-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Notification](#schemanotification)]|false|none|none|
|»» id|string|true|none|none|
|»» disabled|boolean|false|none|none|
|»» condition|string|true|none|none|
|»» targets|[string]|false|none|Targets to send any Notifications to|
|»» targetConfigs|[anyOf]|false|none|none|
|»»» id|string|true|none|none|
|»» conf|object|false|none|none|
|»» metadata|[object]|false|none|Fields to add to events from this input|
|»»» name|string|true|none|none|
|»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|

<aside class="success">
This operation does not require authentication
</aside>

## Delete Notification

<a id="opIddeleteNotificationById"></a>

> Code samples

`DELETE /notifications/{id}`

Delete Notification

<h3 id="delete-notification-parameters">Parameters</h3>

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
      "disabled": false,
      "condition": "string",
      "targets": [],
      "targetConfigs": [
        {
          "id": "string",
          "conf": {
            "subject": "string",
            "body": "string",
            "emailRecipient": {
              "to": "string",
              "cc": "string",
              "bcc": "string"
            }
          }
        }
      ],
      "conf": {},
      "metadata": [
        {
          "name": "string",
          "value": "string"
        }
      ]
    }
  ]
}
```

<h3 id="delete-notification-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Notification objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-notification-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Notification](#schemanotification)]|false|none|none|
|»» id|string|true|none|none|
|»» disabled|boolean|false|none|none|
|»» condition|string|true|none|none|
|»» targets|[string]|false|none|Targets to send any Notifications to|
|»» targetConfigs|[anyOf]|false|none|none|
|»»» id|string|true|none|none|
|»» conf|object|false|none|none|
|»» metadata|[object]|false|none|Fields to add to events from this input|
|»»» name|string|true|none|none|
|»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|

<aside class="success">
This operation does not require authentication
</aside>

## Get Notification by ID

<a id="opIdgetNotificationById"></a>

> Code samples

`GET /notifications/{id}`

Get Notification by ID

<h3 id="get-notification-by-id-parameters">Parameters</h3>

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
      "disabled": false,
      "condition": "string",
      "targets": [],
      "targetConfigs": [
        {
          "id": "string",
          "conf": {
            "subject": "string",
            "body": "string",
            "emailRecipient": {
              "to": "string",
              "cc": "string",
              "bcc": "string"
            }
          }
        }
      ],
      "conf": {},
      "metadata": [
        {
          "name": "string",
          "value": "string"
        }
      ]
    }
  ]
}
```

<h3 id="get-notification-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Notification objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-notification-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Notification](#schemanotification)]|false|none|none|
|»» id|string|true|none|none|
|»» disabled|boolean|false|none|none|
|»» condition|string|true|none|none|
|»» targets|[string]|false|none|Targets to send any Notifications to|
|»» targetConfigs|[anyOf]|false|none|none|
|»»» id|string|true|none|none|
|»» conf|object|false|none|none|
|»» metadata|[object]|false|none|Fields to add to events from this input|
|»»» name|string|true|none|none|
|»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|

<aside class="success">
This operation does not require authentication
</aside>

## Update Notification

<a id="opIdupdateNotificationById"></a>

> Code samples

`PATCH /notifications/{id}`

Update Notification

> Body parameter

```json
{
  "id": "string",
  "disabled": false,
  "condition": "string",
  "targets": [],
  "targetConfigs": [
    {
      "id": "string",
      "conf": {
        "subject": "string",
        "body": "string",
        "emailRecipient": {
          "to": "string",
          "cc": "string",
          "bcc": "string"
        }
      }
    }
  ],
  "conf": {},
  "metadata": [
    {
      "name": "string",
      "value": "string"
    }
  ]
}
```

<h3 id="update-notification-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[Notification](#schemanotification)|true|Notification object to be updated|
|» id|body|string|true|none|
|» disabled|body|boolean|false|none|
|» condition|body|string|true|none|
|» targets|body|[string]|false|Targets to send any Notifications to|
|» targetConfigs|body|[anyOf]|false|none|
|»» id|body|string|true|none|
|» conf|body|object|false|none|
|» metadata|body|[object]|false|Fields to add to events from this input|
|»» name|body|string|true|none|
|»» value|body|string|true|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "disabled": false,
      "condition": "string",
      "targets": [],
      "targetConfigs": [
        {
          "id": "string",
          "conf": {
            "subject": "string",
            "body": "string",
            "emailRecipient": {
              "to": "string",
              "cc": "string",
              "bcc": "string"
            }
          }
        }
      ],
      "conf": {},
      "metadata": [
        {
          "name": "string",
          "value": "string"
        }
      ]
    }
  ]
}
```

<h3 id="update-notification-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Notification objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-notification-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Notification](#schemanotification)]|false|none|none|
|»» id|string|true|none|none|
|»» disabled|boolean|false|none|none|
|»» condition|string|true|none|none|
|»» targets|[string]|false|none|Targets to send any Notifications to|
|»» targetConfigs|[anyOf]|false|none|none|
|»»» id|string|true|none|none|
|»» conf|object|false|none|none|
|»» metadata|[object]|false|none|Fields to add to events from this input|
|»»» name|string|true|none|none|
|»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Notification">Notification</h2>
<!-- backwards compatibility -->
<a id="schemanotification"></a>
<a id="schema_Notification"></a>
<a id="tocSnotification"></a>
<a id="tocsnotification"></a>

```json
{
  "id": "string",
  "disabled": false,
  "condition": "string",
  "targets": [],
  "targetConfigs": [
    {
      "id": "string",
      "conf": {
        "subject": "string",
        "body": "string",
        "emailRecipient": {
          "to": "string",
          "cc": "string",
          "bcc": "string"
        }
      }
    }
  ],
  "conf": {},
  "metadata": [
    {
      "name": "string",
      "value": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|disabled|boolean|false|none|none|
|condition|string|true|none|none|
|targets|[string]|false|none|Targets to send any Notifications to|
|targetConfigs|[anyOf]|false|none|none|
|» id|string|true|none|none|
|conf|object|false|none|none|
|metadata|[object]|false|none|Fields to add to events from this input|
|» name|string|true|none|none|
|» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|

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

