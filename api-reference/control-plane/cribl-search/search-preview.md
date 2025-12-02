
<h1 id="cribl-search-api-search-preview">Cribl Search API - Search Preview v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-search-api-search-preview-search-preview">Search Preview</h1>

## Applies a query snippet on a set of input events for preview

<a id="opIdcreateSearchPreview"></a>

> Code samples

`POST /search/preview`

Applies a query snippet on a set of input events for preview

> Body parameter

```json
{
  "events": [
    {
      "_raw": {},
      "_time": 0
    }
  ],
  "options": {
    "earliest": "string",
    "latest": "string"
  },
  "query": "string"
}
```

<h3 id="applies-a-query-snippet-on-a-set-of-input-events-for-preview-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[PreviewRequestBody](#schemapreviewrequestbody)|true|PreviewRequestBody object|
|» events|body|[allOf]|true|none|
|»» _raw|body|object|true|none|
|»» _time|body|number|true|none|
|» options|body|[PreviewOptions](#schemapreviewoptions)|false|none|
|»» earliest|body|any|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|number|false|none|
|»» latest|body|any|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|number|false|none|
|» query|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "count": 0,
      "events": [
        {
          "_raw": {},
          "_time": 0
        }
      ],
      "processingTimeMS": 0,
      "useFormattedVisualization": true
    }
  ]
}
```

<h3 id="applies-a-query-snippet-on-a-set-of-input-events-for-preview-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PreviewResponseBody objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="applies-a-query-snippet-on-a-set-of-input-events-for-preview-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PreviewResponseBody](#schemapreviewresponsebody)]|false|none|none|
|»» count|number|true|none|none|
|»» events|[allOf]|true|none|none|
|»»» _raw|object|true|none|none|
|»»» _time|number|true|none|none|
|»» processingTimeMS|number|true|none|none|
|»» useFormattedVisualization|boolean|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Runs an event breaker rule on the specified data

<a id="opIdcreateSearchEventBreakerPreview"></a>

> Code samples

`POST /search/event-breaker-preview`

Runs an event breaker rule on the specified data

> Body parameter

```json
{
  "eventBreakerRule": {
    "cleanFields": true,
    "condition": "string",
    "delimiter": "string",
    "delimiterRegex": "string",
    "disabled": true,
    "escapeChar": "string",
    "eventBreakerRegex": "string",
    "fields": [
      {
        "name": "string",
        "value": "string"
      }
    ],
    "fieldsLineRegex": "string",
    "headerLineRegex": "string",
    "index": 0,
    "jsonArrayField": "string",
    "jsonExtractAll": true,
    "jsonTimeField": "string",
    "maxEventBytes": 0,
    "name": "string",
    "nullFieldVal": "string",
    "parentFieldsToCopy": [
      "string"
    ],
    "parser": {
      "allowedKeyChars": true,
      "allowedValueChars": [
        "string"
      ],
      "cleanFields": [
        "string"
      ],
      "delimChar": "string",
      "dstField": "string",
      "escapeChar": "string",
      "fieldFilterExpr": "string",
      "fields": [
        "string"
      ],
      "keep": [
        "string"
      ],
      "mode": "extract",
      "nullValue": "string",
      "quoteChar": "string",
      "remove": [
        "string"
      ],
      "srcField": "string",
      "type": "clf"
    },
    "parserEnabled": true,
    "quoteChar": "string",
    "shouldUseDataRaw": true,
    "timeField": "string",
    "timestamp": {
      "format": "string",
      "length": 0,
      "type": "auto"
    },
    "timestampAnchorRegex": "string",
    "timestampEarliest": "string",
    "timestampLatest": "string",
    "timestampTimezone": "string",
    "type": "regex"
  },
  "input": {
    "dataset": "string",
    "type": "dataset"
  }
}
```

<h3 id="runs-an-event-breaker-rule-on-the-specified-data-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[DatatypePreviewRequestBody](#schemadatatypepreviewrequestbody)|true|DatatypePreviewRequestBody object|
|» eventBreakerRule|body|[EventBreakerRule](#schemaeventbreakerrule)|false|none|
|»» cleanFields|body|boolean|false|none|
|»» condition|body|string|true|none|
|»» delimiter|body|string|false|none|
|»» delimiterRegex|body|string|false|none|
|»» disabled|body|boolean|false|none|
|»» escapeChar|body|string|false|none|
|»» eventBreakerRegex|body|string|false|none|
|»» fields|body|[[EventBreakerRuleFields](#schemaeventbreakerrulefields)]|false|none|
|»»» name|body|string|true|none|
|»»» value|body|string|true|none|
|»» fieldsLineRegex|body|string|false|none|
|»» headerLineRegex|body|string|false|none|
|»» index|body|number|false|none|
|»» jsonArrayField|body|string|false|none|
|»» jsonExtractAll|body|boolean|false|none|
|»» jsonTimeField|body|string|false|none|
|»» maxEventBytes|body|number|true|none|
|»» name|body|string|true|none|
|»» nullFieldVal|body|string|false|none|
|»» parentFieldsToCopy|body|[string]|false|none|
|»» parser|body|any|false|none|
|»»» *anonymous*|body|object|false|none|
|»»»» allowedKeyChars|body|boolean|false|none|
|»»»» allowedValueChars|body|[string]|false|none|
|»»»» cleanFields|body|[string]|false|none|
|»»»» delimChar|body|string|false|none|
|»»»» dstField|body|string|false|none|
|»»»» escapeChar|body|string|false|none|
|»»»» fieldFilterExpr|body|string|false|none|
|»»»» fields|body|[string]|false|none|
|»»»» keep|body|[string]|false|none|
|»»»» mode|body|[ParserMode](#schemaparsermode)|true|none|
|»»»» nullValue|body|string|false|none|
|»»»» quoteChar|body|string|false|none|
|»»»» remove|body|[string]|false|none|
|»»»» srcField|body|string|true|none|
|»»»» type|body|string|true|none|
|»»» *anonymous*|body|object|false|none|
|»»»» dstField|body|string|false|none|
|»»»» mode|body|[ParserMode](#schemaparsermode)|true|none|
|»»»» pattern|body|string|false|none|
|»»»» patternList|body|[object]|false|none|
|»»»»» pattern|body|string|true|none|
|»»»» source|body|string|false|none|
|»»»» srcField|body|string|true|none|
|»»»» type|body|string|true|none|
|»»» *anonymous*|body|object|false|none|
|»»»» cleanFields|body|[string]|false|none|
|»»»» dstField|body|string|false|none|
|»»»» fieldFilterExpr|body|string|false|none|
|»»»» fields|body|[string]|false|none|
|»»»» keep|body|[string]|false|none|
|»»»» mode|body|[ParserMode](#schemaparsermode)|true|none|
|»»»» remove|body|[string]|false|none|
|»»»» srcField|body|string|true|none|
|»»»» type|body|string|true|none|
|»»» *anonymous*|body|object|false|none|
|»»»» allowedKeyChars|body|boolean|false|none|
|»»»» allowedValueChars|body|[string]|false|none|
|»»»» cleanFields|body|[string]|false|none|
|»»»» delimChar|body|string|false|none|
|»»»» dstField|body|string|false|none|
|»»»» escapeChar|body|string|false|none|
|»»»» fieldFilterExpr|body|string|false|none|
|»»»» fields|body|[string]|false|none|
|»»»» keep|body|[string]|false|none|
|»»»» mode|body|[ParserMode](#schemaparsermode)|true|none|
|»»»» nullValue|body|string|false|none|
|»»»» quoteChar|body|string|false|none|
|»»»» remove|body|[string]|false|none|
|»»»» srcField|body|string|true|none|
|»»»» type|body|string|true|none|
|»»» *anonymous*|body|any|false|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» dstField|body|string|false|none|
|»»»»» mode|body|[ParserMode](#schemaparsermode)|true|none|
|»»»»» srcField|body|string|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» fieldNameExpression|body|string|false|none|
|»»»»» iterations|body|number|false|none|
|»»»»» overwrite|body|boolean|false|none|
|»»»»» regex|body|string|true|none|
|»»»»» regexList|body|[[RegexContainer](#schemaregexcontainer)]|false|none|
|»»»»»» regex|body|string|true|none|
|»»»»» srcField|body|string|false|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» type|body|string|true|none|
|»» parserEnabled|body|boolean|false|none|
|»» quoteChar|body|string|false|none|
|»» shouldUseDataRaw|body|boolean|false|none|
|»» timeField|body|string|false|none|
|»» timestamp|body|object|true|none|
|»»» format|body|string|false|none|
|»»» length|body|number|false|none|
|»»» type|body|string|true|none|
|»» timestampAnchorRegex|body|string|true|none|
|»» timestampEarliest|body|string|false|none|
|»» timestampLatest|body|string|false|none|
|»» timestampTimezone|body|any|true|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|object|false|none|
|»»»» name|body|string|true|none|
|»»»» offsets|body|[number]|true|none|
|»»»» untils|body|[number]|true|none|
|»» type|body|string|false|none|
|» input|body|any|true|none|
|»» *anonymous*|body|object|false|none|
|»»» dataset|body|string|true|none|
|»»» type|body|string|true|none|
|»» *anonymous*|body|object|false|none|
|»»» rawData|body|string|true|none|
|»»» type|body|string|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»»» mode|extract|
|»»»» mode|reserialize|
|»»»» type|clf|
|»»»» type|csv|
|»»»» type|delim|
|»»»» type|elff|
|»»»» mode|extract|
|»»»» mode|reserialize|
|»»»» type|grok|
|»»»» mode|extract|
|»»»» mode|reserialize|
|»»»» type|json|
|»»»» mode|extract|
|»»»» mode|reserialize|
|»»»» type|kvp|
|»»»»» mode|extract|
|»»»»» mode|reserialize|
|»»»»» type|regex|
|»»» type|auto|
|»»» type|format|
|»»» type|current|
|»» type|regex|
|»» type|timestamp|
|»» type|json|
|»» type|csv|
|»» type|json_array|
|»» type|header|
|»» type|aws_cloudtrail|
|»» type|aws_vpcflow|
|»»» type|dataset|
|»»» type|rawData|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "count": 0,
      "events": [
        {
          "_raw": {},
          "_time": 0
        }
      ],
      "processingTimeMS": 0,
      "useFormattedVisualization": true
    }
  ]
}
```

<h3 id="runs-an-event-breaker-rule-on-the-specified-data-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PreviewResponseBody objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="runs-an-event-breaker-rule-on-the-specified-data-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PreviewResponseBody](#schemapreviewresponsebody)]|false|none|none|
|»» count|number|true|none|none|
|»» events|[allOf]|true|none|none|
|»»» _raw|object|true|none|none|
|»»» _time|number|true|none|none|
|»» processingTimeMS|number|true|none|none|
|»» useFormattedVisualization|boolean|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_PreviewResponseBody">PreviewResponseBody</h2>
<!-- backwards compatibility -->
<a id="schemapreviewresponsebody"></a>
<a id="schema_PreviewResponseBody"></a>
<a id="tocSpreviewresponsebody"></a>
<a id="tocspreviewresponsebody"></a>

```json
{
  "count": 0,
  "events": [
    {
      "_raw": {},
      "_time": 0
    }
  ],
  "processingTimeMS": 0,
  "useFormattedVisualization": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|count|number|true|none|none|
|events|[[SearchEvent](#schemasearchevent)]|true|none|none|
|processingTimeMS|number|true|none|none|
|useFormattedVisualization|boolean|true|none|none|

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

<h2 id="tocS_PreviewRequestBody">PreviewRequestBody</h2>
<!-- backwards compatibility -->
<a id="schemapreviewrequestbody"></a>
<a id="schema_PreviewRequestBody"></a>
<a id="tocSpreviewrequestbody"></a>
<a id="tocspreviewrequestbody"></a>

```json
{
  "events": [
    {
      "_raw": {},
      "_time": 0
    }
  ],
  "options": {
    "earliest": "string",
    "latest": "string"
  },
  "query": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|events|[[SearchEvent](#schemasearchevent)]|true|none|none|
|options|[PreviewOptions](#schemapreviewoptions)|false|none|none|
|query|string|true|none|none|

<h2 id="tocS_DatatypePreviewRequestBody">DatatypePreviewRequestBody</h2>
<!-- backwards compatibility -->
<a id="schemadatatypepreviewrequestbody"></a>
<a id="schema_DatatypePreviewRequestBody"></a>
<a id="tocSdatatypepreviewrequestbody"></a>
<a id="tocsdatatypepreviewrequestbody"></a>

```json
{
  "eventBreakerRule": {
    "cleanFields": true,
    "condition": "string",
    "delimiter": "string",
    "delimiterRegex": "string",
    "disabled": true,
    "escapeChar": "string",
    "eventBreakerRegex": "string",
    "fields": [
      {
        "name": "string",
        "value": "string"
      }
    ],
    "fieldsLineRegex": "string",
    "headerLineRegex": "string",
    "index": 0,
    "jsonArrayField": "string",
    "jsonExtractAll": true,
    "jsonTimeField": "string",
    "maxEventBytes": 0,
    "name": "string",
    "nullFieldVal": "string",
    "parentFieldsToCopy": [
      "string"
    ],
    "parser": {
      "allowedKeyChars": true,
      "allowedValueChars": [
        "string"
      ],
      "cleanFields": [
        "string"
      ],
      "delimChar": "string",
      "dstField": "string",
      "escapeChar": "string",
      "fieldFilterExpr": "string",
      "fields": [
        "string"
      ],
      "keep": [
        "string"
      ],
      "mode": "extract",
      "nullValue": "string",
      "quoteChar": "string",
      "remove": [
        "string"
      ],
      "srcField": "string",
      "type": "clf"
    },
    "parserEnabled": true,
    "quoteChar": "string",
    "shouldUseDataRaw": true,
    "timeField": "string",
    "timestamp": {
      "format": "string",
      "length": 0,
      "type": "auto"
    },
    "timestampAnchorRegex": "string",
    "timestampEarliest": "string",
    "timestampLatest": "string",
    "timestampTimezone": "string",
    "type": "regex"
  },
  "input": {
    "dataset": "string",
    "type": "dataset"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|eventBreakerRule|[EventBreakerRule](#schemaeventbreakerrule)|false|none|none|
|input|[DatatypePreviewInput](#schemadatatypepreviewinput)|true|none|none|

<h2 id="tocS_SearchEvent">SearchEvent</h2>
<!-- backwards compatibility -->
<a id="schemasearchevent"></a>
<a id="schema_SearchEvent"></a>
<a id="tocSsearchevent"></a>
<a id="tocssearchevent"></a>

```json
{
  "_raw": {},
  "_time": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|_raw|object|true|none|none|
|_time|number|true|none|none|

<h2 id="tocS_PreviewOptions">PreviewOptions</h2>
<!-- backwards compatibility -->
<a id="schemapreviewoptions"></a>
<a id="schema_PreviewOptions"></a>
<a id="tocSpreviewoptions"></a>
<a id="tocspreviewoptions"></a>

```json
{
  "earliest": "string",
  "latest": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|earliest|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|latest|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

<h2 id="tocS_EventBreakerRule">EventBreakerRule</h2>
<!-- backwards compatibility -->
<a id="schemaeventbreakerrule"></a>
<a id="schema_EventBreakerRule"></a>
<a id="tocSeventbreakerrule"></a>
<a id="tocseventbreakerrule"></a>

```json
{
  "cleanFields": true,
  "condition": "string",
  "delimiter": "string",
  "delimiterRegex": "string",
  "disabled": true,
  "escapeChar": "string",
  "eventBreakerRegex": "string",
  "fields": [
    {
      "name": "string",
      "value": "string"
    }
  ],
  "fieldsLineRegex": "string",
  "headerLineRegex": "string",
  "index": 0,
  "jsonArrayField": "string",
  "jsonExtractAll": true,
  "jsonTimeField": "string",
  "maxEventBytes": 0,
  "name": "string",
  "nullFieldVal": "string",
  "parentFieldsToCopy": [
    "string"
  ],
  "parser": {
    "allowedKeyChars": true,
    "allowedValueChars": [
      "string"
    ],
    "cleanFields": [
      "string"
    ],
    "delimChar": "string",
    "dstField": "string",
    "escapeChar": "string",
    "fieldFilterExpr": "string",
    "fields": [
      "string"
    ],
    "keep": [
      "string"
    ],
    "mode": "extract",
    "nullValue": "string",
    "quoteChar": "string",
    "remove": [
      "string"
    ],
    "srcField": "string",
    "type": "clf"
  },
  "parserEnabled": true,
  "quoteChar": "string",
  "shouldUseDataRaw": true,
  "timeField": "string",
  "timestamp": {
    "format": "string",
    "length": 0,
    "type": "auto"
  },
  "timestampAnchorRegex": "string",
  "timestampEarliest": "string",
  "timestampLatest": "string",
  "timestampTimezone": "string",
  "type": "regex"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cleanFields|boolean|false|none|none|
|condition|string|true|none|none|
|delimiter|string|false|none|none|
|delimiterRegex|string|false|none|none|
|disabled|boolean|false|none|none|
|escapeChar|string|false|none|none|
|eventBreakerRegex|string|false|none|none|
|fields|[[EventBreakerRuleFields](#schemaeventbreakerrulefields)]|false|none|none|
|fieldsLineRegex|string|false|none|none|
|headerLineRegex|string|false|none|none|
|index|number|false|none|none|
|jsonArrayField|string|false|none|none|
|jsonExtractAll|boolean|false|none|none|
|jsonTimeField|string|false|none|none|
|maxEventBytes|number|true|none|none|
|name|string|true|none|none|
|nullFieldVal|string|false|none|none|
|parentFieldsToCopy|[string]|false|none|none|
|parser|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» allowedKeyChars|boolean|false|none|none|
|»» allowedValueChars|[string]|false|none|none|
|»» cleanFields|[string]|false|none|none|
|»» delimChar|string|false|none|none|
|»» dstField|string|false|none|none|
|»» escapeChar|string|false|none|none|
|»» fieldFilterExpr|string|false|none|none|
|»» fields|[string]|false|none|none|
|»» keep|[string]|false|none|none|
|»» mode|[ParserMode](#schemaparsermode)|true|none|none|
|»» nullValue|string|false|none|none|
|»» quoteChar|string|false|none|none|
|»» remove|[string]|false|none|none|
|»» srcField|string|true|none|none|
|»» type|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» dstField|string|false|none|none|
|»» mode|[ParserMode](#schemaparsermode)|true|none|none|
|»» pattern|string|false|none|none|
|»» patternList|[object]|false|none|none|
|»»» pattern|string|true|none|none|
|»» source|string|false|none|none|
|»» srcField|string|true|none|none|
|»» type|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» cleanFields|[string]|false|none|none|
|»» dstField|string|false|none|none|
|»» fieldFilterExpr|string|false|none|none|
|»» fields|[string]|false|none|none|
|»» keep|[string]|false|none|none|
|»» mode|[ParserMode](#schemaparsermode)|true|none|none|
|»» remove|[string]|false|none|none|
|»» srcField|string|true|none|none|
|»» type|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» allowedKeyChars|boolean|false|none|none|
|»» allowedValueChars|[string]|false|none|none|
|»» cleanFields|[string]|false|none|none|
|»» delimChar|string|false|none|none|
|»» dstField|string|false|none|none|
|»» escapeChar|string|false|none|none|
|»» fieldFilterExpr|string|false|none|none|
|»» fields|[string]|false|none|none|
|»» keep|[string]|false|none|none|
|»» mode|[ParserMode](#schemaparsermode)|true|none|none|
|»» nullValue|string|false|none|none|
|»» quoteChar|string|false|none|none|
|»» remove|[string]|false|none|none|
|»» srcField|string|true|none|none|
|»» type|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|any|false|none|none|

allOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» dstField|string|false|none|none|
|»»» mode|[ParserMode](#schemaparsermode)|true|none|none|
|»»» srcField|string|true|none|none|

and

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» fieldNameExpression|string|false|none|none|
|»»» iterations|number|false|none|none|
|»»» overwrite|boolean|false|none|none|
|»»» regex|string|true|none|none|
|»»» regexList|[[RegexContainer](#schemaregexcontainer)]|false|none|none|
|»»» srcField|string|false|none|none|

and

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» type|string|true|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|parserEnabled|boolean|false|none|none|
|quoteChar|string|false|none|none|
|shouldUseDataRaw|boolean|false|none|none|
|timeField|string|false|none|none|
|timestamp|object|true|none|none|
|» format|string|false|none|none|
|» length|number|false|none|none|
|» type|string|true|none|none|
|timestampAnchorRegex|string|true|none|none|
|timestampEarliest|string|false|none|none|
|timestampLatest|string|false|none|none|
|timestampTimezone|any|true|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» name|string|true|none|none|
|»» offsets|[number]|true|none|none|
|»» untils|[number]|true|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|type|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|clf|
|type|csv|
|type|delim|
|type|elff|
|type|grok|
|type|json|
|type|kvp|
|type|regex|
|type|auto|
|type|format|
|type|current|
|type|regex|
|type|timestamp|
|type|json|
|type|csv|
|type|json_array|
|type|header|
|type|aws_cloudtrail|
|type|aws_vpcflow|

<h2 id="tocS_DatatypePreviewInput">DatatypePreviewInput</h2>
<!-- backwards compatibility -->
<a id="schemadatatypepreviewinput"></a>
<a id="schema_DatatypePreviewInput"></a>
<a id="tocSdatatypepreviewinput"></a>
<a id="tocsdatatypepreviewinput"></a>

```json
{
  "dataset": "string",
  "type": "dataset"
}

```

### Properties

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» dataset|string|true|none|none|
|» type|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» rawData|string|true|none|none|
|» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|dataset|
|type|rawData|

<h2 id="tocS_EventBreakerRuleFields">EventBreakerRuleFields</h2>
<!-- backwards compatibility -->
<a id="schemaeventbreakerrulefields"></a>
<a id="schema_EventBreakerRuleFields"></a>
<a id="tocSeventbreakerrulefields"></a>
<a id="tocseventbreakerrulefields"></a>

```json
{
  "name": "string",
  "value": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|true|none|none|
|value|string|true|none|none|

<h2 id="tocS_ParserMode">ParserMode</h2>
<!-- backwards compatibility -->
<a id="schemaparsermode"></a>
<a id="schema_ParserMode"></a>
<a id="tocSparsermode"></a>
<a id="tocsparsermode"></a>

```json
"extract"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|extract|
|*anonymous*|reserialize|

<h2 id="tocS_RegexContainer">RegexContainer</h2>
<!-- backwards compatibility -->
<a id="schemaregexcontainer"></a>
<a id="schema_RegexContainer"></a>
<a id="tocSregexcontainer"></a>
<a id="tocsregexcontainer"></a>

```json
{
  "regex": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|regex|string|true|none|none|

