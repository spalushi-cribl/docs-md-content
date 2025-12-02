
<h1 id="cribl-search-api-dataset-providers">Cribl Search API - Dataset Providers v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-search-api-dataset-providers-dataset-providers">Dataset Providers</h1>

## Get a list of DatasetProviderType objects

<a id="opIdlistDatasetProviderType"></a>

> Code samples

`GET /search/dataset-provider-types`

Get a list of DatasetProviderType objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "capabilities": [
        "read"
      ],
      "description": "string",
      "id": "prometheus",
      "locality": {
        "filterExpression": "string",
        "origin": "leader_local"
      }
    }
  ]
}
```

<h3 id="get-a-list-of-datasetprovidertype-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatasetProviderType objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-datasetprovidertype-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DatasetProviderType](#schemadatasetprovidertype)]|false|none|none|
|»» capabilities|[[DatasetProviderCapability](#schemadatasetprovidercapability)]|true|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» locality|[OriginConfig](#schemaoriginconfig)|false|none|none|
|»»» filterExpression|string|false|none|none|
|»»» origin|[DatasetOrigin](#schemadatasetorigin)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|id|prometheus|
|id|s3|
|id|cribl_lake|
|id|gcs|
|id|azure_blob|
|id|cribl_leader|
|id|cribl_edge|
|id|amazon_security_lake|
|id|api_http|
|id|api_aws|
|id|api_azure|
|id|api_gcp|
|id|api_google_workspace|
|id|api_msgraph|
|id|api_okta|
|id|api_tailscale|
|id|api_zoom|
|id|api_opensearch|
|id|api_elasticsearch|
|id|api_azure_data_explorer|
|id|snowflake|
|id|clickhouse|
|id|cribl_meta|
|id|cribl_local|
|origin|leader_local|
|origin|remote|
|origin|worker_local|

<aside class="success">
This operation does not require authentication
</aside>

## Create DatasetProviderType

<a id="opIdcreateDatasetProviderType"></a>

> Code samples

`POST /search/dataset-provider-types`

Create DatasetProviderType

> Body parameter

```json
{
  "capabilities": [
    "read"
  ],
  "description": "string",
  "id": "prometheus",
  "locality": {
    "filterExpression": "string",
    "origin": "leader_local"
  }
}
```

<h3 id="create-datasetprovidertype-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[DatasetProviderType](#schemadatasetprovidertype)|true|New DatasetProviderType object|
|» capabilities|body|[[DatasetProviderCapability](#schemadatasetprovidercapability)]|true|none|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» locality|body|[OriginConfig](#schemaoriginconfig)|false|none|
|»» filterExpression|body|string|false|none|
|»» origin|body|[DatasetOrigin](#schemadatasetorigin)|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» capabilities|read|
|» capabilities|list|
|» id|prometheus|
|» id|s3|
|» id|cribl_lake|
|» id|gcs|
|» id|azure_blob|
|» id|cribl_leader|
|» id|cribl_edge|
|» id|amazon_security_lake|
|» id|api_http|
|» id|api_aws|
|» id|api_azure|
|» id|api_gcp|
|» id|api_google_workspace|
|» id|api_msgraph|
|» id|api_okta|
|» id|api_tailscale|
|» id|api_zoom|
|» id|api_opensearch|
|» id|api_elasticsearch|
|» id|api_azure_data_explorer|
|» id|snowflake|
|» id|clickhouse|
|» id|cribl_meta|
|» id|cribl_local|
|»» origin|leader_local|
|»» origin|remote|
|»» origin|worker_local|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "capabilities": [
        "read"
      ],
      "description": "string",
      "id": "prometheus",
      "locality": {
        "filterExpression": "string",
        "origin": "leader_local"
      }
    }
  ]
}
```

<h3 id="create-datasetprovidertype-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatasetProviderType objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-datasetprovidertype-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DatasetProviderType](#schemadatasetprovidertype)]|false|none|none|
|»» capabilities|[[DatasetProviderCapability](#schemadatasetprovidercapability)]|true|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» locality|[OriginConfig](#schemaoriginconfig)|false|none|none|
|»»» filterExpression|string|false|none|none|
|»»» origin|[DatasetOrigin](#schemadatasetorigin)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|id|prometheus|
|id|s3|
|id|cribl_lake|
|id|gcs|
|id|azure_blob|
|id|cribl_leader|
|id|cribl_edge|
|id|amazon_security_lake|
|id|api_http|
|id|api_aws|
|id|api_azure|
|id|api_gcp|
|id|api_google_workspace|
|id|api_msgraph|
|id|api_okta|
|id|api_tailscale|
|id|api_zoom|
|id|api_opensearch|
|id|api_elasticsearch|
|id|api_azure_data_explorer|
|id|snowflake|
|id|clickhouse|
|id|cribl_meta|
|id|cribl_local|
|origin|leader_local|
|origin|remote|
|origin|worker_local|

<aside class="success">
This operation does not require authentication
</aside>

## Delete DatasetProviderType

<a id="opIddeleteDatasetProviderTypeById"></a>

> Code samples

`DELETE /search/dataset-provider-types/{id}`

Delete DatasetProviderType

<h3 id="delete-datasetprovidertype-parameters">Parameters</h3>

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
      "capabilities": [
        "read"
      ],
      "description": "string",
      "id": "prometheus",
      "locality": {
        "filterExpression": "string",
        "origin": "leader_local"
      }
    }
  ]
}
```

<h3 id="delete-datasetprovidertype-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatasetProviderType objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-datasetprovidertype-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DatasetProviderType](#schemadatasetprovidertype)]|false|none|none|
|»» capabilities|[[DatasetProviderCapability](#schemadatasetprovidercapability)]|true|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» locality|[OriginConfig](#schemaoriginconfig)|false|none|none|
|»»» filterExpression|string|false|none|none|
|»»» origin|[DatasetOrigin](#schemadatasetorigin)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|id|prometheus|
|id|s3|
|id|cribl_lake|
|id|gcs|
|id|azure_blob|
|id|cribl_leader|
|id|cribl_edge|
|id|amazon_security_lake|
|id|api_http|
|id|api_aws|
|id|api_azure|
|id|api_gcp|
|id|api_google_workspace|
|id|api_msgraph|
|id|api_okta|
|id|api_tailscale|
|id|api_zoom|
|id|api_opensearch|
|id|api_elasticsearch|
|id|api_azure_data_explorer|
|id|snowflake|
|id|clickhouse|
|id|cribl_meta|
|id|cribl_local|
|origin|leader_local|
|origin|remote|
|origin|worker_local|

<aside class="success">
This operation does not require authentication
</aside>

## Get DatasetProviderType by ID

<a id="opIdgetDatasetProviderTypeById"></a>

> Code samples

`GET /search/dataset-provider-types/{id}`

Get DatasetProviderType by ID

<h3 id="get-datasetprovidertype-by-id-parameters">Parameters</h3>

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
      "capabilities": [
        "read"
      ],
      "description": "string",
      "id": "prometheus",
      "locality": {
        "filterExpression": "string",
        "origin": "leader_local"
      }
    }
  ]
}
```

<h3 id="get-datasetprovidertype-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatasetProviderType objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-datasetprovidertype-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DatasetProviderType](#schemadatasetprovidertype)]|false|none|none|
|»» capabilities|[[DatasetProviderCapability](#schemadatasetprovidercapability)]|true|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» locality|[OriginConfig](#schemaoriginconfig)|false|none|none|
|»»» filterExpression|string|false|none|none|
|»»» origin|[DatasetOrigin](#schemadatasetorigin)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|id|prometheus|
|id|s3|
|id|cribl_lake|
|id|gcs|
|id|azure_blob|
|id|cribl_leader|
|id|cribl_edge|
|id|amazon_security_lake|
|id|api_http|
|id|api_aws|
|id|api_azure|
|id|api_gcp|
|id|api_google_workspace|
|id|api_msgraph|
|id|api_okta|
|id|api_tailscale|
|id|api_zoom|
|id|api_opensearch|
|id|api_elasticsearch|
|id|api_azure_data_explorer|
|id|snowflake|
|id|clickhouse|
|id|cribl_meta|
|id|cribl_local|
|origin|leader_local|
|origin|remote|
|origin|worker_local|

<aside class="success">
This operation does not require authentication
</aside>

## Update DatasetProviderType

<a id="opIdupdateDatasetProviderTypeById"></a>

> Code samples

`PATCH /search/dataset-provider-types/{id}`

Update DatasetProviderType

> Body parameter

```json
{
  "capabilities": [
    "read"
  ],
  "description": "string",
  "id": "prometheus",
  "locality": {
    "filterExpression": "string",
    "origin": "leader_local"
  }
}
```

<h3 id="update-datasetprovidertype-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[DatasetProviderType](#schemadatasetprovidertype)|true|DatasetProviderType object to be updated|
|» capabilities|body|[[DatasetProviderCapability](#schemadatasetprovidercapability)]|true|none|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» locality|body|[OriginConfig](#schemaoriginconfig)|false|none|
|»» filterExpression|body|string|false|none|
|»» origin|body|[DatasetOrigin](#schemadatasetorigin)|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» capabilities|read|
|» capabilities|list|
|» id|prometheus|
|» id|s3|
|» id|cribl_lake|
|» id|gcs|
|» id|azure_blob|
|» id|cribl_leader|
|» id|cribl_edge|
|» id|amazon_security_lake|
|» id|api_http|
|» id|api_aws|
|» id|api_azure|
|» id|api_gcp|
|» id|api_google_workspace|
|» id|api_msgraph|
|» id|api_okta|
|» id|api_tailscale|
|» id|api_zoom|
|» id|api_opensearch|
|» id|api_elasticsearch|
|» id|api_azure_data_explorer|
|» id|snowflake|
|» id|clickhouse|
|» id|cribl_meta|
|» id|cribl_local|
|»» origin|leader_local|
|»» origin|remote|
|»» origin|worker_local|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "capabilities": [
        "read"
      ],
      "description": "string",
      "id": "prometheus",
      "locality": {
        "filterExpression": "string",
        "origin": "leader_local"
      }
    }
  ]
}
```

<h3 id="update-datasetprovidertype-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatasetProviderType objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-datasetprovidertype-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DatasetProviderType](#schemadatasetprovidertype)]|false|none|none|
|»» capabilities|[[DatasetProviderCapability](#schemadatasetprovidercapability)]|true|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» locality|[OriginConfig](#schemaoriginconfig)|false|none|none|
|»»» filterExpression|string|false|none|none|
|»»» origin|[DatasetOrigin](#schemadatasetorigin)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|id|prometheus|
|id|s3|
|id|cribl_lake|
|id|gcs|
|id|azure_blob|
|id|cribl_leader|
|id|cribl_edge|
|id|amazon_security_lake|
|id|api_http|
|id|api_aws|
|id|api_azure|
|id|api_gcp|
|id|api_google_workspace|
|id|api_msgraph|
|id|api_okta|
|id|api_tailscale|
|id|api_zoom|
|id|api_opensearch|
|id|api_elasticsearch|
|id|api_azure_data_explorer|
|id|snowflake|
|id|clickhouse|
|id|cribl_meta|
|id|cribl_local|
|origin|leader_local|
|origin|remote|
|origin|worker_local|

<aside class="success">
This operation does not require authentication
</aside>

## Create DatasetProvider

<a id="opIdcreateDatasetProvider"></a>

> Code samples

`POST /search/dataset-providers`

Create DatasetProvider

> Body parameter

```json
{}
```

<h3 id="create-datasetprovider-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[UnionOfValues](#schemaunionofvalues)|true|New DatasetProvider object|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="create-datasetprovider-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatasetProvider objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-datasetprovider-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UnionOfValues](#schemaunionofvalues)]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get a list of DatasetProvider objects

<a id="opIdlistDatasetProvider"></a>

> Code samples

`GET /search/dataset-providers`

Get a list of DatasetProvider objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="get-a-list-of-datasetprovider-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatasetProvider objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-datasetprovider-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UnionOfValues](#schemaunionofvalues)]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete DatasetProvider

<a id="opIddeleteDatasetProviderById"></a>

> Code samples

`DELETE /search/dataset-providers/{id}`

Delete DatasetProvider

<h3 id="delete-datasetprovider-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to DELETE|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="delete-datasetprovider-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatasetProvider objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-datasetprovider-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UnionOfValues](#schemaunionofvalues)]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update DatasetProvider

<a id="opIdupdateDatasetProviderById"></a>

> Code samples

`PATCH /search/dataset-providers/{id}`

Update DatasetProvider

> Body parameter

```json
{}
```

<h3 id="update-datasetprovider-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[UnionOfValues](#schemaunionofvalues)|true|DatasetProvider object to be updated|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="update-datasetprovider-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatasetProvider objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-datasetprovider-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UnionOfValues](#schemaunionofvalues)]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get DatasetProvider by ID

<a id="opIdgetDatasetProviderById"></a>

> Code samples

`GET /search/dataset-providers/{id}`

Get DatasetProvider by ID

<h3 id="get-datasetprovider-by-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to GET|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="get-datasetprovider-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatasetProvider objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-datasetprovider-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UnionOfValues](#schemaunionofvalues)]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_DatasetProviderType">DatasetProviderType</h2>
<!-- backwards compatibility -->
<a id="schemadatasetprovidertype"></a>
<a id="schema_DatasetProviderType"></a>
<a id="tocSdatasetprovidertype"></a>
<a id="tocsdatasetprovidertype"></a>

```json
{
  "capabilities": [
    "read"
  ],
  "description": "string",
  "id": "prometheus",
  "locality": {
    "filterExpression": "string",
    "origin": "leader_local"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|capabilities|[[DatasetProviderCapability](#schemadatasetprovidercapability)]|true|none|none|
|description|string|false|none|none|
|id|string|true|none|none|
|locality|[OriginConfig](#schemaoriginconfig)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|id|prometheus|
|id|s3|
|id|cribl_lake|
|id|gcs|
|id|azure_blob|
|id|cribl_leader|
|id|cribl_edge|
|id|amazon_security_lake|
|id|api_http|
|id|api_aws|
|id|api_azure|
|id|api_gcp|
|id|api_google_workspace|
|id|api_msgraph|
|id|api_okta|
|id|api_tailscale|
|id|api_zoom|
|id|api_opensearch|
|id|api_elasticsearch|
|id|api_azure_data_explorer|
|id|snowflake|
|id|clickhouse|
|id|cribl_meta|
|id|cribl_local|

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

<h2 id="tocS_DatasetProvider">DatasetProvider</h2>
<!-- backwards compatibility -->
<a id="schemadatasetprovider"></a>
<a id="schema_DatasetProvider"></a>
<a id="tocSdatasetprovider"></a>
<a id="tocsdatasetprovider"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocS_DatasetProviderCapability">DatasetProviderCapability</h2>
<!-- backwards compatibility -->
<a id="schemadatasetprovidercapability"></a>
<a id="schema_DatasetProviderCapability"></a>
<a id="tocSdatasetprovidercapability"></a>
<a id="tocsdatasetprovidercapability"></a>

```json
"read"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|read|
|*anonymous*|list|

<h2 id="tocS_OriginConfig">OriginConfig</h2>
<!-- backwards compatibility -->
<a id="schemaoriginconfig"></a>
<a id="schema_OriginConfig"></a>
<a id="tocSoriginconfig"></a>
<a id="tocsoriginconfig"></a>

```json
{
  "filterExpression": "string",
  "origin": "leader_local"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|filterExpression|string|false|none|none|
|origin|[DatasetOrigin](#schemadatasetorigin)|true|none|none|

<h2 id="tocS_UnionOfValues">UnionOfValues</h2>
<!-- backwards compatibility -->
<a id="schemaunionofvalues"></a>
<a id="schema_UnionOfValues"></a>
<a id="tocSunionofvalues"></a>
<a id="tocsunionofvalues"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocS_DatasetOrigin">DatasetOrigin</h2>
<!-- backwards compatibility -->
<a id="schemadatasetorigin"></a>
<a id="schema_DatasetOrigin"></a>
<a id="tocSdatasetorigin"></a>
<a id="tocsdatasetorigin"></a>

```json
"leader_local"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|leader_local|
|*anonymous*|remote|
|*anonymous*|worker_local|

