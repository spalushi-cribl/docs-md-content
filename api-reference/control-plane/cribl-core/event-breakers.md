
<h1 id="cribl-core-api-event-breakers">Cribl Core API - Event Breakers v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-event-breakers-event-breakers">Event Breakers</h1>

## Get a list of Event Breaker Ruleset objects

<a id="opIdlistEventBreakerRuleset"></a>

> Code samples

`GET /lib/breakers`

Get a list of Event Breaker Ruleset objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "lib": "custom",
      "description": "string",
      "tags": "string",
      "minRawLength": 256,
      "rules": [
        {
          "name": "string",
          "condition": "true",
          "type": "regex",
          "timestampAnchorRegex": "/^/",
          "timestamp": {
            "type": "auto",
            "length": 150,
            "format": "string"
          },
          "timestampTimezone": "local",
          "timestampEarliest": "-420weeks",
          "timestampLatest": "+1week",
          "maxEventBytes": 51200,
          "fields": [
            {
              "name": "string",
              "value": "string"
            }
          ],
          "disabled": false,
          "parserEnabled": false,
          "shouldUseDataRaw": false
        }
      ]
    }
  ]
}
```

<h3 id="get-a-list-of-event-breaker-ruleset-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Event Breaker Ruleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-event-breaker-ruleset-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|none|
|»» minRawLength|number|false|none|The  minimum number of characters in _raw to determine which rule to use|
|»» rules|[object]|false|none|A list of rules that will be applied, in order, to the input data stream|
|»»» name|string|true|none|none|
|»»» condition|string|true|none|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»»» type|string|true|none|none|
|»»» timestampAnchorRegex|string|true|none|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»»» timestamp|object|true|none|Auto, manual format (strptime), or current time|
|»»»» type|string|true|none|none|
|»»»» length|number|false|none|none|
|»»»» format|string|false|none|none|
|»»» timestampTimezone|string|false|none|Timezone to assign to timestamps without timezone info|
|»»» timestampEarliest|string|false|none|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»»» timestampLatest|string|false|none|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»»» maxEventBytes|number|false|none|The maximum number of bytes in an event before it is flushed to the pipelines|
|»»» fields|[object]|false|none|Key-value pairs to be added to each event|
|»»»» name|string|false|none|none|
|»»»» value|string|true|none|The JavaScript expression used to compute the field's value (can be constant)|
|»»» disabled|boolean|false|none|Disable this breaker rule (enabled by default)|
|»»» parserEnabled|boolean|false|none|none|
|»»» shouldUseDataRaw|boolean|false|none|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|

#### Enumerated Values

|Property|Value|
|---|---|
|lib|custom|
|lib|cribl-custom|
|type|regex|
|type|json|
|type|json_array|
|type|header|
|type|timestamp|
|type|csv|
|type|aws_cloudtrail|
|type|aws_vpcflow|
|type|auto|
|type|format|
|type|current|

<aside class="success">
This operation does not require authentication
</aside>

## Create Event Breaker Ruleset

<a id="opIdcreateEventBreakerRuleset"></a>

> Code samples

`POST /lib/breakers`

Create Event Breaker Ruleset

> Body parameter

```json
{
  "id": "string",
  "lib": "custom",
  "description": "string",
  "tags": "string",
  "minRawLength": 256,
  "rules": [
    {
      "name": "string",
      "condition": "true",
      "type": "regex",
      "timestampAnchorRegex": "/^/",
      "timestamp": {
        "type": "auto",
        "length": 150,
        "format": "string"
      },
      "timestampTimezone": "local",
      "timestampEarliest": "-420weeks",
      "timestampLatest": "+1week",
      "maxEventBytes": 51200,
      "fields": [
        {
          "name": "string",
          "value": "string"
        }
      ],
      "disabled": false,
      "parserEnabled": false,
      "shouldUseDataRaw": false
    }
  ]
}
```

<h3 id="create-event-breaker-ruleset-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[EventBreakerRuleset](#schemaeventbreakerruleset)|true|New Event Breaker Ruleset object|
|» id|body|string|true|none|
|» lib|body|string|false|none|
|» description|body|string|false|none|
|» tags|body|string|false|none|
|» minRawLength|body|number|false|The  minimum number of characters in _raw to determine which rule to use|
|» rules|body|[object]|false|A list of rules that will be applied, in order, to the input data stream|
|»» name|body|string|true|none|
|»» condition|body|string|true|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»» type|body|string|true|none|
|»» timestampAnchorRegex|body|string|true|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»» timestamp|body|object|true|Auto, manual format (strptime), or current time|
|»»» type|body|string|true|none|
|»»» length|body|number|false|none|
|»»» format|body|string|false|none|
|»» timestampTimezone|body|string|false|Timezone to assign to timestamps without timezone info|
|»» timestampEarliest|body|string|false|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»» timestampLatest|body|string|false|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»» maxEventBytes|body|number|false|The maximum number of bytes in an event before it is flushed to the pipelines|
|»» fields|body|[object]|false|Key-value pairs to be added to each event|
|»»» name|body|string|false|none|
|»»» value|body|string|true|The JavaScript expression used to compute the field's value (can be constant)|
|»» disabled|body|boolean|false|Disable this breaker rule (enabled by default)|
|»» parserEnabled|body|boolean|false|none|
|»» shouldUseDataRaw|body|boolean|false|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» lib|custom|
|» lib|cribl-custom|
|»» type|regex|
|»» type|json|
|»» type|json_array|
|»» type|header|
|»» type|timestamp|
|»» type|csv|
|»» type|aws_cloudtrail|
|»» type|aws_vpcflow|
|»»» type|auto|
|»»» type|format|
|»»» type|current|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "lib": "custom",
      "description": "string",
      "tags": "string",
      "minRawLength": 256,
      "rules": [
        {
          "name": "string",
          "condition": "true",
          "type": "regex",
          "timestampAnchorRegex": "/^/",
          "timestamp": {
            "type": "auto",
            "length": 150,
            "format": "string"
          },
          "timestampTimezone": "local",
          "timestampEarliest": "-420weeks",
          "timestampLatest": "+1week",
          "maxEventBytes": 51200,
          "fields": [
            {
              "name": "string",
              "value": "string"
            }
          ],
          "disabled": false,
          "parserEnabled": false,
          "shouldUseDataRaw": false
        }
      ]
    }
  ]
}
```

<h3 id="create-event-breaker-ruleset-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Event Breaker Ruleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-event-breaker-ruleset-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|none|
|»» minRawLength|number|false|none|The  minimum number of characters in _raw to determine which rule to use|
|»» rules|[object]|false|none|A list of rules that will be applied, in order, to the input data stream|
|»»» name|string|true|none|none|
|»»» condition|string|true|none|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»»» type|string|true|none|none|
|»»» timestampAnchorRegex|string|true|none|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»»» timestamp|object|true|none|Auto, manual format (strptime), or current time|
|»»»» type|string|true|none|none|
|»»»» length|number|false|none|none|
|»»»» format|string|false|none|none|
|»»» timestampTimezone|string|false|none|Timezone to assign to timestamps without timezone info|
|»»» timestampEarliest|string|false|none|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»»» timestampLatest|string|false|none|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»»» maxEventBytes|number|false|none|The maximum number of bytes in an event before it is flushed to the pipelines|
|»»» fields|[object]|false|none|Key-value pairs to be added to each event|
|»»»» name|string|false|none|none|
|»»»» value|string|true|none|The JavaScript expression used to compute the field's value (can be constant)|
|»»» disabled|boolean|false|none|Disable this breaker rule (enabled by default)|
|»»» parserEnabled|boolean|false|none|none|
|»»» shouldUseDataRaw|boolean|false|none|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|

#### Enumerated Values

|Property|Value|
|---|---|
|lib|custom|
|lib|cribl-custom|
|type|regex|
|type|json|
|type|json_array|
|type|header|
|type|timestamp|
|type|csv|
|type|aws_cloudtrail|
|type|aws_vpcflow|
|type|auto|
|type|format|
|type|current|

<aside class="success">
This operation does not require authentication
</aside>

## Delete Event Breaker Ruleset

<a id="opIddeleteEventBreakerRulesetById"></a>

> Code samples

`DELETE /lib/breakers/{id}`

Delete Event Breaker Ruleset

<h3 id="delete-event-breaker-ruleset-parameters">Parameters</h3>

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
      "lib": "custom",
      "description": "string",
      "tags": "string",
      "minRawLength": 256,
      "rules": [
        {
          "name": "string",
          "condition": "true",
          "type": "regex",
          "timestampAnchorRegex": "/^/",
          "timestamp": {
            "type": "auto",
            "length": 150,
            "format": "string"
          },
          "timestampTimezone": "local",
          "timestampEarliest": "-420weeks",
          "timestampLatest": "+1week",
          "maxEventBytes": 51200,
          "fields": [
            {
              "name": "string",
              "value": "string"
            }
          ],
          "disabled": false,
          "parserEnabled": false,
          "shouldUseDataRaw": false
        }
      ]
    }
  ]
}
```

<h3 id="delete-event-breaker-ruleset-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Event Breaker Ruleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-event-breaker-ruleset-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|none|
|»» minRawLength|number|false|none|The  minimum number of characters in _raw to determine which rule to use|
|»» rules|[object]|false|none|A list of rules that will be applied, in order, to the input data stream|
|»»» name|string|true|none|none|
|»»» condition|string|true|none|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»»» type|string|true|none|none|
|»»» timestampAnchorRegex|string|true|none|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»»» timestamp|object|true|none|Auto, manual format (strptime), or current time|
|»»»» type|string|true|none|none|
|»»»» length|number|false|none|none|
|»»»» format|string|false|none|none|
|»»» timestampTimezone|string|false|none|Timezone to assign to timestamps without timezone info|
|»»» timestampEarliest|string|false|none|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»»» timestampLatest|string|false|none|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»»» maxEventBytes|number|false|none|The maximum number of bytes in an event before it is flushed to the pipelines|
|»»» fields|[object]|false|none|Key-value pairs to be added to each event|
|»»»» name|string|false|none|none|
|»»»» value|string|true|none|The JavaScript expression used to compute the field's value (can be constant)|
|»»» disabled|boolean|false|none|Disable this breaker rule (enabled by default)|
|»»» parserEnabled|boolean|false|none|none|
|»»» shouldUseDataRaw|boolean|false|none|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|

#### Enumerated Values

|Property|Value|
|---|---|
|lib|custom|
|lib|cribl-custom|
|type|regex|
|type|json|
|type|json_array|
|type|header|
|type|timestamp|
|type|csv|
|type|aws_cloudtrail|
|type|aws_vpcflow|
|type|auto|
|type|format|
|type|current|

<aside class="success">
This operation does not require authentication
</aside>

## Update Event Breaker Ruleset

<a id="opIdupdateEventBreakerRulesetById"></a>

> Code samples

`PATCH /lib/breakers/{id}`

Update Event Breaker Ruleset

> Body parameter

```json
{
  "id": "string",
  "lib": "custom",
  "description": "string",
  "tags": "string",
  "minRawLength": 256,
  "rules": [
    {
      "name": "string",
      "condition": "true",
      "type": "regex",
      "timestampAnchorRegex": "/^/",
      "timestamp": {
        "type": "auto",
        "length": 150,
        "format": "string"
      },
      "timestampTimezone": "local",
      "timestampEarliest": "-420weeks",
      "timestampLatest": "+1week",
      "maxEventBytes": 51200,
      "fields": [
        {
          "name": "string",
          "value": "string"
        }
      ],
      "disabled": false,
      "parserEnabled": false,
      "shouldUseDataRaw": false
    }
  ]
}
```

<h3 id="update-event-breaker-ruleset-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[EventBreakerRuleset](#schemaeventbreakerruleset)|true|Event Breaker Ruleset object to be updated|
|» id|body|string|true|none|
|» lib|body|string|false|none|
|» description|body|string|false|none|
|» tags|body|string|false|none|
|» minRawLength|body|number|false|The  minimum number of characters in _raw to determine which rule to use|
|» rules|body|[object]|false|A list of rules that will be applied, in order, to the input data stream|
|»» name|body|string|true|none|
|»» condition|body|string|true|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»» type|body|string|true|none|
|»» timestampAnchorRegex|body|string|true|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»» timestamp|body|object|true|Auto, manual format (strptime), or current time|
|»»» type|body|string|true|none|
|»»» length|body|number|false|none|
|»»» format|body|string|false|none|
|»» timestampTimezone|body|string|false|Timezone to assign to timestamps without timezone info|
|»» timestampEarliest|body|string|false|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»» timestampLatest|body|string|false|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»» maxEventBytes|body|number|false|The maximum number of bytes in an event before it is flushed to the pipelines|
|»» fields|body|[object]|false|Key-value pairs to be added to each event|
|»»» name|body|string|false|none|
|»»» value|body|string|true|The JavaScript expression used to compute the field's value (can be constant)|
|»» disabled|body|boolean|false|Disable this breaker rule (enabled by default)|
|»» parserEnabled|body|boolean|false|none|
|»» shouldUseDataRaw|body|boolean|false|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» lib|custom|
|» lib|cribl-custom|
|»» type|regex|
|»» type|json|
|»» type|json_array|
|»» type|header|
|»» type|timestamp|
|»» type|csv|
|»» type|aws_cloudtrail|
|»» type|aws_vpcflow|
|»»» type|auto|
|»»» type|format|
|»»» type|current|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "lib": "custom",
      "description": "string",
      "tags": "string",
      "minRawLength": 256,
      "rules": [
        {
          "name": "string",
          "condition": "true",
          "type": "regex",
          "timestampAnchorRegex": "/^/",
          "timestamp": {
            "type": "auto",
            "length": 150,
            "format": "string"
          },
          "timestampTimezone": "local",
          "timestampEarliest": "-420weeks",
          "timestampLatest": "+1week",
          "maxEventBytes": 51200,
          "fields": [
            {
              "name": "string",
              "value": "string"
            }
          ],
          "disabled": false,
          "parserEnabled": false,
          "shouldUseDataRaw": false
        }
      ]
    }
  ]
}
```

<h3 id="update-event-breaker-ruleset-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Event Breaker Ruleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-event-breaker-ruleset-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|none|
|»» minRawLength|number|false|none|The  minimum number of characters in _raw to determine which rule to use|
|»» rules|[object]|false|none|A list of rules that will be applied, in order, to the input data stream|
|»»» name|string|true|none|none|
|»»» condition|string|true|none|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»»» type|string|true|none|none|
|»»» timestampAnchorRegex|string|true|none|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»»» timestamp|object|true|none|Auto, manual format (strptime), or current time|
|»»»» type|string|true|none|none|
|»»»» length|number|false|none|none|
|»»»» format|string|false|none|none|
|»»» timestampTimezone|string|false|none|Timezone to assign to timestamps without timezone info|
|»»» timestampEarliest|string|false|none|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»»» timestampLatest|string|false|none|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»»» maxEventBytes|number|false|none|The maximum number of bytes in an event before it is flushed to the pipelines|
|»»» fields|[object]|false|none|Key-value pairs to be added to each event|
|»»»» name|string|false|none|none|
|»»»» value|string|true|none|The JavaScript expression used to compute the field's value (can be constant)|
|»»» disabled|boolean|false|none|Disable this breaker rule (enabled by default)|
|»»» parserEnabled|boolean|false|none|none|
|»»» shouldUseDataRaw|boolean|false|none|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|

#### Enumerated Values

|Property|Value|
|---|---|
|lib|custom|
|lib|cribl-custom|
|type|regex|
|type|json|
|type|json_array|
|type|header|
|type|timestamp|
|type|csv|
|type|aws_cloudtrail|
|type|aws_vpcflow|
|type|auto|
|type|format|
|type|current|

<aside class="success">
This operation does not require authentication
</aside>

## Get Event Breaker Ruleset by ID

<a id="opIdgetEventBreakerRulesetById"></a>

> Code samples

`GET /lib/breakers/{id}`

Get Event Breaker Ruleset by ID

<h3 id="get-event-breaker-ruleset-by-id-parameters">Parameters</h3>

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
      "lib": "custom",
      "description": "string",
      "tags": "string",
      "minRawLength": 256,
      "rules": [
        {
          "name": "string",
          "condition": "true",
          "type": "regex",
          "timestampAnchorRegex": "/^/",
          "timestamp": {
            "type": "auto",
            "length": 150,
            "format": "string"
          },
          "timestampTimezone": "local",
          "timestampEarliest": "-420weeks",
          "timestampLatest": "+1week",
          "maxEventBytes": 51200,
          "fields": [
            {
              "name": "string",
              "value": "string"
            }
          ],
          "disabled": false,
          "parserEnabled": false,
          "shouldUseDataRaw": false
        }
      ]
    }
  ]
}
```

<h3 id="get-event-breaker-ruleset-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Event Breaker Ruleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-event-breaker-ruleset-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|none|
|»» minRawLength|number|false|none|The  minimum number of characters in _raw to determine which rule to use|
|»» rules|[object]|false|none|A list of rules that will be applied, in order, to the input data stream|
|»»» name|string|true|none|none|
|»»» condition|string|true|none|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»»» type|string|true|none|none|
|»»» timestampAnchorRegex|string|true|none|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»»» timestamp|object|true|none|Auto, manual format (strptime), or current time|
|»»»» type|string|true|none|none|
|»»»» length|number|false|none|none|
|»»»» format|string|false|none|none|
|»»» timestampTimezone|string|false|none|Timezone to assign to timestamps without timezone info|
|»»» timestampEarliest|string|false|none|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»»» timestampLatest|string|false|none|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»»» maxEventBytes|number|false|none|The maximum number of bytes in an event before it is flushed to the pipelines|
|»»» fields|[object]|false|none|Key-value pairs to be added to each event|
|»»»» name|string|false|none|none|
|»»»» value|string|true|none|The JavaScript expression used to compute the field's value (can be constant)|
|»»» disabled|boolean|false|none|Disable this breaker rule (enabled by default)|
|»»» parserEnabled|boolean|false|none|none|
|»»» shouldUseDataRaw|boolean|false|none|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|

#### Enumerated Values

|Property|Value|
|---|---|
|lib|custom|
|lib|cribl-custom|
|type|regex|
|type|json|
|type|json_array|
|type|header|
|type|timestamp|
|type|csv|
|type|aws_cloudtrail|
|type|aws_vpcflow|
|type|auto|
|type|format|
|type|current|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_EventBreakerRuleset">EventBreakerRuleset</h2>
<!-- backwards compatibility -->
<a id="schemaeventbreakerruleset"></a>
<a id="schema_EventBreakerRuleset"></a>
<a id="tocSeventbreakerruleset"></a>
<a id="tocseventbreakerruleset"></a>

```json
{
  "id": "string",
  "lib": "custom",
  "description": "string",
  "tags": "string",
  "minRawLength": 256,
  "rules": [
    {
      "name": "string",
      "condition": "true",
      "type": "regex",
      "timestampAnchorRegex": "/^/",
      "timestamp": {
        "type": "auto",
        "length": 150,
        "format": "string"
      },
      "timestampTimezone": "local",
      "timestampEarliest": "-420weeks",
      "timestampLatest": "+1week",
      "maxEventBytes": 51200,
      "fields": [
        {
          "name": "string",
          "value": "string"
        }
      ],
      "disabled": false,
      "parserEnabled": false,
      "shouldUseDataRaw": false
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|lib|string|false|none|none|
|description|string|false|none|none|
|tags|string|false|none|none|
|minRawLength|number|false|none|The  minimum number of characters in _raw to determine which rule to use|
|rules|[object]|false|none|A list of rules that will be applied, in order, to the input data stream|
|» name|string|true|none|none|
|» condition|string|true|none|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|» type|string|true|none|none|
|» timestampAnchorRegex|string|true|none|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|» timestamp|object|true|none|Auto, manual format (strptime), or current time|
|»» type|string|true|none|none|
|»» length|number|false|none|none|
|»» format|string|false|none|none|
|» timestampTimezone|string|false|none|Timezone to assign to timestamps without timezone info|
|» timestampEarliest|string|false|none|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|» timestampLatest|string|false|none|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|» maxEventBytes|number|false|none|The maximum number of bytes in an event before it is flushed to the pipelines|
|» fields|[object]|false|none|Key-value pairs to be added to each event|
|»» name|string|false|none|none|
|»» value|string|true|none|The JavaScript expression used to compute the field's value (can be constant)|
|» disabled|boolean|false|none|Disable this breaker rule (enabled by default)|
|» parserEnabled|boolean|false|none|none|
|» shouldUseDataRaw|boolean|false|none|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|

#### Enumerated Values

|Property|Value|
|---|---|
|lib|custom|
|lib|cribl-custom|
|type|regex|
|type|json|
|type|json_array|
|type|header|
|type|timestamp|
|type|csv|
|type|aws_cloudtrail|
|type|aws_vpcflow|
|type|auto|
|type|format|
|type|current|

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

