
<h1 id="cribl-stream-api-clickhouse-integration">Cribl Stream API - ClickHouse Integration v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-clickhouse-integration-clickhouse-integration">ClickHouse Integration</h1>

## Retrieve the description of the configured ClickHouse table

<a id="opIdcreateOutputClickHouseDescribeTable"></a>

> Code samples

`POST /output/click-house/describe-table`

Retrieve the description of the configured ClickHouse table

> Body parameter

```json
{
  "asyncInserts": true,
  "auth": {
    "disabled": true,
    "password": "string",
    "token": "string",
    "username": "string"
  },
  "authType": "token",
  "columnMappings": [
    {
      "columnName": "string",
      "columnType": "string",
      "columnValueExpression": "string"
    }
  ],
  "compress": true,
  "concurrency": 0,
  "database": "string",
  "dumpFormatErrorsToDisk": true,
  "excludeMappingFields": [
    "string"
  ],
  "extraHttpHeaders": [
    {
      "name": "string",
      "value": {
        "Create": [
          {
            "actions": [
              "string"
            ],
            "object": "string"
          }
        ],
        "Delete": [
          {
            "actions": [
              "string"
            ],
            "object": "string"
          }
        ],
        "List": [
          {
            "actions": [
              "string"
            ],
            "object": "string"
          }
        ],
        "Read": [
          {
            "actions": [
              "string"
            ],
            "object": "string"
          }
        ],
        "Replace": [
          {
            "actions": [
              "string"
            ],
            "object": "string"
          }
        ],
        "Update": [
          {
            "actions": [
              "string"
            ],
            "object": "string"
          }
        ]
      }
    }
  ],
  "extraParams": [
    {
      "name": "string",
      "value": {}
    }
  ],
  "failedRequestLoggingMode": "string",
  "flushPeriodSec": 0,
  "format": "json-compact-each-row-with-names",
  "keepAlive": true,
  "loadBalanced": true,
  "mappingType": "automatic",
  "maxConnectionReuseSec": 0,
  "maxPayloadEvents": 0,
  "maxPayloadSizeKB": 0,
  "maxSockets": 0,
  "method": "string",
  "password": "string",
  "rejectUnauthorized": true,
  "responseHonorRetryAfterHeader": true,
  "responseRetrySettings": [
    {
      "backoffRate": 0,
      "disableJitter": true,
      "httpStatus": 0,
      "initialBackoff": 0,
      "maxBackoff": 0
    }
  ],
  "safeHeaders": [
    "string"
  ],
  "sqlUsername": "string",
  "tableName": "string",
  "tableNameExpression": "string",
  "timeoutRetrySettings": {
    "backoffRate": 0,
    "disableJitter": true,
    "initialBackoff": 0,
    "maxBackoff": 0
  },
  "timeoutSec": 0,
  "tls": {
    "caPath": "string",
    "certPath": "string",
    "checkServerIdentity": {},
    "disabled": true,
    "maxVersion": "TLSv1.3",
    "minVersion": "TLSv1.3",
    "passphrase": "string",
    "privKeyPath": "string",
    "rejectUnauthorized": true,
    "servername": "string"
  },
  "token": "string",
  "totalMemoryLimitKB": 0,
  "url": "string",
  "urls": [],
  "useRoundRobinDns": true,
  "username": "string",
  "waitForAsyncInserts": true
}
```

<h3 id="retrieve-the-description-of-the-configured-clickhouse-table-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[CHOutConfig](#schemachoutconfig)|true|CHOutConfig object|
|» asyncInserts|body|boolean|true|none|
|» auth|body|[HTTPOutAuthConfig](#schemahttpoutauthconfig)|false|none|
|»» disabled|body|boolean|false|none|
|»» password|body|string|false|none|
|»» token|body|string|false|none|
|»» username|body|string|false|none|
|» authType|body|string|false|none|
|» columnMappings|body|[object]|false|none|
|»» columnName|body|string|true|none|
|»» columnType|body|string|true|none|
|»» columnValueExpression|body|string|true|none|
|» compress|body|boolean|false|none|
|» concurrency|body|number|false|none|
|» database|body|string|true|none|
|» dumpFormatErrorsToDisk|body|boolean|false|none|
|» excludeMappingFields|body|[string]|false|none|
|» extraHttpHeaders|body|[[NameValue](#schemanamevalue)]|false|none|
|»» name|body|string|true|none|
|»» value|body|[CrudPolicy](#schemacrudpolicy)|true|none|
|»»» Create|body|[[AuthPolicyEntry](#schemaauthpolicyentry)]|true|none|
|»»»» actions|body|[string]|true|none|
|»»»» object|body|string|true|none|
|»»» Delete|body|[[AuthPolicyEntry](#schemaauthpolicyentry)]|true|none|
|»»» List|body|[[AuthPolicyEntry](#schemaauthpolicyentry)]|true|none|
|»»» Read|body|[[AuthPolicyEntry](#schemaauthpolicyentry)]|true|none|
|»»» Replace|body|[[AuthPolicyEntry](#schemaauthpolicyentry)]|true|none|
|»»» Update|body|[[AuthPolicyEntry](#schemaauthpolicyentry)]|true|none|
|» extraParams|body|[[HTTPOutExtraParamConfig](#schemahttpoutextraparamconfig)]|false|none|
|»» name|body|string|true|none|
|»» value|body|object|true|none|
|» failedRequestLoggingMode|body|string|false|none|
|» flushPeriodSec|body|number|true|none|
|» format|body|[Format](#schemaformat)|true|none|
|» keepAlive|body|boolean|false|none|
|» loadBalanced|body|boolean|true|none|
|» mappingType|body|[MappingType](#schemamappingtype)|true|none|
|» maxConnectionReuseSec|body|number|false|none|
|» maxPayloadEvents|body|number|false|none|
|» maxPayloadSizeKB|body|number|false|none|
|» maxSockets|body|number|false|none|
|» method|body|string|false|none|
|» password|body|string|false|none|
|» rejectUnauthorized|body|boolean|false|none|
|» responseHonorRetryAfterHeader|body|boolean|false|none|
|» responseRetrySettings|body|[[HTTPOutResponseRetryConfig](#schemahttpoutresponseretryconfig)]|false|none|
|»» backoffRate|body|number|false|none|
|»» disableJitter|body|boolean|false|none|
|»» httpStatus|body|number|false|none|
|»» initialBackoff|body|number|false|none|
|»» maxBackoff|body|number|false|none|
|» safeHeaders|body|[string]|false|none|
|» sqlUsername|body|string|false|none|
|» tableName|body|string|true|none|
|» tableNameExpression|body|string|false|none|
|» timeoutRetrySettings|body|[RetryBackoffOptions](#schemaretrybackoffoptions)|false|none|
|»» backoffRate|body|number|false|none|
|»» disableJitter|body|boolean|false|none|
|»» initialBackoff|body|number|false|none|
|»» maxBackoff|body|number|false|none|
|» timeoutSec|body|number|false|none|
|» tls|body|[TLSClientParams](#schematlsclientparams)|false|none|
|»» caPath|body|string|false|none|
|»» certPath|body|string|false|none|
|»» checkServerIdentity|body|object|false|none|
|»» disabled|body|boolean|true|none|
|»» maxVersion|body|[SecureVersion](#schemasecureversion)|false|none|
|»» minVersion|body|[SecureVersion](#schemasecureversion)|false|none|
|»» passphrase|body|string|false|none|
|»» privKeyPath|body|string|false|none|
|»» rejectUnauthorized|body|boolean|false|none|
|»» servername|body|string|false|none|
|» token|body|string|false|none|
|» totalMemoryLimitKB|body|number|false|none|
|» url|body|string|true|none|
|» urls|body|array|false|none|
|» useRoundRobinDns|body|boolean|false|none|
|» username|body|string|false|none|
|» waitForAsyncInserts|body|boolean|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» authType|token|
|» authType|none|
|» authType|textSecret|
|» authType|basic|
|» authType|credentialsSecret|
|» authType|secret|
|» authType|manual|
|» authType|manualAPIKey|
|» authType|sslUserCertificate|
|» format|json-compact-each-row-with-names|
|» format|json-each-row|
|» mappingType|automatic|
|» mappingType|custom|
|»» maxVersion|TLSv1.3|
|»» maxVersion|TLSv1.2|
|»» maxVersion|TLSv1.1|
|»» maxVersion|TLSv1|
|»» minVersion|TLSv1.3|
|»» minVersion|TLSv1.2|
|»» minVersion|TLSv1.1|
|»» minVersion|TLSv1|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": [
        {
          "name": "string",
          "type": "string"
        }
      ],
      "errorMsg": "string",
      "success": true
    }
  ]
}
```

<h3 id="retrieve-the-description-of-the-configured-clickhouse-table-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ClickHouseDescriptionResult objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="retrieve-the-description-of-the-configured-clickhouse-table-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ClickHouseDescriptionResult](#schemaclickhousedescriptionresult)]|false|none|none|
|»» description|[[ClickHouseDescriptionColumn](#schemaclickhousedescriptioncolumn)]|false|none|none|
|»»» name|string|true|none|none|
|»»» type|string|true|none|none|
|»» errorMsg|string|false|none|none|
|»» success|boolean|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_ClickHouseDescriptionResult">ClickHouseDescriptionResult</h2>
<!-- backwards compatibility -->
<a id="schemaclickhousedescriptionresult"></a>
<a id="schema_ClickHouseDescriptionResult"></a>
<a id="tocSclickhousedescriptionresult"></a>
<a id="tocsclickhousedescriptionresult"></a>

```json
{
  "description": [
    {
      "name": "string",
      "type": "string"
    }
  ],
  "errorMsg": "string",
  "success": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|description|[[ClickHouseDescriptionColumn](#schemaclickhousedescriptioncolumn)]|false|none|none|
|errorMsg|string|false|none|none|
|success|boolean|true|none|none|

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

<h2 id="tocS_CHOutConfig">CHOutConfig</h2>
<!-- backwards compatibility -->
<a id="schemachoutconfig"></a>
<a id="schema_CHOutConfig"></a>
<a id="tocSchoutconfig"></a>
<a id="tocschoutconfig"></a>

```json
{
  "asyncInserts": true,
  "auth": {
    "disabled": true,
    "password": "string",
    "token": "string",
    "username": "string"
  },
  "authType": "token",
  "columnMappings": [
    {
      "columnName": "string",
      "columnType": "string",
      "columnValueExpression": "string"
    }
  ],
  "compress": true,
  "concurrency": 0,
  "database": "string",
  "dumpFormatErrorsToDisk": true,
  "excludeMappingFields": [
    "string"
  ],
  "extraHttpHeaders": [
    {
      "name": "string",
      "value": {
        "Create": [
          {
            "actions": [
              "string"
            ],
            "object": "string"
          }
        ],
        "Delete": [
          {
            "actions": [
              "string"
            ],
            "object": "string"
          }
        ],
        "List": [
          {
            "actions": [
              "string"
            ],
            "object": "string"
          }
        ],
        "Read": [
          {
            "actions": [
              "string"
            ],
            "object": "string"
          }
        ],
        "Replace": [
          {
            "actions": [
              "string"
            ],
            "object": "string"
          }
        ],
        "Update": [
          {
            "actions": [
              "string"
            ],
            "object": "string"
          }
        ]
      }
    }
  ],
  "extraParams": [
    {
      "name": "string",
      "value": {}
    }
  ],
  "failedRequestLoggingMode": "string",
  "flushPeriodSec": 0,
  "format": "json-compact-each-row-with-names",
  "keepAlive": true,
  "loadBalanced": true,
  "mappingType": "automatic",
  "maxConnectionReuseSec": 0,
  "maxPayloadEvents": 0,
  "maxPayloadSizeKB": 0,
  "maxSockets": 0,
  "method": "string",
  "password": "string",
  "rejectUnauthorized": true,
  "responseHonorRetryAfterHeader": true,
  "responseRetrySettings": [
    {
      "backoffRate": 0,
      "disableJitter": true,
      "httpStatus": 0,
      "initialBackoff": 0,
      "maxBackoff": 0
    }
  ],
  "safeHeaders": [
    "string"
  ],
  "sqlUsername": "string",
  "tableName": "string",
  "tableNameExpression": "string",
  "timeoutRetrySettings": {
    "backoffRate": 0,
    "disableJitter": true,
    "initialBackoff": 0,
    "maxBackoff": 0
  },
  "timeoutSec": 0,
  "tls": {
    "caPath": "string",
    "certPath": "string",
    "checkServerIdentity": {},
    "disabled": true,
    "maxVersion": "TLSv1.3",
    "minVersion": "TLSv1.3",
    "passphrase": "string",
    "privKeyPath": "string",
    "rejectUnauthorized": true,
    "servername": "string"
  },
  "token": "string",
  "totalMemoryLimitKB": 0,
  "url": "string",
  "urls": [],
  "useRoundRobinDns": true,
  "username": "string",
  "waitForAsyncInserts": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|asyncInserts|boolean|true|none|none|
|auth|[HTTPOutAuthConfig](#schemahttpoutauthconfig)|false|none|none|
|authType|string|false|none|none|
|columnMappings|[object]|false|none|none|
|» columnName|string|true|none|none|
|» columnType|string|true|none|none|
|» columnValueExpression|string|true|none|none|
|compress|boolean|false|none|none|
|concurrency|number|false|none|none|
|database|string|true|none|none|
|dumpFormatErrorsToDisk|boolean|false|none|none|
|excludeMappingFields|[string]|false|none|none|
|extraHttpHeaders|[[NameValue](#schemanamevalue)]|false|none|none|
|extraParams|[[HTTPOutExtraParamConfig](#schemahttpoutextraparamconfig)]|false|none|none|
|failedRequestLoggingMode|string|false|none|none|
|flushPeriodSec|number|true|none|none|
|format|[Format](#schemaformat)|true|none|none|
|keepAlive|boolean|false|none|none|
|loadBalanced|boolean|true|none|none|
|mappingType|[MappingType](#schemamappingtype)|true|none|none|
|maxConnectionReuseSec|number|false|none|none|
|maxPayloadEvents|number|false|none|none|
|maxPayloadSizeKB|number|false|none|none|
|maxSockets|number|false|none|none|
|method|string|false|none|none|
|password|string|false|none|none|
|rejectUnauthorized|boolean|false|none|none|
|responseHonorRetryAfterHeader|boolean|false|none|none|
|responseRetrySettings|[[HTTPOutResponseRetryConfig](#schemahttpoutresponseretryconfig)]|false|none|none|
|safeHeaders|[string]|false|none|none|
|sqlUsername|string|false|none|none|
|tableName|string|true|none|none|
|tableNameExpression|string|false|none|none|
|timeoutRetrySettings|[RetryBackoffOptions](#schemaretrybackoffoptions)|false|none|none|
|timeoutSec|number|false|none|none|
|tls|[TLSClientParams](#schematlsclientparams)|false|none|none|
|token|string|false|none|none|
|totalMemoryLimitKB|number|false|none|none|
|url|string|true|none|none|
|urls|array|false|none|none|
|useRoundRobinDns|boolean|false|none|none|
|username|string|false|none|none|
|waitForAsyncInserts|boolean|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|authType|token|
|authType|none|
|authType|textSecret|
|authType|basic|
|authType|credentialsSecret|
|authType|secret|
|authType|manual|
|authType|manualAPIKey|
|authType|sslUserCertificate|

<h2 id="tocS_ClickHouseDescriptionColumn">ClickHouseDescriptionColumn</h2>
<!-- backwards compatibility -->
<a id="schemaclickhousedescriptioncolumn"></a>
<a id="schema_ClickHouseDescriptionColumn"></a>
<a id="tocSclickhousedescriptioncolumn"></a>
<a id="tocsclickhousedescriptioncolumn"></a>

```json
{
  "name": "string",
  "type": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|true|none|none|
|type|string|true|none|none|

<h2 id="tocS_HTTPOutAuthConfig">HTTPOutAuthConfig</h2>
<!-- backwards compatibility -->
<a id="schemahttpoutauthconfig"></a>
<a id="schema_HTTPOutAuthConfig"></a>
<a id="tocShttpoutauthconfig"></a>
<a id="tocshttpoutauthconfig"></a>

```json
{
  "disabled": true,
  "password": "string",
  "token": "string",
  "username": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|disabled|boolean|false|none|none|
|password|string|false|none|none|
|token|string|false|none|none|
|username|string|false|none|none|

<h2 id="tocS_NameValue">NameValue</h2>
<!-- backwards compatibility -->
<a id="schemanamevalue"></a>
<a id="schema_NameValue"></a>
<a id="tocSnamevalue"></a>
<a id="tocsnamevalue"></a>

```json
{
  "name": "string",
  "value": {
    "Create": [
      {
        "actions": [
          "string"
        ],
        "object": "string"
      }
    ],
    "Delete": [
      {
        "actions": [
          "string"
        ],
        "object": "string"
      }
    ],
    "List": [
      {
        "actions": [
          "string"
        ],
        "object": "string"
      }
    ],
    "Read": [
      {
        "actions": [
          "string"
        ],
        "object": "string"
      }
    ],
    "Replace": [
      {
        "actions": [
          "string"
        ],
        "object": "string"
      }
    ],
    "Update": [
      {
        "actions": [
          "string"
        ],
        "object": "string"
      }
    ]
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|true|none|none|
|value|[Expression](#schemaexpression)|true|none|none|

<h2 id="tocS_HTTPOutExtraParamConfig">HTTPOutExtraParamConfig</h2>
<!-- backwards compatibility -->
<a id="schemahttpoutextraparamconfig"></a>
<a id="schema_HTTPOutExtraParamConfig"></a>
<a id="tocShttpoutextraparamconfig"></a>
<a id="tocshttpoutextraparamconfig"></a>

```json
{
  "name": "string",
  "value": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|true|none|none|
|value|object|true|none|none|

<h2 id="tocS_Format">Format</h2>
<!-- backwards compatibility -->
<a id="schemaformat"></a>
<a id="schema_Format"></a>
<a id="tocSformat"></a>
<a id="tocsformat"></a>

```json
"json-compact-each-row-with-names"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|json-compact-each-row-with-names|
|*anonymous*|json-each-row|

<h2 id="tocS_MappingType">MappingType</h2>
<!-- backwards compatibility -->
<a id="schemamappingtype"></a>
<a id="schema_MappingType"></a>
<a id="tocSmappingtype"></a>
<a id="tocsmappingtype"></a>

```json
"automatic"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|automatic|
|*anonymous*|custom|

<h2 id="tocS_HTTPOutResponseRetryConfig">HTTPOutResponseRetryConfig</h2>
<!-- backwards compatibility -->
<a id="schemahttpoutresponseretryconfig"></a>
<a id="schema_HTTPOutResponseRetryConfig"></a>
<a id="tocShttpoutresponseretryconfig"></a>
<a id="tocshttpoutresponseretryconfig"></a>

```json
{
  "backoffRate": 0,
  "disableJitter": true,
  "httpStatus": 0,
  "initialBackoff": 0,
  "maxBackoff": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|backoffRate|number|false|none|none|
|disableJitter|boolean|false|none|none|
|httpStatus|number|false|none|none|
|initialBackoff|number|false|none|none|
|maxBackoff|number|false|none|none|

<h2 id="tocS_RetryBackoffOptions">RetryBackoffOptions</h2>
<!-- backwards compatibility -->
<a id="schemaretrybackoffoptions"></a>
<a id="schema_RetryBackoffOptions"></a>
<a id="tocSretrybackoffoptions"></a>
<a id="tocsretrybackoffoptions"></a>

```json
{
  "backoffRate": 0,
  "disableJitter": true,
  "initialBackoff": 0,
  "maxBackoff": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|backoffRate|number|false|none|none|
|disableJitter|boolean|false|none|none|
|initialBackoff|number|false|none|none|
|maxBackoff|number|false|none|none|

<h2 id="tocS_TLSClientParams">TLSClientParams</h2>
<!-- backwards compatibility -->
<a id="schematlsclientparams"></a>
<a id="schema_TLSClientParams"></a>
<a id="tocStlsclientparams"></a>
<a id="tocstlsclientparams"></a>

```json
{
  "caPath": "string",
  "certPath": "string",
  "checkServerIdentity": {},
  "disabled": true,
  "maxVersion": "TLSv1.3",
  "minVersion": "TLSv1.3",
  "passphrase": "string",
  "privKeyPath": "string",
  "rejectUnauthorized": true,
  "servername": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|caPath|string|false|none|none|
|certPath|string|false|none|none|
|checkServerIdentity|object|false|none|none|
|disabled|boolean|true|none|none|
|maxVersion|[SecureVersion](#schemasecureversion)|false|none|none|
|minVersion|[SecureVersion](#schemasecureversion)|false|none|none|
|passphrase|string|false|none|none|
|privKeyPath|string|false|none|none|
|rejectUnauthorized|boolean|false|none|none|
|servername|string|false|none|none|

<h2 id="tocS_Expression">Expression</h2>
<!-- backwards compatibility -->
<a id="schemaexpression"></a>
<a id="schema_Expression"></a>
<a id="tocSexpression"></a>
<a id="tocsexpression"></a>

```json
{
  "Create": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ],
  "Delete": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ],
  "List": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ],
  "Read": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ],
  "Replace": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ],
  "Update": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ]
}

```

### Properties

*None*

<h2 id="tocS_SecureVersion">SecureVersion</h2>
<!-- backwards compatibility -->
<a id="schemasecureversion"></a>
<a id="schema_SecureVersion"></a>
<a id="tocSsecureversion"></a>
<a id="tocssecureversion"></a>

```json
"TLSv1.3"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|TLSv1.3|
|*anonymous*|TLSv1.2|
|*anonymous*|TLSv1.1|
|*anonymous*|TLSv1|

<h2 id="tocS_CrudPolicy">CrudPolicy</h2>
<!-- backwards compatibility -->
<a id="schemacrudpolicy"></a>
<a id="schema_CrudPolicy"></a>
<a id="tocScrudpolicy"></a>
<a id="tocscrudpolicy"></a>

```json
{
  "Create": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ],
  "Delete": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ],
  "List": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ],
  "Read": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ],
  "Replace": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ],
  "Update": [
    {
      "actions": [
        "string"
      ],
      "object": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Create|[[AuthPolicyEntry](#schemaauthpolicyentry)]|true|none|none|
|Delete|[[AuthPolicyEntry](#schemaauthpolicyentry)]|true|none|none|
|List|[[AuthPolicyEntry](#schemaauthpolicyentry)]|true|none|none|
|Read|[[AuthPolicyEntry](#schemaauthpolicyentry)]|true|none|none|
|Replace|[[AuthPolicyEntry](#schemaauthpolicyentry)]|true|none|none|
|Update|[[AuthPolicyEntry](#schemaauthpolicyentry)]|true|none|none|

<h2 id="tocS_AuthPolicyEntry">AuthPolicyEntry</h2>
<!-- backwards compatibility -->
<a id="schemaauthpolicyentry"></a>
<a id="schema_AuthPolicyEntry"></a>
<a id="tocSauthpolicyentry"></a>
<a id="tocsauthpolicyentry"></a>

```json
{
  "actions": [
    "string"
  ],
  "object": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|actions|[string]|true|none|none|
|object|string|true|none|none|

