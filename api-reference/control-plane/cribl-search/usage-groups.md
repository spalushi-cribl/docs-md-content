
<h1 id="cribl-search-api-usage-groups">Cribl Search API - Usage Groups v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-search-api-usage-groups-usage-groups">Usage Groups</h1>

## Get a list of UsageGroup objects

<a id="opIdlistUsageGroup"></a>

> Code samples

`GET /search/usage-groups`

Get a list of UsageGroup objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "coordinatorHeapMemoryLimit": 0,
      "description": "string",
      "enabled": true,
      "id": "string",
      "rules": {
        "coordinatorHeapMemoryLimit": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxBytesReadPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxConcurrentAdhocSearchesPerUser": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxConcurrentScheduledSearchesPerUser": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxConcurrentSearches": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxExecutorsPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxRelativeEarliestTimerange": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxResultsPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxRunningTimePerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxTimerangeWidth": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        }
      },
      "users": {
        "property1": {
          "email": "string",
          "id": "string"
        },
        "property2": {
          "email": "string",
          "id": "string"
        }
      },
      "usersCount": 0
    }
  ]
}
```

<h3 id="get-a-list-of-usagegroup-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UsageGroup objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-usagegroup-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UsageGroup](#schemausagegroup)]|false|none|none|
|»» coordinatorHeapMemoryLimit|number|false|none|none|
|»» description|string|false|none|none|
|»» enabled|boolean|false|none|none|
|»» id|string|true|none|none|
|»» rules|[LimitRuleDefinitions](#schemalimitruledefinitions)|true|none|none|
|»»» coordinatorHeapMemoryLimit|[LimitRule](#schemalimitrule)|true|none|none|
|»»»» description|string|false|none|none|
|»»»» limit|number|true|none|none|
|»»»» metric|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» maxBytesReadPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxConcurrentAdhocSearchesPerUser|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»»» description|string|false|none|none|
|»»»» limit|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» metric|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» maxConcurrentScheduledSearchesPerUser|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»» maxConcurrentSearches|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»» maxExecutorsPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxRelativeEarliestTimerange|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxResultsPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxRunningTimePerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxTimerangeWidth|[LimitRule](#schemalimitrule)|true|none|none|
|»» users|object|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» email|string|true|none|none|
|»»»» id|string|true|none|none|
|»» usersCount|number|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|maxRelativeEarliestTimerange|
|type|maxTimerangeWidth|
|type|maxBytesReadPerSearch|
|type|maxRunningTimePerSearch|
|type|maxResultsPerSearch|
|type|maxExecutorsPerSearch|
|type|coordinatorHeapMemoryLimit|
|type|maxConcurrentAdhocSearchesPerUser|
|type|maxConcurrentScheduledSearchesPerUser|
|type|maxConcurrentSearches|

<aside class="success">
This operation does not require authentication
</aside>

## Create UsageGroup

<a id="opIdcreateUsageGroup"></a>

> Code samples

`POST /search/usage-groups`

Create UsageGroup

> Body parameter

```json
{
  "coordinatorHeapMemoryLimit": 0,
  "description": "string",
  "enabled": true,
  "id": "string",
  "rules": {
    "coordinatorHeapMemoryLimit": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxBytesReadPerSearch": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxConcurrentAdhocSearchesPerUser": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxConcurrentAdhocSearchesPerUser"
    },
    "maxConcurrentScheduledSearchesPerUser": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxConcurrentAdhocSearchesPerUser"
    },
    "maxConcurrentSearches": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxConcurrentAdhocSearchesPerUser"
    },
    "maxExecutorsPerSearch": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxRelativeEarliestTimerange": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxResultsPerSearch": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxRunningTimePerSearch": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxTimerangeWidth": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    }
  },
  "users": {
    "property1": {
      "email": "string",
      "id": "string"
    },
    "property2": {
      "email": "string",
      "id": "string"
    }
  },
  "usersCount": 0
}
```

<h3 id="create-usagegroup-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[UsageGroup](#schemausagegroup)|true|New UsageGroup object|
|» coordinatorHeapMemoryLimit|body|number|false|none|
|» description|body|string|false|none|
|» enabled|body|boolean|false|none|
|» id|body|string|true|none|
|» rules|body|[LimitRuleDefinitions](#schemalimitruledefinitions)|true|none|
|»» coordinatorHeapMemoryLimit|body|[LimitRule](#schemalimitrule)|true|none|
|»»» description|body|string|false|none|
|»»» limit|body|number|true|none|
|»»» metric|body|string|false|none|
|»»» type|body|string|true|none|
|»» maxBytesReadPerSearch|body|[LimitRule](#schemalimitrule)|true|none|
|»» maxConcurrentAdhocSearchesPerUser|body|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|
|»»» description|body|string|false|none|
|»»» limit|body|any|true|none|
|»»»» *anonymous*|body|number|false|none|
|»»»» *anonymous*|body|string|false|none|
|»»» metric|body|string|false|none|
|»»» type|body|string|true|none|
|»» maxConcurrentScheduledSearchesPerUser|body|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|
|»» maxConcurrentSearches|body|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|
|»» maxExecutorsPerSearch|body|[LimitRule](#schemalimitrule)|true|none|
|»» maxRelativeEarliestTimerange|body|[LimitRule](#schemalimitrule)|true|none|
|»» maxResultsPerSearch|body|[LimitRule](#schemalimitrule)|true|none|
|»» maxRunningTimePerSearch|body|[LimitRule](#schemalimitrule)|true|none|
|»» maxTimerangeWidth|body|[LimitRule](#schemalimitrule)|true|none|
|» users|body|object|false|none|
|»» **additionalProperties**|body|object|false|none|
|»»» email|body|string|true|none|
|»»» id|body|string|true|none|
|» usersCount|body|number|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» type|maxRelativeEarliestTimerange|
|»»» type|maxTimerangeWidth|
|»»» type|maxBytesReadPerSearch|
|»»» type|maxRunningTimePerSearch|
|»»» type|maxResultsPerSearch|
|»»» type|maxExecutorsPerSearch|
|»»» type|coordinatorHeapMemoryLimit|
|»»» type|maxConcurrentAdhocSearchesPerUser|
|»»» type|maxConcurrentScheduledSearchesPerUser|
|»»» type|maxConcurrentSearches|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "coordinatorHeapMemoryLimit": 0,
      "description": "string",
      "enabled": true,
      "id": "string",
      "rules": {
        "coordinatorHeapMemoryLimit": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxBytesReadPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxConcurrentAdhocSearchesPerUser": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxConcurrentScheduledSearchesPerUser": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxConcurrentSearches": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxExecutorsPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxRelativeEarliestTimerange": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxResultsPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxRunningTimePerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxTimerangeWidth": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        }
      },
      "users": {
        "property1": {
          "email": "string",
          "id": "string"
        },
        "property2": {
          "email": "string",
          "id": "string"
        }
      },
      "usersCount": 0
    }
  ]
}
```

<h3 id="create-usagegroup-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UsageGroup objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-usagegroup-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UsageGroup](#schemausagegroup)]|false|none|none|
|»» coordinatorHeapMemoryLimit|number|false|none|none|
|»» description|string|false|none|none|
|»» enabled|boolean|false|none|none|
|»» id|string|true|none|none|
|»» rules|[LimitRuleDefinitions](#schemalimitruledefinitions)|true|none|none|
|»»» coordinatorHeapMemoryLimit|[LimitRule](#schemalimitrule)|true|none|none|
|»»»» description|string|false|none|none|
|»»»» limit|number|true|none|none|
|»»»» metric|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» maxBytesReadPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxConcurrentAdhocSearchesPerUser|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»»» description|string|false|none|none|
|»»»» limit|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» metric|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» maxConcurrentScheduledSearchesPerUser|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»» maxConcurrentSearches|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»» maxExecutorsPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxRelativeEarliestTimerange|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxResultsPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxRunningTimePerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxTimerangeWidth|[LimitRule](#schemalimitrule)|true|none|none|
|»» users|object|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» email|string|true|none|none|
|»»»» id|string|true|none|none|
|»» usersCount|number|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|maxRelativeEarliestTimerange|
|type|maxTimerangeWidth|
|type|maxBytesReadPerSearch|
|type|maxRunningTimePerSearch|
|type|maxResultsPerSearch|
|type|maxExecutorsPerSearch|
|type|coordinatorHeapMemoryLimit|
|type|maxConcurrentAdhocSearchesPerUser|
|type|maxConcurrentScheduledSearchesPerUser|
|type|maxConcurrentSearches|

<aside class="success">
This operation does not require authentication
</aside>

## Delete UsageGroup

<a id="opIddeleteUsageGroupById"></a>

> Code samples

`DELETE /search/usage-groups/{id}`

Delete UsageGroup

<h3 id="delete-usagegroup-parameters">Parameters</h3>

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
      "coordinatorHeapMemoryLimit": 0,
      "description": "string",
      "enabled": true,
      "id": "string",
      "rules": {
        "coordinatorHeapMemoryLimit": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxBytesReadPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxConcurrentAdhocSearchesPerUser": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxConcurrentScheduledSearchesPerUser": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxConcurrentSearches": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxExecutorsPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxRelativeEarliestTimerange": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxResultsPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxRunningTimePerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxTimerangeWidth": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        }
      },
      "users": {
        "property1": {
          "email": "string",
          "id": "string"
        },
        "property2": {
          "email": "string",
          "id": "string"
        }
      },
      "usersCount": 0
    }
  ]
}
```

<h3 id="delete-usagegroup-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UsageGroup objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-usagegroup-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UsageGroup](#schemausagegroup)]|false|none|none|
|»» coordinatorHeapMemoryLimit|number|false|none|none|
|»» description|string|false|none|none|
|»» enabled|boolean|false|none|none|
|»» id|string|true|none|none|
|»» rules|[LimitRuleDefinitions](#schemalimitruledefinitions)|true|none|none|
|»»» coordinatorHeapMemoryLimit|[LimitRule](#schemalimitrule)|true|none|none|
|»»»» description|string|false|none|none|
|»»»» limit|number|true|none|none|
|»»»» metric|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» maxBytesReadPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxConcurrentAdhocSearchesPerUser|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»»» description|string|false|none|none|
|»»»» limit|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» metric|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» maxConcurrentScheduledSearchesPerUser|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»» maxConcurrentSearches|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»» maxExecutorsPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxRelativeEarliestTimerange|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxResultsPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxRunningTimePerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxTimerangeWidth|[LimitRule](#schemalimitrule)|true|none|none|
|»» users|object|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» email|string|true|none|none|
|»»»» id|string|true|none|none|
|»» usersCount|number|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|maxRelativeEarliestTimerange|
|type|maxTimerangeWidth|
|type|maxBytesReadPerSearch|
|type|maxRunningTimePerSearch|
|type|maxResultsPerSearch|
|type|maxExecutorsPerSearch|
|type|coordinatorHeapMemoryLimit|
|type|maxConcurrentAdhocSearchesPerUser|
|type|maxConcurrentScheduledSearchesPerUser|
|type|maxConcurrentSearches|

<aside class="success">
This operation does not require authentication
</aside>

## Get UsageGroup by ID

<a id="opIdgetUsageGroupById"></a>

> Code samples

`GET /search/usage-groups/{id}`

Get UsageGroup by ID

<h3 id="get-usagegroup-by-id-parameters">Parameters</h3>

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
      "coordinatorHeapMemoryLimit": 0,
      "description": "string",
      "enabled": true,
      "id": "string",
      "rules": {
        "coordinatorHeapMemoryLimit": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxBytesReadPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxConcurrentAdhocSearchesPerUser": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxConcurrentScheduledSearchesPerUser": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxConcurrentSearches": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxExecutorsPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxRelativeEarliestTimerange": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxResultsPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxRunningTimePerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxTimerangeWidth": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        }
      },
      "users": {
        "property1": {
          "email": "string",
          "id": "string"
        },
        "property2": {
          "email": "string",
          "id": "string"
        }
      },
      "usersCount": 0
    }
  ]
}
```

<h3 id="get-usagegroup-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UsageGroup objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-usagegroup-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UsageGroup](#schemausagegroup)]|false|none|none|
|»» coordinatorHeapMemoryLimit|number|false|none|none|
|»» description|string|false|none|none|
|»» enabled|boolean|false|none|none|
|»» id|string|true|none|none|
|»» rules|[LimitRuleDefinitions](#schemalimitruledefinitions)|true|none|none|
|»»» coordinatorHeapMemoryLimit|[LimitRule](#schemalimitrule)|true|none|none|
|»»»» description|string|false|none|none|
|»»»» limit|number|true|none|none|
|»»»» metric|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» maxBytesReadPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxConcurrentAdhocSearchesPerUser|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»»» description|string|false|none|none|
|»»»» limit|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» metric|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» maxConcurrentScheduledSearchesPerUser|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»» maxConcurrentSearches|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»» maxExecutorsPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxRelativeEarliestTimerange|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxResultsPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxRunningTimePerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxTimerangeWidth|[LimitRule](#schemalimitrule)|true|none|none|
|»» users|object|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» email|string|true|none|none|
|»»»» id|string|true|none|none|
|»» usersCount|number|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|maxRelativeEarliestTimerange|
|type|maxTimerangeWidth|
|type|maxBytesReadPerSearch|
|type|maxRunningTimePerSearch|
|type|maxResultsPerSearch|
|type|maxExecutorsPerSearch|
|type|coordinatorHeapMemoryLimit|
|type|maxConcurrentAdhocSearchesPerUser|
|type|maxConcurrentScheduledSearchesPerUser|
|type|maxConcurrentSearches|

<aside class="success">
This operation does not require authentication
</aside>

## Update UsageGroup

<a id="opIdupdateUsageGroupById"></a>

> Code samples

`PATCH /search/usage-groups/{id}`

Update UsageGroup

> Body parameter

```json
{
  "coordinatorHeapMemoryLimit": 0,
  "description": "string",
  "enabled": true,
  "id": "string",
  "rules": {
    "coordinatorHeapMemoryLimit": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxBytesReadPerSearch": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxConcurrentAdhocSearchesPerUser": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxConcurrentAdhocSearchesPerUser"
    },
    "maxConcurrentScheduledSearchesPerUser": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxConcurrentAdhocSearchesPerUser"
    },
    "maxConcurrentSearches": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxConcurrentAdhocSearchesPerUser"
    },
    "maxExecutorsPerSearch": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxRelativeEarliestTimerange": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxResultsPerSearch": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxRunningTimePerSearch": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxTimerangeWidth": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    }
  },
  "users": {
    "property1": {
      "email": "string",
      "id": "string"
    },
    "property2": {
      "email": "string",
      "id": "string"
    }
  },
  "usersCount": 0
}
```

<h3 id="update-usagegroup-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[UsageGroup](#schemausagegroup)|true|UsageGroup object to be updated|
|» coordinatorHeapMemoryLimit|body|number|false|none|
|» description|body|string|false|none|
|» enabled|body|boolean|false|none|
|» id|body|string|true|none|
|» rules|body|[LimitRuleDefinitions](#schemalimitruledefinitions)|true|none|
|»» coordinatorHeapMemoryLimit|body|[LimitRule](#schemalimitrule)|true|none|
|»»» description|body|string|false|none|
|»»» limit|body|number|true|none|
|»»» metric|body|string|false|none|
|»»» type|body|string|true|none|
|»» maxBytesReadPerSearch|body|[LimitRule](#schemalimitrule)|true|none|
|»» maxConcurrentAdhocSearchesPerUser|body|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|
|»»» description|body|string|false|none|
|»»» limit|body|any|true|none|
|»»»» *anonymous*|body|number|false|none|
|»»»» *anonymous*|body|string|false|none|
|»»» metric|body|string|false|none|
|»»» type|body|string|true|none|
|»» maxConcurrentScheduledSearchesPerUser|body|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|
|»» maxConcurrentSearches|body|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|
|»» maxExecutorsPerSearch|body|[LimitRule](#schemalimitrule)|true|none|
|»» maxRelativeEarliestTimerange|body|[LimitRule](#schemalimitrule)|true|none|
|»» maxResultsPerSearch|body|[LimitRule](#schemalimitrule)|true|none|
|»» maxRunningTimePerSearch|body|[LimitRule](#schemalimitrule)|true|none|
|»» maxTimerangeWidth|body|[LimitRule](#schemalimitrule)|true|none|
|» users|body|object|false|none|
|»» **additionalProperties**|body|object|false|none|
|»»» email|body|string|true|none|
|»»» id|body|string|true|none|
|» usersCount|body|number|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» type|maxRelativeEarliestTimerange|
|»»» type|maxTimerangeWidth|
|»»» type|maxBytesReadPerSearch|
|»»» type|maxRunningTimePerSearch|
|»»» type|maxResultsPerSearch|
|»»» type|maxExecutorsPerSearch|
|»»» type|coordinatorHeapMemoryLimit|
|»»» type|maxConcurrentAdhocSearchesPerUser|
|»»» type|maxConcurrentScheduledSearchesPerUser|
|»»» type|maxConcurrentSearches|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "coordinatorHeapMemoryLimit": 0,
      "description": "string",
      "enabled": true,
      "id": "string",
      "rules": {
        "coordinatorHeapMemoryLimit": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxBytesReadPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxConcurrentAdhocSearchesPerUser": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxConcurrentScheduledSearchesPerUser": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxConcurrentSearches": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxConcurrentAdhocSearchesPerUser"
        },
        "maxExecutorsPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxRelativeEarliestTimerange": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxResultsPerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxRunningTimePerSearch": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        },
        "maxTimerangeWidth": {
          "description": "string",
          "limit": 0,
          "metric": "string",
          "type": "maxRelativeEarliestTimerange"
        }
      },
      "users": {
        "property1": {
          "email": "string",
          "id": "string"
        },
        "property2": {
          "email": "string",
          "id": "string"
        }
      },
      "usersCount": 0
    }
  ]
}
```

<h3 id="update-usagegroup-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UsageGroup objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-usagegroup-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UsageGroup](#schemausagegroup)]|false|none|none|
|»» coordinatorHeapMemoryLimit|number|false|none|none|
|»» description|string|false|none|none|
|»» enabled|boolean|false|none|none|
|»» id|string|true|none|none|
|»» rules|[LimitRuleDefinitions](#schemalimitruledefinitions)|true|none|none|
|»»» coordinatorHeapMemoryLimit|[LimitRule](#schemalimitrule)|true|none|none|
|»»»» description|string|false|none|none|
|»»»» limit|number|true|none|none|
|»»»» metric|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» maxBytesReadPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxConcurrentAdhocSearchesPerUser|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»»» description|string|false|none|none|
|»»»» limit|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» metric|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» maxConcurrentScheduledSearchesPerUser|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»» maxConcurrentSearches|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|»»» maxExecutorsPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxRelativeEarliestTimerange|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxResultsPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxRunningTimePerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|»»» maxTimerangeWidth|[LimitRule](#schemalimitrule)|true|none|none|
|»» users|object|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» email|string|true|none|none|
|»»»» id|string|true|none|none|
|»» usersCount|number|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|maxRelativeEarliestTimerange|
|type|maxTimerangeWidth|
|type|maxBytesReadPerSearch|
|type|maxRunningTimePerSearch|
|type|maxResultsPerSearch|
|type|maxExecutorsPerSearch|
|type|coordinatorHeapMemoryLimit|
|type|maxConcurrentAdhocSearchesPerUser|
|type|maxConcurrentScheduledSearchesPerUser|
|type|maxConcurrentSearches|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_UsageGroup">UsageGroup</h2>
<!-- backwards compatibility -->
<a id="schemausagegroup"></a>
<a id="schema_UsageGroup"></a>
<a id="tocSusagegroup"></a>
<a id="tocsusagegroup"></a>

```json
{
  "coordinatorHeapMemoryLimit": 0,
  "description": "string",
  "enabled": true,
  "id": "string",
  "rules": {
    "coordinatorHeapMemoryLimit": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxBytesReadPerSearch": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxConcurrentAdhocSearchesPerUser": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxConcurrentAdhocSearchesPerUser"
    },
    "maxConcurrentScheduledSearchesPerUser": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxConcurrentAdhocSearchesPerUser"
    },
    "maxConcurrentSearches": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxConcurrentAdhocSearchesPerUser"
    },
    "maxExecutorsPerSearch": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxRelativeEarliestTimerange": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxResultsPerSearch": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxRunningTimePerSearch": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    },
    "maxTimerangeWidth": {
      "description": "string",
      "limit": 0,
      "metric": "string",
      "type": "maxRelativeEarliestTimerange"
    }
  },
  "users": {
    "property1": {
      "email": "string",
      "id": "string"
    },
    "property2": {
      "email": "string",
      "id": "string"
    }
  },
  "usersCount": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|coordinatorHeapMemoryLimit|number|false|none|none|
|description|string|false|none|none|
|enabled|boolean|false|none|none|
|id|string|true|none|none|
|rules|[LimitRuleDefinitions](#schemalimitruledefinitions)|true|none|none|
|users|object|false|none|none|
|» **additionalProperties**|object|false|none|none|
|»» email|string|true|none|none|
|»» id|string|true|none|none|
|usersCount|number|false|none|none|

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

<h2 id="tocS_LimitRuleDefinitions">LimitRuleDefinitions</h2>
<!-- backwards compatibility -->
<a id="schemalimitruledefinitions"></a>
<a id="schema_LimitRuleDefinitions"></a>
<a id="tocSlimitruledefinitions"></a>
<a id="tocslimitruledefinitions"></a>

```json
{
  "coordinatorHeapMemoryLimit": {
    "description": "string",
    "limit": 0,
    "metric": "string",
    "type": "maxRelativeEarliestTimerange"
  },
  "maxBytesReadPerSearch": {
    "description": "string",
    "limit": 0,
    "metric": "string",
    "type": "maxRelativeEarliestTimerange"
  },
  "maxConcurrentAdhocSearchesPerUser": {
    "description": "string",
    "limit": 0,
    "metric": "string",
    "type": "maxConcurrentAdhocSearchesPerUser"
  },
  "maxConcurrentScheduledSearchesPerUser": {
    "description": "string",
    "limit": 0,
    "metric": "string",
    "type": "maxConcurrentAdhocSearchesPerUser"
  },
  "maxConcurrentSearches": {
    "description": "string",
    "limit": 0,
    "metric": "string",
    "type": "maxConcurrentAdhocSearchesPerUser"
  },
  "maxExecutorsPerSearch": {
    "description": "string",
    "limit": 0,
    "metric": "string",
    "type": "maxRelativeEarliestTimerange"
  },
  "maxRelativeEarliestTimerange": {
    "description": "string",
    "limit": 0,
    "metric": "string",
    "type": "maxRelativeEarliestTimerange"
  },
  "maxResultsPerSearch": {
    "description": "string",
    "limit": 0,
    "metric": "string",
    "type": "maxRelativeEarliestTimerange"
  },
  "maxRunningTimePerSearch": {
    "description": "string",
    "limit": 0,
    "metric": "string",
    "type": "maxRelativeEarliestTimerange"
  },
  "maxTimerangeWidth": {
    "description": "string",
    "limit": 0,
    "metric": "string",
    "type": "maxRelativeEarliestTimerange"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|coordinatorHeapMemoryLimit|[LimitRule](#schemalimitrule)|true|none|none|
|maxBytesReadPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|maxConcurrentAdhocSearchesPerUser|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|maxConcurrentScheduledSearchesPerUser|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|maxConcurrentSearches|[SchedulingLimitRule](#schemaschedulinglimitrule)|true|none|none|
|maxExecutorsPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|maxRelativeEarliestTimerange|[LimitRule](#schemalimitrule)|true|none|none|
|maxResultsPerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|maxRunningTimePerSearch|[LimitRule](#schemalimitrule)|true|none|none|
|maxTimerangeWidth|[LimitRule](#schemalimitrule)|true|none|none|

<h2 id="tocS_LimitRule">LimitRule</h2>
<!-- backwards compatibility -->
<a id="schemalimitrule"></a>
<a id="schema_LimitRule"></a>
<a id="tocSlimitrule"></a>
<a id="tocslimitrule"></a>

```json
{
  "description": "string",
  "limit": 0,
  "metric": "string",
  "type": "maxRelativeEarliestTimerange"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|description|string|false|none|none|
|limit|number|true|none|none|
|metric|string|false|none|none|
|type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|maxRelativeEarliestTimerange|
|type|maxTimerangeWidth|
|type|maxBytesReadPerSearch|
|type|maxRunningTimePerSearch|
|type|maxResultsPerSearch|
|type|maxExecutorsPerSearch|
|type|coordinatorHeapMemoryLimit|

<h2 id="tocS_SchedulingLimitRule">SchedulingLimitRule</h2>
<!-- backwards compatibility -->
<a id="schemaschedulinglimitrule"></a>
<a id="schema_SchedulingLimitRule"></a>
<a id="tocSschedulinglimitrule"></a>
<a id="tocsschedulinglimitrule"></a>

```json
{
  "description": "string",
  "limit": 0,
  "metric": "string",
  "type": "maxConcurrentAdhocSearchesPerUser"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|description|string|false|none|none|
|limit|[NumberOrPercent](#schemanumberorpercent)|true|none|none|
|metric|string|false|none|none|
|type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|maxConcurrentAdhocSearchesPerUser|
|type|maxConcurrentScheduledSearchesPerUser|
|type|maxConcurrentSearches|

<h2 id="tocS_NumberOrPercent">NumberOrPercent</h2>
<!-- backwards compatibility -->
<a id="schemanumberorpercent"></a>
<a id="schema_NumberOrPercent"></a>
<a id="tocSnumberorpercent"></a>
<a id="tocsnumberorpercent"></a>

```json
0

```

### Properties

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|number|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

