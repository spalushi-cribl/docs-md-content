
<h1 id="cribl-core-api-protobuf-libraries">Cribl Core API - Protobuf Libraries v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-protobuf-libraries-protobuf-libraries">Protobuf Libraries</h1>

## Show list of Protobuf encodings for a given Library

<a id="opIdgetProtobufLibraryConfigEncodingsById"></a>

> Code samples

`GET /lib/protobuf-libraries/{id}/encodings`

Show list of Protobuf encodings for a given Library

<h3 id="show-list-of-protobuf-encodings-for-a-given-library-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Library id|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "eventModel": "string",
      "id": "string",
      "name": "string",
      "wrapping": {
        "wrapperField": "string",
        "wrapperFieldType": "single",
        "wrapperModel": "string"
      }
    }
  ]
}
```

<h3 id="show-list-of-protobuf-encodings-for-a-given-library-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ProtobufEncodingConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="show-list-of-protobuf-encodings-for-a-given-library-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ProtobufEncodingConfig](#schemaprotobufencodingconfig)]|false|none|none|
|»» eventModel|string|true|none|none|
|»» id|string|true|none|none|
|»» name|string|true|none|none|
|»» wrapping|object|false|none|none|
|»»» wrapperField|string|true|none|none|
|»»» wrapperFieldType|string|true|none|none|
|»»» wrapperModel|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|wrapperFieldType|single|
|wrapperFieldType|array|

<aside class="success">
This operation does not require authentication
</aside>

## Show Library by Id

<a id="opIdgetProtobufLibraryConfigById"></a>

> Code samples

`GET /lib/protobuf-libraries/{id}`

Show Library by Id

<h3 id="show-library-by-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Library Id|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "availableEncodings": [
        {
          "eventModel": "string",
          "id": "string",
          "name": "string",
          "wrapping": {
            "wrapperField": "string",
            "wrapperFieldType": "single",
            "wrapperModel": "string"
          }
        }
      ],
      "conversion": {
        "arrays": true,
        "bytes": "buffer",
        "defaults": true,
        "enums": "string",
        "json": true,
        "longs": "number",
        "objects": true,
        "oneofs": true
      },
      "dependsOn": [
        "string"
      ],
      "description": "string",
      "id": "string",
      "name": "string",
      "tags": "string"
    }
  ]
}
```

<h3 id="show-library-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ProtobufLibraryConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="show-library-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ProtobufLibraryConfig](#schemaprotobuflibraryconfig)]|false|none|none|
|»» availableEncodings|[[ProtobufEncodingConfig](#schemaprotobufencodingconfig)]|false|none|none|
|»»» eventModel|string|true|none|none|
|»»» id|string|true|none|none|
|»»» name|string|true|none|none|
|»»» wrapping|object|false|none|none|
|»»»» wrapperField|string|true|none|none|
|»»»» wrapperFieldType|string|true|none|none|
|»»»» wrapperModel|string|true|none|none|
|»» conversion|[ProtobufLibraryConversionConfig](#schemaprotobuflibraryconversionconfig)|false|none|none|
|»»» arrays|boolean|false|none|none|
|»»» bytes|[ProtobufBytesConversion](#schemaprotobufbytesconversion)|false|none|none|
|»»» defaults|boolean|false|none|none|
|»»» enums|[ProtobufEnumConversion](#schemaprotobufenumconversion)|false|none|none|
|»»» json|boolean|false|none|none|
|»»» longs|[ProtobufLongConversion](#schemaprotobuflongconversion)|false|none|none|
|»»» objects|boolean|false|none|none|
|»»» oneofs|boolean|false|none|none|
|»» dependsOn|[string]|true|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» name|string|true|none|none|
|»» tags|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|wrapperFieldType|single|
|wrapperFieldType|array|
|bytes|buffer|
|bytes|array|
|bytes|string|
|enums|string|
|enums|number|
|longs|number|
|longs|string|
|longs|object|

<aside class="success">
This operation does not require authentication
</aside>

## Show Protobuf library encodings by encoding id

<a id="opIdgetProtobufLibraryConfigEncodingsByIdAndEncId"></a>

> Code samples

`GET /lib/protobuf-libraries/{id}/encodings/{encid}`

Show Protobuf library encodings by encoding id

<h3 id="show-protobuf-library-encodings-by-encoding-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Library id|
|encid|path|string|true|Encoding id|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "eventModel": "string",
      "id": "string",
      "name": "string",
      "wrapping": {
        "wrapperField": "string",
        "wrapperFieldType": "single",
        "wrapperModel": "string"
      }
    }
  ]
}
```

<h3 id="show-protobuf-library-encodings-by-encoding-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ProtobufEncodingConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="show-protobuf-library-encodings-by-encoding-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ProtobufEncodingConfig](#schemaprotobufencodingconfig)]|false|none|none|
|»» eventModel|string|true|none|none|
|»» id|string|true|none|none|
|»» name|string|true|none|none|
|»» wrapping|object|false|none|none|
|»»» wrapperField|string|true|none|none|
|»»» wrapperFieldType|string|true|none|none|
|»»» wrapperModel|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|wrapperFieldType|single|
|wrapperFieldType|array|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_ProtobufEncodingConfig">ProtobufEncodingConfig</h2>
<!-- backwards compatibility -->
<a id="schemaprotobufencodingconfig"></a>
<a id="schema_ProtobufEncodingConfig"></a>
<a id="tocSprotobufencodingconfig"></a>
<a id="tocsprotobufencodingconfig"></a>

```json
{
  "eventModel": "string",
  "id": "string",
  "name": "string",
  "wrapping": {
    "wrapperField": "string",
    "wrapperFieldType": "single",
    "wrapperModel": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|eventModel|string|true|none|none|
|id|string|true|none|none|
|name|string|true|none|none|
|wrapping|object|false|none|none|
|» wrapperField|string|true|none|none|
|» wrapperFieldType|string|true|none|none|
|» wrapperModel|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|wrapperFieldType|single|
|wrapperFieldType|array|

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

<h2 id="tocS_ProtobufLibraryConfig">ProtobufLibraryConfig</h2>
<!-- backwards compatibility -->
<a id="schemaprotobuflibraryconfig"></a>
<a id="schema_ProtobufLibraryConfig"></a>
<a id="tocSprotobuflibraryconfig"></a>
<a id="tocsprotobuflibraryconfig"></a>

```json
{
  "availableEncodings": [
    {
      "eventModel": "string",
      "id": "string",
      "name": "string",
      "wrapping": {
        "wrapperField": "string",
        "wrapperFieldType": "single",
        "wrapperModel": "string"
      }
    }
  ],
  "conversion": {
    "arrays": true,
    "bytes": "buffer",
    "defaults": true,
    "enums": "string",
    "json": true,
    "longs": "number",
    "objects": true,
    "oneofs": true
  },
  "dependsOn": [
    "string"
  ],
  "description": "string",
  "id": "string",
  "name": "string",
  "tags": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|availableEncodings|[[ProtobufEncodingConfig](#schemaprotobufencodingconfig)]|false|none|none|
|conversion|[ProtobufLibraryConversionConfig](#schemaprotobuflibraryconversionconfig)|false|none|none|
|dependsOn|[string]|true|none|none|
|description|string|true|none|none|
|id|string|true|none|none|
|name|string|true|none|none|
|tags|string|false|none|none|

<h2 id="tocS_ProtobufLibraryConversionConfig">ProtobufLibraryConversionConfig</h2>
<!-- backwards compatibility -->
<a id="schemaprotobuflibraryconversionconfig"></a>
<a id="schema_ProtobufLibraryConversionConfig"></a>
<a id="tocSprotobuflibraryconversionconfig"></a>
<a id="tocsprotobuflibraryconversionconfig"></a>

```json
{
  "arrays": true,
  "bytes": "buffer",
  "defaults": true,
  "enums": "string",
  "json": true,
  "longs": "number",
  "objects": true,
  "oneofs": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|arrays|boolean|false|none|none|
|bytes|[ProtobufBytesConversion](#schemaprotobufbytesconversion)|false|none|none|
|defaults|boolean|false|none|none|
|enums|[ProtobufEnumConversion](#schemaprotobufenumconversion)|false|none|none|
|json|boolean|false|none|none|
|longs|[ProtobufLongConversion](#schemaprotobuflongconversion)|false|none|none|
|objects|boolean|false|none|none|
|oneofs|boolean|false|none|none|

<h2 id="tocS_ProtobufBytesConversion">ProtobufBytesConversion</h2>
<!-- backwards compatibility -->
<a id="schemaprotobufbytesconversion"></a>
<a id="schema_ProtobufBytesConversion"></a>
<a id="tocSprotobufbytesconversion"></a>
<a id="tocsprotobufbytesconversion"></a>

```json
"buffer"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|buffer|
|*anonymous*|array|
|*anonymous*|string|

<h2 id="tocS_ProtobufEnumConversion">ProtobufEnumConversion</h2>
<!-- backwards compatibility -->
<a id="schemaprotobufenumconversion"></a>
<a id="schema_ProtobufEnumConversion"></a>
<a id="tocSprotobufenumconversion"></a>
<a id="tocsprotobufenumconversion"></a>

```json
"string"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|string|
|*anonymous*|number|

<h2 id="tocS_ProtobufLongConversion">ProtobufLongConversion</h2>
<!-- backwards compatibility -->
<a id="schemaprotobuflongconversion"></a>
<a id="schema_ProtobufLongConversion"></a>
<a id="tocSprotobuflongconversion"></a>
<a id="tocsprotobuflongconversion"></a>

```json
"number"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|number|
|*anonymous*|string|
|*anonymous*|object|

