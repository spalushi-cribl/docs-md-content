
<h1 id="cribl-edge-api-logging">Cribl Edge API - Logging v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-logging-logging">Logging</h1>

## Get a list of LoggerConfig objects

<a id="opIdlistLoggerConfig"></a>

> Code samples

`GET /system/logger`

Get a list of LoggerConfig objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "channels": [
        {
          "canDelete": true,
          "id": "string",
          "level": "string",
          "ttl": 0
        }
      ],
      "defaultRedactFields": [
        "string"
      ],
      "id": "string",
      "limitRate": 0,
      "maxSizeBytes": 0,
      "redactFields": [
        "string"
      ],
      "redactLabel": "string",
      "ttlConfig": {
        "property1": {
          "level": "string",
          "ttl": 0
        },
        "property2": {
          "level": "string",
          "ttl": 0
        }
      }
    }
  ]
}
```

<h3 id="get-a-list-of-loggerconfig-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of LoggerConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-loggerconfig-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[LoggerConfig](#schemaloggerconfig)]|false|none|none|
|»» channels|[[LoggerEntry](#schemaloggerentry)]|true|none|none|
|»»» canDelete|boolean|false|none|none|
|»»» id|string|true|none|none|
|»»» level|string|true|none|none|
|»»» ttl|number|false|none|none|
|»» defaultRedactFields|[string]|false|none|none|
|»» id|string|true|none|none|
|»» limitRate|number|true|none|none|
|»» maxSizeBytes|number|true|none|none|
|»» redactFields|[string]|true|none|none|
|»» redactLabel|string|true|none|none|
|»» ttlConfig|[TtlConfig](#schemattlconfig)|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» level|string|true|none|none|
|»»»» ttl|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete LoggerConfig

<a id="opIddeleteLoggerConfigById"></a>

> Code samples

`DELETE /system/logger/{id}`

Delete LoggerConfig

<h3 id="delete-loggerconfig-parameters">Parameters</h3>

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
      "channels": [
        {
          "canDelete": true,
          "id": "string",
          "level": "string",
          "ttl": 0
        }
      ],
      "defaultRedactFields": [
        "string"
      ],
      "id": "string",
      "limitRate": 0,
      "maxSizeBytes": 0,
      "redactFields": [
        "string"
      ],
      "redactLabel": "string",
      "ttlConfig": {
        "property1": {
          "level": "string",
          "ttl": 0
        },
        "property2": {
          "level": "string",
          "ttl": 0
        }
      }
    }
  ]
}
```

<h3 id="delete-loggerconfig-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of LoggerConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-loggerconfig-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[LoggerConfig](#schemaloggerconfig)]|false|none|none|
|»» channels|[[LoggerEntry](#schemaloggerentry)]|true|none|none|
|»»» canDelete|boolean|false|none|none|
|»»» id|string|true|none|none|
|»»» level|string|true|none|none|
|»»» ttl|number|false|none|none|
|»» defaultRedactFields|[string]|false|none|none|
|»» id|string|true|none|none|
|»» limitRate|number|true|none|none|
|»» maxSizeBytes|number|true|none|none|
|»» redactFields|[string]|true|none|none|
|»» redactLabel|string|true|none|none|
|»» ttlConfig|[TtlConfig](#schemattlconfig)|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» level|string|true|none|none|
|»»»» ttl|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get LoggerConfig by ID

<a id="opIdgetLoggerConfigById"></a>

> Code samples

`GET /system/logger/{id}`

Get LoggerConfig by ID

<h3 id="get-loggerconfig-by-id-parameters">Parameters</h3>

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
      "channels": [
        {
          "canDelete": true,
          "id": "string",
          "level": "string",
          "ttl": 0
        }
      ],
      "defaultRedactFields": [
        "string"
      ],
      "id": "string",
      "limitRate": 0,
      "maxSizeBytes": 0,
      "redactFields": [
        "string"
      ],
      "redactLabel": "string",
      "ttlConfig": {
        "property1": {
          "level": "string",
          "ttl": 0
        },
        "property2": {
          "level": "string",
          "ttl": 0
        }
      }
    }
  ]
}
```

<h3 id="get-loggerconfig-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of LoggerConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-loggerconfig-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[LoggerConfig](#schemaloggerconfig)]|false|none|none|
|»» channels|[[LoggerEntry](#schemaloggerentry)]|true|none|none|
|»»» canDelete|boolean|false|none|none|
|»»» id|string|true|none|none|
|»»» level|string|true|none|none|
|»»» ttl|number|false|none|none|
|»» defaultRedactFields|[string]|false|none|none|
|»» id|string|true|none|none|
|»» limitRate|number|true|none|none|
|»» maxSizeBytes|number|true|none|none|
|»» redactFields|[string]|true|none|none|
|»» redactLabel|string|true|none|none|
|»» ttlConfig|[TtlConfig](#schemattlconfig)|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» level|string|true|none|none|
|»»»» ttl|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update LoggerConfig

<a id="opIdupdateLoggerConfigById"></a>

> Code samples

`PATCH /system/logger/{id}`

Update LoggerConfig

> Body parameter

```json
{
  "channels": [
    {
      "canDelete": true,
      "id": "string",
      "level": "string",
      "ttl": 0
    }
  ],
  "defaultRedactFields": [
    "string"
  ],
  "id": "string",
  "limitRate": 0,
  "maxSizeBytes": 0,
  "redactFields": [
    "string"
  ],
  "redactLabel": "string",
  "ttlConfig": {
    "property1": {
      "level": "string",
      "ttl": 0
    },
    "property2": {
      "level": "string",
      "ttl": 0
    }
  }
}
```

<h3 id="update-loggerconfig-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[LoggerConfig](#schemaloggerconfig)|true|LoggerConfig object to be updated|
|» channels|body|[[LoggerEntry](#schemaloggerentry)]|true|none|
|»» canDelete|body|boolean|false|none|
|»» id|body|string|true|none|
|»» level|body|string|true|none|
|»» ttl|body|number|false|none|
|» defaultRedactFields|body|[string]|false|none|
|» id|body|string|true|none|
|» limitRate|body|number|true|none|
|» maxSizeBytes|body|number|true|none|
|» redactFields|body|[string]|true|none|
|» redactLabel|body|string|true|none|
|» ttlConfig|body|[TtlConfig](#schemattlconfig)|false|none|
|»» **additionalProperties**|body|object|false|none|
|»»» level|body|string|true|none|
|»»» ttl|body|number|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "channels": [
        {
          "canDelete": true,
          "id": "string",
          "level": "string",
          "ttl": 0
        }
      ],
      "defaultRedactFields": [
        "string"
      ],
      "id": "string",
      "limitRate": 0,
      "maxSizeBytes": 0,
      "redactFields": [
        "string"
      ],
      "redactLabel": "string",
      "ttlConfig": {
        "property1": {
          "level": "string",
          "ttl": 0
        },
        "property2": {
          "level": "string",
          "ttl": 0
        }
      }
    }
  ]
}
```

<h3 id="update-loggerconfig-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of LoggerConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-loggerconfig-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[LoggerConfig](#schemaloggerconfig)]|false|none|none|
|»» channels|[[LoggerEntry](#schemaloggerentry)]|true|none|none|
|»»» canDelete|boolean|false|none|none|
|»»» id|string|true|none|none|
|»»» level|string|true|none|none|
|»»» ttl|number|false|none|none|
|»» defaultRedactFields|[string]|false|none|none|
|»» id|string|true|none|none|
|»» limitRate|number|true|none|none|
|»» maxSizeBytes|number|true|none|none|
|»» redactFields|[string]|true|none|none|
|»» redactLabel|string|true|none|none|
|»» ttlConfig|[TtlConfig](#schemattlconfig)|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» level|string|true|none|none|
|»»»» ttl|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_LoggerConfig">LoggerConfig</h2>
<!-- backwards compatibility -->
<a id="schemaloggerconfig"></a>
<a id="schema_LoggerConfig"></a>
<a id="tocSloggerconfig"></a>
<a id="tocsloggerconfig"></a>

```json
{
  "channels": [
    {
      "canDelete": true,
      "id": "string",
      "level": "string",
      "ttl": 0
    }
  ],
  "defaultRedactFields": [
    "string"
  ],
  "id": "string",
  "limitRate": 0,
  "maxSizeBytes": 0,
  "redactFields": [
    "string"
  ],
  "redactLabel": "string",
  "ttlConfig": {
    "property1": {
      "level": "string",
      "ttl": 0
    },
    "property2": {
      "level": "string",
      "ttl": 0
    }
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|channels|[[LoggerEntry](#schemaloggerentry)]|true|none|none|
|defaultRedactFields|[string]|false|none|none|
|id|string|true|none|none|
|limitRate|number|true|none|none|
|maxSizeBytes|number|true|none|none|
|redactFields|[string]|true|none|none|
|redactLabel|string|true|none|none|
|ttlConfig|[TtlConfig](#schemattlconfig)|false|none|none|

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

<h2 id="tocS_LoggerEntry">LoggerEntry</h2>
<!-- backwards compatibility -->
<a id="schemaloggerentry"></a>
<a id="schema_LoggerEntry"></a>
<a id="tocSloggerentry"></a>
<a id="tocsloggerentry"></a>

```json
{
  "canDelete": true,
  "id": "string",
  "level": "string",
  "ttl": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|canDelete|boolean|false|none|none|
|id|string|true|none|none|
|level|string|true|none|none|
|ttl|number|false|none|none|

<h2 id="tocS_TtlConfig">TtlConfig</h2>
<!-- backwards compatibility -->
<a id="schemattlconfig"></a>
<a id="schema_TtlConfig"></a>
<a id="tocSttlconfig"></a>
<a id="tocsttlconfig"></a>

```json
{
  "property1": {
    "level": "string",
    "ttl": 0
  },
  "property2": {
    "level": "string",
    "ttl": 0
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|**additionalProperties**|object|false|none|none|
|» level|string|true|none|none|
|» ttl|number|true|none|none|

