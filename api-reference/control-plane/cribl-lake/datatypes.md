
<h1 id="cribl-lake-api-datatypes">Cribl Lake API - Datatypes v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-lake-api-datatypes-datatypes">Datatypes</h1>

## Get a list of DataTypeDescriptor objects

<a id="opIdlistDataTypeDescriptor"></a>

> Code samples

`GET /search/datatypes`

Get a list of DataTypeDescriptor objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "addFields": [
        {
          "fieldName": "string",
          "source": "string"
        }
      ],
      "breakerType": "parquet",
      "description": "string",
      "extractions": [
        {
          "name": "string",
          "sourceField": "string",
          "type": "csv",
          "delimiter": "string",
          "escape": "string",
          "fieldList": [
            "string"
          ],
          "nullValue": "string",
          "quote": "string"
        }
      ],
      "id": "string",
      "lib": "cribl",
      "schemaMap": [
        {
          "fieldName": "string",
          "source": "string"
        }
      ],
      "tags": "string",
      "timestampExtraction": {
        "anchorRegex": "string",
        "earliest": "string",
        "latest": "string",
        "sourceField": "string",
        "timezone": "string",
        "type": "auto",
        "scanDepth": 0
      }
    }
  ]
}
```

<h3 id="get-a-list-of-datatypedescriptor-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DataTypeDescriptor objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-datatypedescriptor-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DataTypeDescriptor](#schemadatatypedescriptor)]|false|none|none|
|»» addFields|[[FieldMappingType](#schemafieldmappingtype)]|false|none|none|
|»»» fieldName|string|true|none|none|
|»»» source|string|true|none|none|
|»» breakerType|[EventBreakerType](#schemaeventbreakertype)|true|none|none|
|»» description|string|false|none|none|
|»» extractions|[allOf]|false|none|none|

*allOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» name|string|true|none|none|
|»»»» sourceField|string|true|none|none|
|»»»» type|[ExtractionType](#schemaextractiontype)|true|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» delimiter|string|true|none|none|
|»»»»» escape|string|true|none|none|
|»»»»» fieldList|[string]|false|none|none|
|»»»»» nullValue|string|true|none|none|
|»»»»» quote|string|true|none|none|
|»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» regexpList|[object]|true|none|none|
|»»»»»» overwrite|boolean|false|none|none|
|»»»»»» regexp|string|true|none|none|
|»»»»» type|string|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» schemaMap|[[FieldMappingType](#schemafieldmappingtype)]|false|none|none|
|»» tags|string|false|none|none|
|»» timestampExtraction|any|true|none|none|

*allOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» anchorRegex|string|false|none|none|
|»»»» earliest|string|false|none|none|
|»»»» latest|string|false|none|none|
|»»»» sourceField|string|false|none|none|
|»»»» timezone|string|false|none|none|
|»»»» type|[TimestampExtractionType](#schematimestampextractiontype)|true|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» scanDepth|number|true|none|none|
|»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» format|string|true|none|none|
|»»»»» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|breakerType|parquet|
|breakerType|ndjson|
|breakerType|csv|
|type|csv|
|type|regexp|
|type|csv|
|type|regexp|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|
|type|auto|
|type|manual|
|type|auto|
|type|manual|

<aside class="success">
This operation does not require authentication
</aside>

## Create DataTypeDescriptor

<a id="opIdcreateDataTypeDescriptor"></a>

> Code samples

`POST /search/datatypes`

Create DataTypeDescriptor

> Body parameter

```json
{
  "addFields": [
    {
      "fieldName": "string",
      "source": "string"
    }
  ],
  "breakerType": "parquet",
  "description": "string",
  "extractions": [
    {
      "name": "string",
      "sourceField": "string",
      "type": "csv",
      "delimiter": "string",
      "escape": "string",
      "fieldList": [
        "string"
      ],
      "nullValue": "string",
      "quote": "string"
    }
  ],
  "id": "string",
  "lib": "cribl",
  "schemaMap": [
    {
      "fieldName": "string",
      "source": "string"
    }
  ],
  "tags": "string",
  "timestampExtraction": {
    "anchorRegex": "string",
    "earliest": "string",
    "latest": "string",
    "sourceField": "string",
    "timezone": "string",
    "type": "auto",
    "scanDepth": 0
  }
}
```

<h3 id="create-datatypedescriptor-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[DataTypeDescriptor](#schemadatatypedescriptor)|true|New DataTypeDescriptor object|
|» addFields|body|[[FieldMappingType](#schemafieldmappingtype)]|false|none|
|»» fieldName|body|string|true|none|
|»» source|body|string|true|none|
|» breakerType|body|[EventBreakerType](#schemaeventbreakertype)|true|none|
|» description|body|string|false|none|
|» extractions|body|[allOf]|false|none|
|»» *anonymous*|body|object|false|none|
|»»» name|body|string|true|none|
|»»» sourceField|body|string|true|none|
|»»» type|body|[ExtractionType](#schemaextractiontype)|true|none|
|»» *anonymous*|body|any|false|none|
|»»» *anonymous*|body|object|false|none|
|»»»» delimiter|body|string|true|none|
|»»»» escape|body|string|true|none|
|»»»» fieldList|body|[string]|false|none|
|»»»» nullValue|body|string|true|none|
|»»»» quote|body|string|true|none|
|»»»» type|body|string|true|none|
|»»» *anonymous*|body|object|false|none|
|»»»» regexpList|body|[object]|true|none|
|»»»»» overwrite|body|boolean|false|none|
|»»»»» regexp|body|string|true|none|
|»»»» type|body|string|true|none|
|» id|body|string|true|none|
|» lib|body|[CriblLib](#schemacribllib)|true|none|
|» schemaMap|body|[[FieldMappingType](#schemafieldmappingtype)]|false|none|
|» tags|body|string|false|none|
|» timestampExtraction|body|any|true|none|
|»» *anonymous*|body|object|false|none|
|»»» anchorRegex|body|string|false|none|
|»»» earliest|body|string|false|none|
|»»» latest|body|string|false|none|
|»»» sourceField|body|string|false|none|
|»»» timezone|body|string|false|none|
|»»» type|body|[TimestampExtractionType](#schematimestampextractiontype)|true|none|
|»» *anonymous*|body|any|false|none|
|»»» *anonymous*|body|object|false|none|
|»»»» scanDepth|body|number|true|none|
|»»»» type|body|string|true|none|
|»»» *anonymous*|body|object|false|none|
|»»»» format|body|string|true|none|
|»»»» type|body|string|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» breakerType|parquet|
|» breakerType|ndjson|
|» breakerType|csv|
|»»» type|csv|
|»»» type|regexp|
|»»»» type|csv|
|»»»» type|regexp|
|» lib|cribl|
|» lib|cribl-custom|
|» lib|custom|
|»»» type|auto|
|»»» type|manual|
|»»»» type|auto|
|»»»» type|manual|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "addFields": [
        {
          "fieldName": "string",
          "source": "string"
        }
      ],
      "breakerType": "parquet",
      "description": "string",
      "extractions": [
        {
          "name": "string",
          "sourceField": "string",
          "type": "csv",
          "delimiter": "string",
          "escape": "string",
          "fieldList": [
            "string"
          ],
          "nullValue": "string",
          "quote": "string"
        }
      ],
      "id": "string",
      "lib": "cribl",
      "schemaMap": [
        {
          "fieldName": "string",
          "source": "string"
        }
      ],
      "tags": "string",
      "timestampExtraction": {
        "anchorRegex": "string",
        "earliest": "string",
        "latest": "string",
        "sourceField": "string",
        "timezone": "string",
        "type": "auto",
        "scanDepth": 0
      }
    }
  ]
}
```

<h3 id="create-datatypedescriptor-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DataTypeDescriptor objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-datatypedescriptor-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DataTypeDescriptor](#schemadatatypedescriptor)]|false|none|none|
|»» addFields|[[FieldMappingType](#schemafieldmappingtype)]|false|none|none|
|»»» fieldName|string|true|none|none|
|»»» source|string|true|none|none|
|»» breakerType|[EventBreakerType](#schemaeventbreakertype)|true|none|none|
|»» description|string|false|none|none|
|»» extractions|[allOf]|false|none|none|

*allOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» name|string|true|none|none|
|»»»» sourceField|string|true|none|none|
|»»»» type|[ExtractionType](#schemaextractiontype)|true|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» delimiter|string|true|none|none|
|»»»»» escape|string|true|none|none|
|»»»»» fieldList|[string]|false|none|none|
|»»»»» nullValue|string|true|none|none|
|»»»»» quote|string|true|none|none|
|»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» regexpList|[object]|true|none|none|
|»»»»»» overwrite|boolean|false|none|none|
|»»»»»» regexp|string|true|none|none|
|»»»»» type|string|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» schemaMap|[[FieldMappingType](#schemafieldmappingtype)]|false|none|none|
|»» tags|string|false|none|none|
|»» timestampExtraction|any|true|none|none|

*allOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» anchorRegex|string|false|none|none|
|»»»» earliest|string|false|none|none|
|»»»» latest|string|false|none|none|
|»»»» sourceField|string|false|none|none|
|»»»» timezone|string|false|none|none|
|»»»» type|[TimestampExtractionType](#schematimestampextractiontype)|true|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» scanDepth|number|true|none|none|
|»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» format|string|true|none|none|
|»»»»» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|breakerType|parquet|
|breakerType|ndjson|
|breakerType|csv|
|type|csv|
|type|regexp|
|type|csv|
|type|regexp|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|
|type|auto|
|type|manual|
|type|auto|
|type|manual|

<aside class="success">
This operation does not require authentication
</aside>

## Get DataTypeDescriptor by ID

<a id="opIdgetDataTypeDescriptorById"></a>

> Code samples

`GET /search/datatypes/{id}`

Get DataTypeDescriptor by ID

<h3 id="get-datatypedescriptor-by-id-parameters">Parameters</h3>

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
      "addFields": [
        {
          "fieldName": "string",
          "source": "string"
        }
      ],
      "breakerType": "parquet",
      "description": "string",
      "extractions": [
        {
          "name": "string",
          "sourceField": "string",
          "type": "csv",
          "delimiter": "string",
          "escape": "string",
          "fieldList": [
            "string"
          ],
          "nullValue": "string",
          "quote": "string"
        }
      ],
      "id": "string",
      "lib": "cribl",
      "schemaMap": [
        {
          "fieldName": "string",
          "source": "string"
        }
      ],
      "tags": "string",
      "timestampExtraction": {
        "anchorRegex": "string",
        "earliest": "string",
        "latest": "string",
        "sourceField": "string",
        "timezone": "string",
        "type": "auto",
        "scanDepth": 0
      }
    }
  ]
}
```

<h3 id="get-datatypedescriptor-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DataTypeDescriptor objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-datatypedescriptor-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DataTypeDescriptor](#schemadatatypedescriptor)]|false|none|none|
|»» addFields|[[FieldMappingType](#schemafieldmappingtype)]|false|none|none|
|»»» fieldName|string|true|none|none|
|»»» source|string|true|none|none|
|»» breakerType|[EventBreakerType](#schemaeventbreakertype)|true|none|none|
|»» description|string|false|none|none|
|»» extractions|[allOf]|false|none|none|

*allOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» name|string|true|none|none|
|»»»» sourceField|string|true|none|none|
|»»»» type|[ExtractionType](#schemaextractiontype)|true|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» delimiter|string|true|none|none|
|»»»»» escape|string|true|none|none|
|»»»»» fieldList|[string]|false|none|none|
|»»»»» nullValue|string|true|none|none|
|»»»»» quote|string|true|none|none|
|»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» regexpList|[object]|true|none|none|
|»»»»»» overwrite|boolean|false|none|none|
|»»»»»» regexp|string|true|none|none|
|»»»»» type|string|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» schemaMap|[[FieldMappingType](#schemafieldmappingtype)]|false|none|none|
|»» tags|string|false|none|none|
|»» timestampExtraction|any|true|none|none|

*allOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» anchorRegex|string|false|none|none|
|»»»» earliest|string|false|none|none|
|»»»» latest|string|false|none|none|
|»»»» sourceField|string|false|none|none|
|»»»» timezone|string|false|none|none|
|»»»» type|[TimestampExtractionType](#schematimestampextractiontype)|true|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» scanDepth|number|true|none|none|
|»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» format|string|true|none|none|
|»»»»» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|breakerType|parquet|
|breakerType|ndjson|
|breakerType|csv|
|type|csv|
|type|regexp|
|type|csv|
|type|regexp|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|
|type|auto|
|type|manual|
|type|auto|
|type|manual|

<aside class="success">
This operation does not require authentication
</aside>

## Update DataTypeDescriptor

<a id="opIdupdateDataTypeDescriptorById"></a>

> Code samples

`PATCH /search/datatypes/{id}`

Update DataTypeDescriptor

> Body parameter

```json
{
  "addFields": [
    {
      "fieldName": "string",
      "source": "string"
    }
  ],
  "breakerType": "parquet",
  "description": "string",
  "extractions": [
    {
      "name": "string",
      "sourceField": "string",
      "type": "csv",
      "delimiter": "string",
      "escape": "string",
      "fieldList": [
        "string"
      ],
      "nullValue": "string",
      "quote": "string"
    }
  ],
  "id": "string",
  "lib": "cribl",
  "schemaMap": [
    {
      "fieldName": "string",
      "source": "string"
    }
  ],
  "tags": "string",
  "timestampExtraction": {
    "anchorRegex": "string",
    "earliest": "string",
    "latest": "string",
    "sourceField": "string",
    "timezone": "string",
    "type": "auto",
    "scanDepth": 0
  }
}
```

<h3 id="update-datatypedescriptor-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[DataTypeDescriptor](#schemadatatypedescriptor)|true|DataTypeDescriptor object to be updated|
|» addFields|body|[[FieldMappingType](#schemafieldmappingtype)]|false|none|
|»» fieldName|body|string|true|none|
|»» source|body|string|true|none|
|» breakerType|body|[EventBreakerType](#schemaeventbreakertype)|true|none|
|» description|body|string|false|none|
|» extractions|body|[allOf]|false|none|
|»» *anonymous*|body|object|false|none|
|»»» name|body|string|true|none|
|»»» sourceField|body|string|true|none|
|»»» type|body|[ExtractionType](#schemaextractiontype)|true|none|
|»» *anonymous*|body|any|false|none|
|»»» *anonymous*|body|object|false|none|
|»»»» delimiter|body|string|true|none|
|»»»» escape|body|string|true|none|
|»»»» fieldList|body|[string]|false|none|
|»»»» nullValue|body|string|true|none|
|»»»» quote|body|string|true|none|
|»»»» type|body|string|true|none|
|»»» *anonymous*|body|object|false|none|
|»»»» regexpList|body|[object]|true|none|
|»»»»» overwrite|body|boolean|false|none|
|»»»»» regexp|body|string|true|none|
|»»»» type|body|string|true|none|
|» id|body|string|true|none|
|» lib|body|[CriblLib](#schemacribllib)|true|none|
|» schemaMap|body|[[FieldMappingType](#schemafieldmappingtype)]|false|none|
|» tags|body|string|false|none|
|» timestampExtraction|body|any|true|none|
|»» *anonymous*|body|object|false|none|
|»»» anchorRegex|body|string|false|none|
|»»» earliest|body|string|false|none|
|»»» latest|body|string|false|none|
|»»» sourceField|body|string|false|none|
|»»» timezone|body|string|false|none|
|»»» type|body|[TimestampExtractionType](#schematimestampextractiontype)|true|none|
|»» *anonymous*|body|any|false|none|
|»»» *anonymous*|body|object|false|none|
|»»»» scanDepth|body|number|true|none|
|»»»» type|body|string|true|none|
|»»» *anonymous*|body|object|false|none|
|»»»» format|body|string|true|none|
|»»»» type|body|string|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» breakerType|parquet|
|» breakerType|ndjson|
|» breakerType|csv|
|»»» type|csv|
|»»» type|regexp|
|»»»» type|csv|
|»»»» type|regexp|
|» lib|cribl|
|» lib|cribl-custom|
|» lib|custom|
|»»» type|auto|
|»»» type|manual|
|»»»» type|auto|
|»»»» type|manual|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "addFields": [
        {
          "fieldName": "string",
          "source": "string"
        }
      ],
      "breakerType": "parquet",
      "description": "string",
      "extractions": [
        {
          "name": "string",
          "sourceField": "string",
          "type": "csv",
          "delimiter": "string",
          "escape": "string",
          "fieldList": [
            "string"
          ],
          "nullValue": "string",
          "quote": "string"
        }
      ],
      "id": "string",
      "lib": "cribl",
      "schemaMap": [
        {
          "fieldName": "string",
          "source": "string"
        }
      ],
      "tags": "string",
      "timestampExtraction": {
        "anchorRegex": "string",
        "earliest": "string",
        "latest": "string",
        "sourceField": "string",
        "timezone": "string",
        "type": "auto",
        "scanDepth": 0
      }
    }
  ]
}
```

<h3 id="update-datatypedescriptor-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DataTypeDescriptor objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-datatypedescriptor-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DataTypeDescriptor](#schemadatatypedescriptor)]|false|none|none|
|»» addFields|[[FieldMappingType](#schemafieldmappingtype)]|false|none|none|
|»»» fieldName|string|true|none|none|
|»»» source|string|true|none|none|
|»» breakerType|[EventBreakerType](#schemaeventbreakertype)|true|none|none|
|»» description|string|false|none|none|
|»» extractions|[allOf]|false|none|none|

*allOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» name|string|true|none|none|
|»»»» sourceField|string|true|none|none|
|»»»» type|[ExtractionType](#schemaextractiontype)|true|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» delimiter|string|true|none|none|
|»»»»» escape|string|true|none|none|
|»»»»» fieldList|[string]|false|none|none|
|»»»»» nullValue|string|true|none|none|
|»»»»» quote|string|true|none|none|
|»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» regexpList|[object]|true|none|none|
|»»»»»» overwrite|boolean|false|none|none|
|»»»»»» regexp|string|true|none|none|
|»»»»» type|string|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» schemaMap|[[FieldMappingType](#schemafieldmappingtype)]|false|none|none|
|»» tags|string|false|none|none|
|»» timestampExtraction|any|true|none|none|

*allOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» anchorRegex|string|false|none|none|
|»»»» earliest|string|false|none|none|
|»»»» latest|string|false|none|none|
|»»»» sourceField|string|false|none|none|
|»»»» timezone|string|false|none|none|
|»»»» type|[TimestampExtractionType](#schematimestampextractiontype)|true|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» scanDepth|number|true|none|none|
|»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» format|string|true|none|none|
|»»»»» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|breakerType|parquet|
|breakerType|ndjson|
|breakerType|csv|
|type|csv|
|type|regexp|
|type|csv|
|type|regexp|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|
|type|auto|
|type|manual|
|type|auto|
|type|manual|

<aside class="success">
This operation does not require authentication
</aside>

## Delete DataTypeDescriptor

<a id="opIddeleteDataTypeDescriptorById"></a>

> Code samples

`DELETE /search/datatypes/{id}`

Delete DataTypeDescriptor

<h3 id="delete-datatypedescriptor-parameters">Parameters</h3>

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
      "addFields": [
        {
          "fieldName": "string",
          "source": "string"
        }
      ],
      "breakerType": "parquet",
      "description": "string",
      "extractions": [
        {
          "name": "string",
          "sourceField": "string",
          "type": "csv",
          "delimiter": "string",
          "escape": "string",
          "fieldList": [
            "string"
          ],
          "nullValue": "string",
          "quote": "string"
        }
      ],
      "id": "string",
      "lib": "cribl",
      "schemaMap": [
        {
          "fieldName": "string",
          "source": "string"
        }
      ],
      "tags": "string",
      "timestampExtraction": {
        "anchorRegex": "string",
        "earliest": "string",
        "latest": "string",
        "sourceField": "string",
        "timezone": "string",
        "type": "auto",
        "scanDepth": 0
      }
    }
  ]
}
```

<h3 id="delete-datatypedescriptor-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DataTypeDescriptor objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-datatypedescriptor-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DataTypeDescriptor](#schemadatatypedescriptor)]|false|none|none|
|»» addFields|[[FieldMappingType](#schemafieldmappingtype)]|false|none|none|
|»»» fieldName|string|true|none|none|
|»»» source|string|true|none|none|
|»» breakerType|[EventBreakerType](#schemaeventbreakertype)|true|none|none|
|»» description|string|false|none|none|
|»» extractions|[allOf]|false|none|none|

*allOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» name|string|true|none|none|
|»»»» sourceField|string|true|none|none|
|»»»» type|[ExtractionType](#schemaextractiontype)|true|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» delimiter|string|true|none|none|
|»»»»» escape|string|true|none|none|
|»»»»» fieldList|[string]|false|none|none|
|»»»»» nullValue|string|true|none|none|
|»»»»» quote|string|true|none|none|
|»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» regexpList|[object]|true|none|none|
|»»»»»» overwrite|boolean|false|none|none|
|»»»»»» regexp|string|true|none|none|
|»»»»» type|string|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» schemaMap|[[FieldMappingType](#schemafieldmappingtype)]|false|none|none|
|»» tags|string|false|none|none|
|»» timestampExtraction|any|true|none|none|

*allOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» anchorRegex|string|false|none|none|
|»»»» earliest|string|false|none|none|
|»»»» latest|string|false|none|none|
|»»»» sourceField|string|false|none|none|
|»»»» timezone|string|false|none|none|
|»»»» type|[TimestampExtractionType](#schematimestampextractiontype)|true|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» scanDepth|number|true|none|none|
|»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» format|string|true|none|none|
|»»»»» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|breakerType|parquet|
|breakerType|ndjson|
|breakerType|csv|
|type|csv|
|type|regexp|
|type|csv|
|type|regexp|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|
|type|auto|
|type|manual|
|type|auto|
|type|manual|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_DataTypeDescriptor">DataTypeDescriptor</h2>
<!-- backwards compatibility -->
<a id="schemadatatypedescriptor"></a>
<a id="schema_DataTypeDescriptor"></a>
<a id="tocSdatatypedescriptor"></a>
<a id="tocsdatatypedescriptor"></a>

```json
{
  "addFields": [
    {
      "fieldName": "string",
      "source": "string"
    }
  ],
  "breakerType": "parquet",
  "description": "string",
  "extractions": [
    {
      "name": "string",
      "sourceField": "string",
      "type": "csv",
      "delimiter": "string",
      "escape": "string",
      "fieldList": [
        "string"
      ],
      "nullValue": "string",
      "quote": "string"
    }
  ],
  "id": "string",
  "lib": "cribl",
  "schemaMap": [
    {
      "fieldName": "string",
      "source": "string"
    }
  ],
  "tags": "string",
  "timestampExtraction": {
    "anchorRegex": "string",
    "earliest": "string",
    "latest": "string",
    "sourceField": "string",
    "timezone": "string",
    "type": "auto",
    "scanDepth": 0
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|addFields|[[FieldMappingType](#schemafieldmappingtype)]|false|none|none|
|breakerType|[EventBreakerType](#schemaeventbreakertype)|true|none|none|
|description|string|false|none|none|
|extractions|[[DataTypeExtraction](#schemadatatypeextraction)]|false|none|none|
|id|string|true|none|none|
|lib|[CriblLib](#schemacribllib)|true|none|none|
|schemaMap|[[FieldMappingType](#schemafieldmappingtype)]|false|none|none|
|tags|string|false|none|none|
|timestampExtraction|[TimestampExtraction](#schematimestampextraction)|true|none|none|

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

<h2 id="tocS_FieldMappingType">FieldMappingType</h2>
<!-- backwards compatibility -->
<a id="schemafieldmappingtype"></a>
<a id="schema_FieldMappingType"></a>
<a id="tocSfieldmappingtype"></a>
<a id="tocsfieldmappingtype"></a>

```json
{
  "fieldName": "string",
  "source": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|fieldName|string|true|none|none|
|source|string|true|none|none|

<h2 id="tocS_EventBreakerType">EventBreakerType</h2>
<!-- backwards compatibility -->
<a id="schemaeventbreakertype"></a>
<a id="schema_EventBreakerType"></a>
<a id="tocSeventbreakertype"></a>
<a id="tocseventbreakertype"></a>

```json
"parquet"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|parquet|
|*anonymous*|ndjson|
|*anonymous*|csv|

<h2 id="tocS_DataTypeExtraction">DataTypeExtraction</h2>
<!-- backwards compatibility -->
<a id="schemadatatypeextraction"></a>
<a id="schema_DataTypeExtraction"></a>
<a id="tocSdatatypeextraction"></a>
<a id="tocsdatatypeextraction"></a>

```json
{
  "name": "string",
  "sourceField": "string",
  "type": "csv",
  "delimiter": "string",
  "escape": "string",
  "fieldList": [
    "string"
  ],
  "nullValue": "string",
  "quote": "string"
}

```

### Properties

allOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» name|string|true|none|none|
|» sourceField|string|true|none|none|
|» type|[ExtractionType](#schemaextractiontype)|true|none|none|

and

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» delimiter|string|true|none|none|
|»» escape|string|true|none|none|
|»» fieldList|[string]|false|none|none|
|»» nullValue|string|true|none|none|
|»» quote|string|true|none|none|
|»» type|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» regexpList|[object]|true|none|none|
|»»» overwrite|boolean|false|none|none|
|»»» regexp|string|true|none|none|
|»» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|csv|
|type|regexp|

<h2 id="tocS_CriblLib">CriblLib</h2>
<!-- backwards compatibility -->
<a id="schemacribllib"></a>
<a id="schema_CriblLib"></a>
<a id="tocScribllib"></a>
<a id="tocscribllib"></a>

```json
"cribl"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|cribl|
|*anonymous*|cribl-custom|
|*anonymous*|custom|

<h2 id="tocS_TimestampExtraction">TimestampExtraction</h2>
<!-- backwards compatibility -->
<a id="schematimestampextraction"></a>
<a id="schema_TimestampExtraction"></a>
<a id="tocStimestampextraction"></a>
<a id="tocstimestampextraction"></a>

```json
{
  "anchorRegex": "string",
  "earliest": "string",
  "latest": "string",
  "sourceField": "string",
  "timezone": "string",
  "type": "auto",
  "scanDepth": 0
}

```

### Properties

allOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» anchorRegex|string|false|none|none|
|» earliest|string|false|none|none|
|» latest|string|false|none|none|
|» sourceField|string|false|none|none|
|» timezone|string|false|none|none|
|» type|[TimestampExtractionType](#schematimestampextractiontype)|true|none|none|

and

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» scanDepth|number|true|none|none|
|»» type|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» format|string|true|none|none|
|»» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|auto|
|type|manual|

<h2 id="tocS_ExtractionType">ExtractionType</h2>
<!-- backwards compatibility -->
<a id="schemaextractiontype"></a>
<a id="schema_ExtractionType"></a>
<a id="tocSextractiontype"></a>
<a id="tocsextractiontype"></a>

```json
"csv"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|csv|
|*anonymous*|regexp|

<h2 id="tocS_TimestampExtractionType">TimestampExtractionType</h2>
<!-- backwards compatibility -->
<a id="schematimestampextractiontype"></a>
<a id="schema_TimestampExtractionType"></a>
<a id="tocStimestampextractiontype"></a>
<a id="tocstimestampextractiontype"></a>

```json
"auto"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|auto|
|*anonymous*|manual|

