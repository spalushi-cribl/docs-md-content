
<h1 id="cribl-lake-api-datasets">Cribl Lake API - Datasets v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-lake-api-datasets-datasets">Datasets</h1>

## List all Lake Datasets

<a id="opIdgetCriblLakeDatasetByLakeId"></a>

> Code samples

`GET /products/lake/lakes/{lakeId}/datasets`

Get a list of all Lake Datasets in the specified Lake.

<h3 id="list-all-lake-datasets-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|lakeId|path|string|true|The <code>id</code> of the Lake that contains the Lake Datasets to list.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "acceleratedFields": [
        "string"
      ],
      "bucketName": "string",
      "cacheConnection": {
        "acceleratedFields": [
          "string"
        ],
        "backfillStatus": "scheduled",
        "cacheRef": "string",
        "createdAt": 0,
        "lakehouseConnectionType": "cache",
        "migrationQueryId": "string",
        "retentionInDays": 0
      },
      "deletionStartedAt": 0,
      "description": "string",
      "format": "json",
      "httpDAUsed": true,
      "id": "string",
      "metrics": {
        "currentSizeBytes": 0,
        "metricsDate": "string"
      },
      "retentionPeriodInDays": 0,
      "searchConfig": {
        "datatypes": [
          "string"
        ],
        "metadata": {
          "earliest": "string",
          "enableAcceleration": true,
          "fieldList": [
            "string"
          ],
          "latestRunInfo": {
            "earliestScannedTime": 0,
            "finishedAt": 0,
            "latestScannedTime": 0,
            "objectCount": 0
          },
          "scanMode": "detailed"
        }
      },
      "storageLocationId": "string",
      "viewName": "string"
    }
  ]
}
```

<h3 id="list-all-lake-datasets-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CriblLakeDataset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="list-all-lake-datasets-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CriblLakeDataset](#schemacribllakedataset)]|false|none|none|
|»» acceleratedFields|[string]|false|none|none|
|»» bucketName|string|false|none|none|
|»» cacheConnection|[CacheConnection](#schemacacheconnection)|false|none|none|
|»»» acceleratedFields|[string]|false|none|none|
|»»» backfillStatus|[CacheConnectionBackfillStatus](#schemacacheconnectionbackfillstatus)|false|none|none|
|»»» cacheRef|string|true|none|none|
|»»» createdAt|number|true|none|none|
|»»» lakehouseConnectionType|[LakehouseConnectionType](#schemalakehouseconnectiontype)|false|none|none|
|»»» migrationQueryId|string|false|none|none|
|»»» retentionInDays|number|true|none|none|
|»» deletionStartedAt|number|false|none|none|
|»» description|string|false|none|none|
|»» format|string|false|none|none|
|»» httpDAUsed|boolean|false|none|none|
|»» id|string|true|none|none|
|»» metrics|[LakeDatasetMetrics](#schemalakedatasetmetrics)|false|none|none|
|»»» currentSizeBytes|number|true|none|none|
|»»» metricsDate|string|true|none|none|
|»» retentionPeriodInDays|number|false|none|none|
|»» searchConfig|[LakeDatasetSearchConfig](#schemalakedatasetsearchconfig)|false|none|none|
|»»» datatypes|[string]|false|none|none|
|»»» metadata|[DatasetMetadata](#schemadatasetmetadata)|false|none|none|
|»»»» earliest|string|true|none|none|
|»»»» enableAcceleration|boolean|true|none|none|
|»»»» fieldList|[string]|true|none|none|
|»»»» latestRunInfo|[DatasetMetadataRunInfo](#schemadatasetmetadataruninfo)|false|none|none|
|»»»»» earliestScannedTime|number|false|none|none|
|»»»»» finishedAt|number|false|none|none|
|»»»»» latestScannedTime|number|false|none|none|
|»»»»» objectCount|number|false|none|none|
|»»»» scanMode|string|true|none|none|
|»» storageLocationId|string|false|none|none|
|»» viewName|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|backfillStatus|scheduled|
|backfillStatus|pending|
|backfillStatus|started|
|backfillStatus|finished|
|backfillStatus|incomplete|
|lakehouseConnectionType|cache|
|lakehouseConnectionType|zeroPoint|
|format|json|
|format|ddss|
|format|parquet|
|scanMode|detailed|
|scanMode|quick|

<aside class="success">
This operation does not require authentication
</aside>

## Create a Lake Dataset

<a id="opIdcreateCriblLakeDatasetByLakeId"></a>

> Code samples

`POST /products/lake/lakes/{lakeId}/datasets`

Create a new Lake Dataset in the specified Lake.

> Body parameter

```json
{
  "acceleratedFields": [
    "string"
  ],
  "bucketName": "string",
  "cacheConnection": {
    "acceleratedFields": [
      "string"
    ],
    "backfillStatus": "scheduled",
    "cacheRef": "string",
    "createdAt": 0,
    "lakehouseConnectionType": "cache",
    "migrationQueryId": "string",
    "retentionInDays": 0
  },
  "deletionStartedAt": 0,
  "description": "string",
  "format": "json",
  "httpDAUsed": true,
  "id": "string",
  "metrics": {
    "currentSizeBytes": 0,
    "metricsDate": "string"
  },
  "retentionPeriodInDays": 0,
  "searchConfig": {
    "datatypes": [
      "string"
    ],
    "metadata": {
      "earliest": "string",
      "enableAcceleration": true,
      "fieldList": [
        "string"
      ],
      "latestRunInfo": {
        "earliestScannedTime": 0,
        "finishedAt": 0,
        "latestScannedTime": 0,
        "objectCount": 0
      },
      "scanMode": "detailed"
    }
  },
  "storageLocationId": "string",
  "viewName": "string"
}
```

<h3 id="create-a-lake-dataset-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|lakeId|path|string|true|The <code>id</code> of the Lake to create the Lake Dataset in.|
|body|body|[CriblLakeDataset](#schemacribllakedataset)|true|CriblLakeDataset object|
|» acceleratedFields|body|[string]|false|none|
|» bucketName|body|string|false|none|
|» cacheConnection|body|[CacheConnection](#schemacacheconnection)|false|none|
|»» acceleratedFields|body|[string]|false|none|
|»» backfillStatus|body|[CacheConnectionBackfillStatus](#schemacacheconnectionbackfillstatus)|false|none|
|»» cacheRef|body|string|true|none|
|»» createdAt|body|number|true|none|
|»» lakehouseConnectionType|body|[LakehouseConnectionType](#schemalakehouseconnectiontype)|false|none|
|»» migrationQueryId|body|string|false|none|
|»» retentionInDays|body|number|true|none|
|» deletionStartedAt|body|number|false|none|
|» description|body|string|false|none|
|» format|body|string|false|none|
|» httpDAUsed|body|boolean|false|none|
|» id|body|string|true|none|
|» metrics|body|[LakeDatasetMetrics](#schemalakedatasetmetrics)|false|none|
|»» currentSizeBytes|body|number|true|none|
|»» metricsDate|body|string|true|none|
|» retentionPeriodInDays|body|number|false|none|
|» searchConfig|body|[LakeDatasetSearchConfig](#schemalakedatasetsearchconfig)|false|none|
|»» datatypes|body|[string]|false|none|
|»» metadata|body|[DatasetMetadata](#schemadatasetmetadata)|false|none|
|»»» earliest|body|string|true|none|
|»»» enableAcceleration|body|boolean|true|none|
|»»» fieldList|body|[string]|true|none|
|»»» latestRunInfo|body|[DatasetMetadataRunInfo](#schemadatasetmetadataruninfo)|false|none|
|»»»» earliestScannedTime|body|number|false|none|
|»»»» finishedAt|body|number|false|none|
|»»»» latestScannedTime|body|number|false|none|
|»»»» objectCount|body|number|false|none|
|»»» scanMode|body|string|true|none|
|» storageLocationId|body|string|false|none|
|» viewName|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»» backfillStatus|scheduled|
|»» backfillStatus|pending|
|»» backfillStatus|started|
|»» backfillStatus|finished|
|»» backfillStatus|incomplete|
|»» lakehouseConnectionType|cache|
|»» lakehouseConnectionType|zeroPoint|
|» format|json|
|» format|ddss|
|» format|parquet|
|»»» scanMode|detailed|
|»»» scanMode|quick|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "acceleratedFields": [
        "string"
      ],
      "bucketName": "string",
      "cacheConnection": {
        "acceleratedFields": [
          "string"
        ],
        "backfillStatus": "scheduled",
        "cacheRef": "string",
        "createdAt": 0,
        "lakehouseConnectionType": "cache",
        "migrationQueryId": "string",
        "retentionInDays": 0
      },
      "deletionStartedAt": 0,
      "description": "string",
      "format": "json",
      "httpDAUsed": true,
      "id": "string",
      "metrics": {
        "currentSizeBytes": 0,
        "metricsDate": "string"
      },
      "retentionPeriodInDays": 0,
      "searchConfig": {
        "datatypes": [
          "string"
        ],
        "metadata": {
          "earliest": "string",
          "enableAcceleration": true,
          "fieldList": [
            "string"
          ],
          "latestRunInfo": {
            "earliestScannedTime": 0,
            "finishedAt": 0,
            "latestScannedTime": 0,
            "objectCount": 0
          },
          "scanMode": "detailed"
        }
      },
      "storageLocationId": "string",
      "viewName": "string"
    }
  ]
}
```

<h3 id="create-a-lake-dataset-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CriblLakeDataset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-a-lake-dataset-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CriblLakeDataset](#schemacribllakedataset)]|false|none|none|
|»» acceleratedFields|[string]|false|none|none|
|»» bucketName|string|false|none|none|
|»» cacheConnection|[CacheConnection](#schemacacheconnection)|false|none|none|
|»»» acceleratedFields|[string]|false|none|none|
|»»» backfillStatus|[CacheConnectionBackfillStatus](#schemacacheconnectionbackfillstatus)|false|none|none|
|»»» cacheRef|string|true|none|none|
|»»» createdAt|number|true|none|none|
|»»» lakehouseConnectionType|[LakehouseConnectionType](#schemalakehouseconnectiontype)|false|none|none|
|»»» migrationQueryId|string|false|none|none|
|»»» retentionInDays|number|true|none|none|
|»» deletionStartedAt|number|false|none|none|
|»» description|string|false|none|none|
|»» format|string|false|none|none|
|»» httpDAUsed|boolean|false|none|none|
|»» id|string|true|none|none|
|»» metrics|[LakeDatasetMetrics](#schemalakedatasetmetrics)|false|none|none|
|»»» currentSizeBytes|number|true|none|none|
|»»» metricsDate|string|true|none|none|
|»» retentionPeriodInDays|number|false|none|none|
|»» searchConfig|[LakeDatasetSearchConfig](#schemalakedatasetsearchconfig)|false|none|none|
|»»» datatypes|[string]|false|none|none|
|»»» metadata|[DatasetMetadata](#schemadatasetmetadata)|false|none|none|
|»»»» earliest|string|true|none|none|
|»»»» enableAcceleration|boolean|true|none|none|
|»»»» fieldList|[string]|true|none|none|
|»»»» latestRunInfo|[DatasetMetadataRunInfo](#schemadatasetmetadataruninfo)|false|none|none|
|»»»»» earliestScannedTime|number|false|none|none|
|»»»»» finishedAt|number|false|none|none|
|»»»»» latestScannedTime|number|false|none|none|
|»»»»» objectCount|number|false|none|none|
|»»»» scanMode|string|true|none|none|
|»» storageLocationId|string|false|none|none|
|»» viewName|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|backfillStatus|scheduled|
|backfillStatus|pending|
|backfillStatus|started|
|backfillStatus|finished|
|backfillStatus|incomplete|
|lakehouseConnectionType|cache|
|lakehouseConnectionType|zeroPoint|
|format|json|
|format|ddss|
|format|parquet|
|scanMode|detailed|
|scanMode|quick|

<aside class="success">
This operation does not require authentication
</aside>

## Updates a list of datasets in the specified Lake

<a id="opIdupdateCriblLakeDatasetByLakeId"></a>

> Code samples

`PATCH /products/lake/lakes/{lakeId}/datasets`

Updates a list of datasets in the specified Lake

> Body parameter

```json
{
  "acceleratedFields": [
    "string"
  ],
  "bucketName": "string",
  "cacheConnection": {
    "acceleratedFields": [
      "string"
    ],
    "backfillStatus": "scheduled",
    "cacheRef": "string",
    "createdAt": 0,
    "lakehouseConnectionType": "cache",
    "migrationQueryId": "string",
    "retentionInDays": 0
  },
  "deletionStartedAt": 0,
  "description": "string",
  "format": "json",
  "httpDAUsed": true,
  "id": "string",
  "metrics": {
    "currentSizeBytes": 0,
    "metricsDate": "string"
  },
  "retentionPeriodInDays": 0,
  "searchConfig": {
    "datatypes": [
      "string"
    ],
    "metadata": {
      "earliest": "string",
      "enableAcceleration": true,
      "fieldList": [
        "string"
      ],
      "latestRunInfo": {
        "earliestScannedTime": 0,
        "finishedAt": 0,
        "latestScannedTime": 0,
        "objectCount": 0
      },
      "scanMode": "detailed"
    }
  },
  "storageLocationId": "string",
  "viewName": "string"
}
```

<h3 id="updates-a-list-of-datasets-in-the-specified-lake-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|lakeId|path|string|true|lake id that contains the Datasets|
|body|body|[CriblLakeDataset](#schemacribllakedataset)|true|CriblLakeDataset object|
|» acceleratedFields|body|[string]|false|none|
|» bucketName|body|string|false|none|
|» cacheConnection|body|[CacheConnection](#schemacacheconnection)|false|none|
|»» acceleratedFields|body|[string]|false|none|
|»» backfillStatus|body|[CacheConnectionBackfillStatus](#schemacacheconnectionbackfillstatus)|false|none|
|»» cacheRef|body|string|true|none|
|»» createdAt|body|number|true|none|
|»» lakehouseConnectionType|body|[LakehouseConnectionType](#schemalakehouseconnectiontype)|false|none|
|»» migrationQueryId|body|string|false|none|
|»» retentionInDays|body|number|true|none|
|» deletionStartedAt|body|number|false|none|
|» description|body|string|false|none|
|» format|body|string|false|none|
|» httpDAUsed|body|boolean|false|none|
|» id|body|string|true|none|
|» metrics|body|[LakeDatasetMetrics](#schemalakedatasetmetrics)|false|none|
|»» currentSizeBytes|body|number|true|none|
|»» metricsDate|body|string|true|none|
|» retentionPeriodInDays|body|number|false|none|
|» searchConfig|body|[LakeDatasetSearchConfig](#schemalakedatasetsearchconfig)|false|none|
|»» datatypes|body|[string]|false|none|
|»» metadata|body|[DatasetMetadata](#schemadatasetmetadata)|false|none|
|»»» earliest|body|string|true|none|
|»»» enableAcceleration|body|boolean|true|none|
|»»» fieldList|body|[string]|true|none|
|»»» latestRunInfo|body|[DatasetMetadataRunInfo](#schemadatasetmetadataruninfo)|false|none|
|»»»» earliestScannedTime|body|number|false|none|
|»»»» finishedAt|body|number|false|none|
|»»»» latestScannedTime|body|number|false|none|
|»»»» objectCount|body|number|false|none|
|»»» scanMode|body|string|true|none|
|» storageLocationId|body|string|false|none|
|» viewName|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»» backfillStatus|scheduled|
|»» backfillStatus|pending|
|»» backfillStatus|started|
|»» backfillStatus|finished|
|»» backfillStatus|incomplete|
|»» lakehouseConnectionType|cache|
|»» lakehouseConnectionType|zeroPoint|
|» format|json|
|» format|ddss|
|» format|parquet|
|»»» scanMode|detailed|
|»»» scanMode|quick|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "acceleratedFields": [
        "string"
      ],
      "bucketName": "string",
      "cacheConnection": {
        "acceleratedFields": [
          "string"
        ],
        "backfillStatus": "scheduled",
        "cacheRef": "string",
        "createdAt": 0,
        "lakehouseConnectionType": "cache",
        "migrationQueryId": "string",
        "retentionInDays": 0
      },
      "deletionStartedAt": 0,
      "description": "string",
      "format": "json",
      "httpDAUsed": true,
      "id": "string",
      "metrics": {
        "currentSizeBytes": 0,
        "metricsDate": "string"
      },
      "retentionPeriodInDays": 0,
      "searchConfig": {
        "datatypes": [
          "string"
        ],
        "metadata": {
          "earliest": "string",
          "enableAcceleration": true,
          "fieldList": [
            "string"
          ],
          "latestRunInfo": {
            "earliestScannedTime": 0,
            "finishedAt": 0,
            "latestScannedTime": 0,
            "objectCount": 0
          },
          "scanMode": "detailed"
        }
      },
      "storageLocationId": "string",
      "viewName": "string"
    }
  ]
}
```

<h3 id="updates-a-list-of-datasets-in-the-specified-lake-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CriblLakeDataset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="updates-a-list-of-datasets-in-the-specified-lake-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CriblLakeDataset](#schemacribllakedataset)]|false|none|none|
|»» acceleratedFields|[string]|false|none|none|
|»» bucketName|string|false|none|none|
|»» cacheConnection|[CacheConnection](#schemacacheconnection)|false|none|none|
|»»» acceleratedFields|[string]|false|none|none|
|»»» backfillStatus|[CacheConnectionBackfillStatus](#schemacacheconnectionbackfillstatus)|false|none|none|
|»»» cacheRef|string|true|none|none|
|»»» createdAt|number|true|none|none|
|»»» lakehouseConnectionType|[LakehouseConnectionType](#schemalakehouseconnectiontype)|false|none|none|
|»»» migrationQueryId|string|false|none|none|
|»»» retentionInDays|number|true|none|none|
|»» deletionStartedAt|number|false|none|none|
|»» description|string|false|none|none|
|»» format|string|false|none|none|
|»» httpDAUsed|boolean|false|none|none|
|»» id|string|true|none|none|
|»» metrics|[LakeDatasetMetrics](#schemalakedatasetmetrics)|false|none|none|
|»»» currentSizeBytes|number|true|none|none|
|»»» metricsDate|string|true|none|none|
|»» retentionPeriodInDays|number|false|none|none|
|»» searchConfig|[LakeDatasetSearchConfig](#schemalakedatasetsearchconfig)|false|none|none|
|»»» datatypes|[string]|false|none|none|
|»»» metadata|[DatasetMetadata](#schemadatasetmetadata)|false|none|none|
|»»»» earliest|string|true|none|none|
|»»»» enableAcceleration|boolean|true|none|none|
|»»»» fieldList|[string]|true|none|none|
|»»»» latestRunInfo|[DatasetMetadataRunInfo](#schemadatasetmetadataruninfo)|false|none|none|
|»»»»» earliestScannedTime|number|false|none|none|
|»»»»» finishedAt|number|false|none|none|
|»»»»» latestScannedTime|number|false|none|none|
|»»»»» objectCount|number|false|none|none|
|»»»» scanMode|string|true|none|none|
|»» storageLocationId|string|false|none|none|
|»» viewName|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|backfillStatus|scheduled|
|backfillStatus|pending|
|backfillStatus|started|
|backfillStatus|finished|
|backfillStatus|incomplete|
|lakehouseConnectionType|cache|
|lakehouseConnectionType|zeroPoint|
|format|json|
|format|ddss|
|format|parquet|
|scanMode|detailed|
|scanMode|quick|

<aside class="success">
This operation does not require authentication
</aside>

## Delete a Lake Dataset

<a id="opIddeleteCriblLakeDatasetByLakeIdAndId"></a>

> Code samples

`DELETE /products/lake/lakes/{lakeId}/datasets/{id}`

Delete the specified Lake Dataset in the specified Lake

<h3 id="delete-a-lake-dataset-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|lakeId|path|string|true|The <code>id</code> of the Lake that contains the Lake Dataset to delete.|
|id|path|string|true|The <code>id</code> of the Lake Dataset to delete.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "acceleratedFields": [
        "string"
      ],
      "bucketName": "string",
      "cacheConnection": {
        "acceleratedFields": [
          "string"
        ],
        "backfillStatus": "scheduled",
        "cacheRef": "string",
        "createdAt": 0,
        "lakehouseConnectionType": "cache",
        "migrationQueryId": "string",
        "retentionInDays": 0
      },
      "deletionStartedAt": 0,
      "description": "string",
      "format": "json",
      "httpDAUsed": true,
      "id": "string",
      "metrics": {
        "currentSizeBytes": 0,
        "metricsDate": "string"
      },
      "retentionPeriodInDays": 0,
      "searchConfig": {
        "datatypes": [
          "string"
        ],
        "metadata": {
          "earliest": "string",
          "enableAcceleration": true,
          "fieldList": [
            "string"
          ],
          "latestRunInfo": {
            "earliestScannedTime": 0,
            "finishedAt": 0,
            "latestScannedTime": 0,
            "objectCount": 0
          },
          "scanMode": "detailed"
        }
      },
      "storageLocationId": "string",
      "viewName": "string"
    }
  ]
}
```

<h3 id="delete-a-lake-dataset-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CriblLakeDataset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-a-lake-dataset-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CriblLakeDataset](#schemacribllakedataset)]|false|none|none|
|»» acceleratedFields|[string]|false|none|none|
|»» bucketName|string|false|none|none|
|»» cacheConnection|[CacheConnection](#schemacacheconnection)|false|none|none|
|»»» acceleratedFields|[string]|false|none|none|
|»»» backfillStatus|[CacheConnectionBackfillStatus](#schemacacheconnectionbackfillstatus)|false|none|none|
|»»» cacheRef|string|true|none|none|
|»»» createdAt|number|true|none|none|
|»»» lakehouseConnectionType|[LakehouseConnectionType](#schemalakehouseconnectiontype)|false|none|none|
|»»» migrationQueryId|string|false|none|none|
|»»» retentionInDays|number|true|none|none|
|»» deletionStartedAt|number|false|none|none|
|»» description|string|false|none|none|
|»» format|string|false|none|none|
|»» httpDAUsed|boolean|false|none|none|
|»» id|string|true|none|none|
|»» metrics|[LakeDatasetMetrics](#schemalakedatasetmetrics)|false|none|none|
|»»» currentSizeBytes|number|true|none|none|
|»»» metricsDate|string|true|none|none|
|»» retentionPeriodInDays|number|false|none|none|
|»» searchConfig|[LakeDatasetSearchConfig](#schemalakedatasetsearchconfig)|false|none|none|
|»»» datatypes|[string]|false|none|none|
|»»» metadata|[DatasetMetadata](#schemadatasetmetadata)|false|none|none|
|»»»» earliest|string|true|none|none|
|»»»» enableAcceleration|boolean|true|none|none|
|»»»» fieldList|[string]|true|none|none|
|»»»» latestRunInfo|[DatasetMetadataRunInfo](#schemadatasetmetadataruninfo)|false|none|none|
|»»»»» earliestScannedTime|number|false|none|none|
|»»»»» finishedAt|number|false|none|none|
|»»»»» latestScannedTime|number|false|none|none|
|»»»»» objectCount|number|false|none|none|
|»»»» scanMode|string|true|none|none|
|»» storageLocationId|string|false|none|none|
|»» viewName|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|backfillStatus|scheduled|
|backfillStatus|pending|
|backfillStatus|started|
|backfillStatus|finished|
|backfillStatus|incomplete|
|lakehouseConnectionType|cache|
|lakehouseConnectionType|zeroPoint|
|format|json|
|format|ddss|
|format|parquet|
|scanMode|detailed|
|scanMode|quick|

<aside class="success">
This operation does not require authentication
</aside>

## Get a Lake Dataset

<a id="opIdgetCriblLakeDatasetByLakeIdAndId"></a>

> Code samples

`GET /products/lake/lakes/{lakeId}/datasets/{id}`

Get the specified Lake Dataset in the specified Lake.

<h3 id="get-a-lake-dataset-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|lakeId|path|string|true|The <code>id</code> of the Lake that contains the Lake Dataset to get.|
|id|path|string|true|The <code>id</code> of the Lake Dataset to get.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "acceleratedFields": [
        "string"
      ],
      "bucketName": "string",
      "cacheConnection": {
        "acceleratedFields": [
          "string"
        ],
        "backfillStatus": "scheduled",
        "cacheRef": "string",
        "createdAt": 0,
        "lakehouseConnectionType": "cache",
        "migrationQueryId": "string",
        "retentionInDays": 0
      },
      "deletionStartedAt": 0,
      "description": "string",
      "format": "json",
      "httpDAUsed": true,
      "id": "string",
      "metrics": {
        "currentSizeBytes": 0,
        "metricsDate": "string"
      },
      "retentionPeriodInDays": 0,
      "searchConfig": {
        "datatypes": [
          "string"
        ],
        "metadata": {
          "earliest": "string",
          "enableAcceleration": true,
          "fieldList": [
            "string"
          ],
          "latestRunInfo": {
            "earliestScannedTime": 0,
            "finishedAt": 0,
            "latestScannedTime": 0,
            "objectCount": 0
          },
          "scanMode": "detailed"
        }
      },
      "storageLocationId": "string",
      "viewName": "string"
    }
  ]
}
```

<h3 id="get-a-lake-dataset-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CriblLakeDataset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-lake-dataset-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CriblLakeDataset](#schemacribllakedataset)]|false|none|none|
|»» acceleratedFields|[string]|false|none|none|
|»» bucketName|string|false|none|none|
|»» cacheConnection|[CacheConnection](#schemacacheconnection)|false|none|none|
|»»» acceleratedFields|[string]|false|none|none|
|»»» backfillStatus|[CacheConnectionBackfillStatus](#schemacacheconnectionbackfillstatus)|false|none|none|
|»»» cacheRef|string|true|none|none|
|»»» createdAt|number|true|none|none|
|»»» lakehouseConnectionType|[LakehouseConnectionType](#schemalakehouseconnectiontype)|false|none|none|
|»»» migrationQueryId|string|false|none|none|
|»»» retentionInDays|number|true|none|none|
|»» deletionStartedAt|number|false|none|none|
|»» description|string|false|none|none|
|»» format|string|false|none|none|
|»» httpDAUsed|boolean|false|none|none|
|»» id|string|true|none|none|
|»» metrics|[LakeDatasetMetrics](#schemalakedatasetmetrics)|false|none|none|
|»»» currentSizeBytes|number|true|none|none|
|»»» metricsDate|string|true|none|none|
|»» retentionPeriodInDays|number|false|none|none|
|»» searchConfig|[LakeDatasetSearchConfig](#schemalakedatasetsearchconfig)|false|none|none|
|»»» datatypes|[string]|false|none|none|
|»»» metadata|[DatasetMetadata](#schemadatasetmetadata)|false|none|none|
|»»»» earliest|string|true|none|none|
|»»»» enableAcceleration|boolean|true|none|none|
|»»»» fieldList|[string]|true|none|none|
|»»»» latestRunInfo|[DatasetMetadataRunInfo](#schemadatasetmetadataruninfo)|false|none|none|
|»»»»» earliestScannedTime|number|false|none|none|
|»»»»» finishedAt|number|false|none|none|
|»»»»» latestScannedTime|number|false|none|none|
|»»»»» objectCount|number|false|none|none|
|»»»» scanMode|string|true|none|none|
|»» storageLocationId|string|false|none|none|
|»» viewName|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|backfillStatus|scheduled|
|backfillStatus|pending|
|backfillStatus|started|
|backfillStatus|finished|
|backfillStatus|incomplete|
|lakehouseConnectionType|cache|
|lakehouseConnectionType|zeroPoint|
|format|json|
|format|ddss|
|format|parquet|
|scanMode|detailed|
|scanMode|quick|

<aside class="success">
This operation does not require authentication
</aside>

## Update a Lake Dataset

<a id="opIdupdateCriblLakeDatasetByLakeIdAndId"></a>

> Code samples

`PATCH /products/lake/lakes/{lakeId}/datasets/{id}`

Update the specified Lake Dataset in the specified Lake.

> Body parameter

```json
{
  "acceleratedFields": [
    "string"
  ],
  "bucketName": "string",
  "cacheConnection": {
    "acceleratedFields": [
      "string"
    ],
    "backfillStatus": "scheduled",
    "cacheRef": "string",
    "createdAt": 0,
    "lakehouseConnectionType": "cache",
    "migrationQueryId": "string",
    "retentionInDays": 0
  },
  "deletionStartedAt": 0,
  "description": "string",
  "format": "json",
  "httpDAUsed": true,
  "id": "string",
  "metrics": {
    "currentSizeBytes": 0,
    "metricsDate": "string"
  },
  "retentionPeriodInDays": 0,
  "searchConfig": {
    "datatypes": [
      "string"
    ],
    "metadata": {
      "earliest": "string",
      "enableAcceleration": true,
      "fieldList": [
        "string"
      ],
      "latestRunInfo": {
        "earliestScannedTime": 0,
        "finishedAt": 0,
        "latestScannedTime": 0,
        "objectCount": 0
      },
      "scanMode": "detailed"
    }
  },
  "storageLocationId": "string",
  "viewName": "string"
}
```

<h3 id="update-a-lake-dataset-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|lakeId|path|string|true|The <code>id</code> of the Lake that contains the Lake Dataset to update.|
|id|path|string|true|The <code>id</code> of the Lake Dataset to update.|
|body|body|[CriblLakeDatasetUpdate](#schemacribllakedatasetupdate)|true|CriblLakeDatasetUpdate object|
|» acceleratedFields|body|[string]|false|none|
|» bucketName|body|string|false|none|
|» cacheConnection|body|[CacheConnection](#schemacacheconnection)|false|none|
|»» acceleratedFields|body|[string]|false|none|
|»» backfillStatus|body|[CacheConnectionBackfillStatus](#schemacacheconnectionbackfillstatus)|false|none|
|»» cacheRef|body|string|true|none|
|»» createdAt|body|number|true|none|
|»» lakehouseConnectionType|body|[LakehouseConnectionType](#schemalakehouseconnectiontype)|false|none|
|»» migrationQueryId|body|string|false|none|
|»» retentionInDays|body|number|true|none|
|» deletionStartedAt|body|number|false|none|
|» description|body|string|false|none|
|» format|body|string|false|none|
|» httpDAUsed|body|boolean|false|none|
|» id|body|string|false|none|
|» metrics|body|[LakeDatasetMetrics](#schemalakedatasetmetrics)|false|none|
|»» currentSizeBytes|body|number|true|none|
|»» metricsDate|body|string|true|none|
|» retentionPeriodInDays|body|number|false|none|
|» searchConfig|body|[LakeDatasetSearchConfig](#schemalakedatasetsearchconfig)|false|none|
|»» datatypes|body|[string]|false|none|
|»» metadata|body|[DatasetMetadata](#schemadatasetmetadata)|false|none|
|»»» earliest|body|string|true|none|
|»»» enableAcceleration|body|boolean|true|none|
|»»» fieldList|body|[string]|true|none|
|»»» latestRunInfo|body|[DatasetMetadataRunInfo](#schemadatasetmetadataruninfo)|false|none|
|»»»» earliestScannedTime|body|number|false|none|
|»»»» finishedAt|body|number|false|none|
|»»»» latestScannedTime|body|number|false|none|
|»»»» objectCount|body|number|false|none|
|»»» scanMode|body|string|true|none|
|» storageLocationId|body|string|false|none|
|» viewName|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»» backfillStatus|scheduled|
|»» backfillStatus|pending|
|»» backfillStatus|started|
|»» backfillStatus|finished|
|»» backfillStatus|incomplete|
|»» lakehouseConnectionType|cache|
|»» lakehouseConnectionType|zeroPoint|
|» format|json|
|» format|ddss|
|» format|parquet|
|»»» scanMode|detailed|
|»»» scanMode|quick|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "acceleratedFields": [
        "string"
      ],
      "bucketName": "string",
      "cacheConnection": {
        "acceleratedFields": [
          "string"
        ],
        "backfillStatus": "scheduled",
        "cacheRef": "string",
        "createdAt": 0,
        "lakehouseConnectionType": "cache",
        "migrationQueryId": "string",
        "retentionInDays": 0
      },
      "deletionStartedAt": 0,
      "description": "string",
      "format": "json",
      "httpDAUsed": true,
      "id": "string",
      "metrics": {
        "currentSizeBytes": 0,
        "metricsDate": "string"
      },
      "retentionPeriodInDays": 0,
      "searchConfig": {
        "datatypes": [
          "string"
        ],
        "metadata": {
          "earliest": "string",
          "enableAcceleration": true,
          "fieldList": [
            "string"
          ],
          "latestRunInfo": {
            "earliestScannedTime": 0,
            "finishedAt": 0,
            "latestScannedTime": 0,
            "objectCount": 0
          },
          "scanMode": "detailed"
        }
      },
      "storageLocationId": "string",
      "viewName": "string"
    }
  ]
}
```

<h3 id="update-a-lake-dataset-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CriblLakeDataset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-a-lake-dataset-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CriblLakeDataset](#schemacribllakedataset)]|false|none|none|
|»» acceleratedFields|[string]|false|none|none|
|»» bucketName|string|false|none|none|
|»» cacheConnection|[CacheConnection](#schemacacheconnection)|false|none|none|
|»»» acceleratedFields|[string]|false|none|none|
|»»» backfillStatus|[CacheConnectionBackfillStatus](#schemacacheconnectionbackfillstatus)|false|none|none|
|»»» cacheRef|string|true|none|none|
|»»» createdAt|number|true|none|none|
|»»» lakehouseConnectionType|[LakehouseConnectionType](#schemalakehouseconnectiontype)|false|none|none|
|»»» migrationQueryId|string|false|none|none|
|»»» retentionInDays|number|true|none|none|
|»» deletionStartedAt|number|false|none|none|
|»» description|string|false|none|none|
|»» format|string|false|none|none|
|»» httpDAUsed|boolean|false|none|none|
|»» id|string|true|none|none|
|»» metrics|[LakeDatasetMetrics](#schemalakedatasetmetrics)|false|none|none|
|»»» currentSizeBytes|number|true|none|none|
|»»» metricsDate|string|true|none|none|
|»» retentionPeriodInDays|number|false|none|none|
|»» searchConfig|[LakeDatasetSearchConfig](#schemalakedatasetsearchconfig)|false|none|none|
|»»» datatypes|[string]|false|none|none|
|»»» metadata|[DatasetMetadata](#schemadatasetmetadata)|false|none|none|
|»»»» earliest|string|true|none|none|
|»»»» enableAcceleration|boolean|true|none|none|
|»»»» fieldList|[string]|true|none|none|
|»»»» latestRunInfo|[DatasetMetadataRunInfo](#schemadatasetmetadataruninfo)|false|none|none|
|»»»»» earliestScannedTime|number|false|none|none|
|»»»»» finishedAt|number|false|none|none|
|»»»»» latestScannedTime|number|false|none|none|
|»»»»» objectCount|number|false|none|none|
|»»»» scanMode|string|true|none|none|
|»» storageLocationId|string|false|none|none|
|»» viewName|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|backfillStatus|scheduled|
|backfillStatus|pending|
|backfillStatus|started|
|backfillStatus|finished|
|backfillStatus|incomplete|
|lakehouseConnectionType|cache|
|lakehouseConnectionType|zeroPoint|
|format|json|
|format|ddss|
|format|parquet|
|scanMode|detailed|
|scanMode|quick|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_CriblLakeDataset">CriblLakeDataset</h2>
<!-- backwards compatibility -->
<a id="schemacribllakedataset"></a>
<a id="schema_CriblLakeDataset"></a>
<a id="tocScribllakedataset"></a>
<a id="tocscribllakedataset"></a>

```json
{
  "acceleratedFields": [
    "string"
  ],
  "bucketName": "string",
  "cacheConnection": {
    "acceleratedFields": [
      "string"
    ],
    "backfillStatus": "scheduled",
    "cacheRef": "string",
    "createdAt": 0,
    "lakehouseConnectionType": "cache",
    "migrationQueryId": "string",
    "retentionInDays": 0
  },
  "deletionStartedAt": 0,
  "description": "string",
  "format": "json",
  "httpDAUsed": true,
  "id": "string",
  "metrics": {
    "currentSizeBytes": 0,
    "metricsDate": "string"
  },
  "retentionPeriodInDays": 0,
  "searchConfig": {
    "datatypes": [
      "string"
    ],
    "metadata": {
      "earliest": "string",
      "enableAcceleration": true,
      "fieldList": [
        "string"
      ],
      "latestRunInfo": {
        "earliestScannedTime": 0,
        "finishedAt": 0,
        "latestScannedTime": 0,
        "objectCount": 0
      },
      "scanMode": "detailed"
    }
  },
  "storageLocationId": "string",
  "viewName": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|acceleratedFields|[string]|false|none|none|
|bucketName|string|false|none|none|
|cacheConnection|[CacheConnection](#schemacacheconnection)|false|none|none|
|deletionStartedAt|number|false|none|none|
|description|string|false|none|none|
|format|string|false|none|none|
|httpDAUsed|boolean|false|none|none|
|id|string|true|none|none|
|metrics|[LakeDatasetMetrics](#schemalakedatasetmetrics)|false|none|none|
|retentionPeriodInDays|number|false|none|none|
|searchConfig|[LakeDatasetSearchConfig](#schemalakedatasetsearchconfig)|false|none|none|
|storageLocationId|string|false|none|none|
|viewName|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|format|json|
|format|ddss|
|format|parquet|

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

<h2 id="tocS_CriblLakeDatasetUpdate">CriblLakeDatasetUpdate</h2>
<!-- backwards compatibility -->
<a id="schemacribllakedatasetupdate"></a>
<a id="schema_CriblLakeDatasetUpdate"></a>
<a id="tocScribllakedatasetupdate"></a>
<a id="tocscribllakedatasetupdate"></a>

```json
{
  "acceleratedFields": [
    "string"
  ],
  "bucketName": "string",
  "cacheConnection": {
    "acceleratedFields": [
      "string"
    ],
    "backfillStatus": "scheduled",
    "cacheRef": "string",
    "createdAt": 0,
    "lakehouseConnectionType": "cache",
    "migrationQueryId": "string",
    "retentionInDays": 0
  },
  "deletionStartedAt": 0,
  "description": "string",
  "format": "json",
  "httpDAUsed": true,
  "id": "string",
  "metrics": {
    "currentSizeBytes": 0,
    "metricsDate": "string"
  },
  "retentionPeriodInDays": 0,
  "searchConfig": {
    "datatypes": [
      "string"
    ],
    "metadata": {
      "earliest": "string",
      "enableAcceleration": true,
      "fieldList": [
        "string"
      ],
      "latestRunInfo": {
        "earliestScannedTime": 0,
        "finishedAt": 0,
        "latestScannedTime": 0,
        "objectCount": 0
      },
      "scanMode": "detailed"
    }
  },
  "storageLocationId": "string",
  "viewName": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|acceleratedFields|[string]|false|none|none|
|bucketName|string|false|none|none|
|cacheConnection|[CacheConnection](#schemacacheconnection)|false|none|none|
|deletionStartedAt|number|false|none|none|
|description|string|false|none|none|
|format|string|false|none|none|
|httpDAUsed|boolean|false|none|none|
|id|string|false|none|none|
|metrics|[LakeDatasetMetrics](#schemalakedatasetmetrics)|false|none|none|
|retentionPeriodInDays|number|false|none|none|
|searchConfig|[LakeDatasetSearchConfig](#schemalakedatasetsearchconfig)|false|none|none|
|storageLocationId|string|false|none|none|
|viewName|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|format|json|
|format|ddss|
|format|parquet|

<h2 id="tocS_CacheConnection">CacheConnection</h2>
<!-- backwards compatibility -->
<a id="schemacacheconnection"></a>
<a id="schema_CacheConnection"></a>
<a id="tocScacheconnection"></a>
<a id="tocscacheconnection"></a>

```json
{
  "acceleratedFields": [
    "string"
  ],
  "backfillStatus": "scheduled",
  "cacheRef": "string",
  "createdAt": 0,
  "lakehouseConnectionType": "cache",
  "migrationQueryId": "string",
  "retentionInDays": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|acceleratedFields|[string]|false|none|none|
|backfillStatus|[CacheConnectionBackfillStatus](#schemacacheconnectionbackfillstatus)|false|none|none|
|cacheRef|string|true|none|none|
|createdAt|number|true|none|none|
|lakehouseConnectionType|[LakehouseConnectionType](#schemalakehouseconnectiontype)|false|none|none|
|migrationQueryId|string|false|none|none|
|retentionInDays|number|true|none|none|

<h2 id="tocS_LakeDatasetMetrics">LakeDatasetMetrics</h2>
<!-- backwards compatibility -->
<a id="schemalakedatasetmetrics"></a>
<a id="schema_LakeDatasetMetrics"></a>
<a id="tocSlakedatasetmetrics"></a>
<a id="tocslakedatasetmetrics"></a>

```json
{
  "currentSizeBytes": 0,
  "metricsDate": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|currentSizeBytes|number|true|none|none|
|metricsDate|string|true|none|none|

<h2 id="tocS_LakeDatasetSearchConfig">LakeDatasetSearchConfig</h2>
<!-- backwards compatibility -->
<a id="schemalakedatasetsearchconfig"></a>
<a id="schema_LakeDatasetSearchConfig"></a>
<a id="tocSlakedatasetsearchconfig"></a>
<a id="tocslakedatasetsearchconfig"></a>

```json
{
  "datatypes": [
    "string"
  ],
  "metadata": {
    "earliest": "string",
    "enableAcceleration": true,
    "fieldList": [
      "string"
    ],
    "latestRunInfo": {
      "earliestScannedTime": 0,
      "finishedAt": 0,
      "latestScannedTime": 0,
      "objectCount": 0
    },
    "scanMode": "detailed"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|datatypes|[string]|false|none|none|
|metadata|[DatasetMetadata](#schemadatasetmetadata)|false|none|none|

<h2 id="tocS_CacheConnectionBackfillStatus">CacheConnectionBackfillStatus</h2>
<!-- backwards compatibility -->
<a id="schemacacheconnectionbackfillstatus"></a>
<a id="schema_CacheConnectionBackfillStatus"></a>
<a id="tocScacheconnectionbackfillstatus"></a>
<a id="tocscacheconnectionbackfillstatus"></a>

```json
"scheduled"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|scheduled|
|*anonymous*|pending|
|*anonymous*|started|
|*anonymous*|finished|
|*anonymous*|incomplete|

<h2 id="tocS_LakehouseConnectionType">LakehouseConnectionType</h2>
<!-- backwards compatibility -->
<a id="schemalakehouseconnectiontype"></a>
<a id="schema_LakehouseConnectionType"></a>
<a id="tocSlakehouseconnectiontype"></a>
<a id="tocslakehouseconnectiontype"></a>

```json
"cache"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|cache|
|*anonymous*|zeroPoint|

<h2 id="tocS_DatasetMetadata">DatasetMetadata</h2>
<!-- backwards compatibility -->
<a id="schemadatasetmetadata"></a>
<a id="schema_DatasetMetadata"></a>
<a id="tocSdatasetmetadata"></a>
<a id="tocsdatasetmetadata"></a>

```json
{
  "earliest": "string",
  "enableAcceleration": true,
  "fieldList": [
    "string"
  ],
  "latestRunInfo": {
    "earliestScannedTime": 0,
    "finishedAt": 0,
    "latestScannedTime": 0,
    "objectCount": 0
  },
  "scanMode": "detailed"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|earliest|string|true|none|none|
|enableAcceleration|boolean|true|none|none|
|fieldList|[string]|true|none|none|
|latestRunInfo|[DatasetMetadataRunInfo](#schemadatasetmetadataruninfo)|false|none|none|
|scanMode|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|scanMode|detailed|
|scanMode|quick|

<h2 id="tocS_DatasetMetadataRunInfo">DatasetMetadataRunInfo</h2>
<!-- backwards compatibility -->
<a id="schemadatasetmetadataruninfo"></a>
<a id="schema_DatasetMetadataRunInfo"></a>
<a id="tocSdatasetmetadataruninfo"></a>
<a id="tocsdatasetmetadataruninfo"></a>

```json
{
  "earliestScannedTime": 0,
  "finishedAt": 0,
  "latestScannedTime": 0,
  "objectCount": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|earliestScannedTime|number|false|none|none|
|finishedAt|number|false|none|none|
|latestScannedTime|number|false|none|none|
|objectCount|number|false|none|none|

