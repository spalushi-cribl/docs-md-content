
<h1 id="cribl-stream-api-worker-and-edge-nodes">Cribl Stream API - Worker and Edge Nodes v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-worker-and-edge-nodes-worker-and-edge-nodes">Worker and Edge Nodes</h1>

## Get a summary of the Distributed deployment

<a id="opIdgetSummary"></a>

> Code samples

`GET /master/summary`

Get a summary of the Distributed deployment. The response includes counts of Worker Groups, Edge Fleets, Pipelines, Routes, Sources, Destinations, and Worker and Edge Nodes, as well as statistics for the Worker and Edge Nodes.

<h3 id="get-a-summary-of-the-distributed-deployment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|mode|query|[WorkerTypes](#schemaworkertypes)|false|Filter for limiting the response by Cribl product: Cribl Stream (<code>worker</code>) or Cribl Edge (<code>managed-edge</code>).|

#### Enumerated Values

|Parameter|Value|
|---|---|
|mode|worker|
|mode|managed-edge|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "groups": {
        "count": 0,
        "destinations": 0,
        "packs": 0,
        "pipelines": 0,
        "quickConnects": 0,
        "routes": 0,
        "sources": 0
      },
      "workers": {
        "alive": 0,
        "confVersions": 0,
        "count": 0,
        "disconnectedCount": 0,
        "groups": 0,
        "softwareVersions": 0,
        "unhealthy": 0
      }
    }
  ]
}
```

<h3 id="get-a-summary-of-the-distributed-deployment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DistributedSummary objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-summary-of-the-distributed-deployment-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DistributedSummary](#schemadistributedsummary)]|false|none|none|
|»» groups|object|true|none|none|
|»»» count|number|true|none|none|
|»»» destinations|number|true|none|none|
|»»» packs|number|true|none|none|
|»»» pipelines|number|true|none|none|
|»»» quickConnects|number|true|none|none|
|»»» routes|number|true|none|none|
|»»» sources|number|true|none|none|
|»» workers|object|true|none|none|
|»»» alive|number|true|none|none|
|»»» confVersions|number|true|none|none|
|»»» count|number|true|none|none|
|»»» disconnectedCount|number|true|none|none|
|»»» groups|number|true|none|none|
|»»» softwareVersions|number|true|none|none|
|»»» unhealthy|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get a count of Worker and Edge Nodes

<a id="opIdgetMasterWorkerEntry"></a>

> Code samples

`GET /master/summary/workers`

Get a count of all Worker and Edge Nodes.

<h3 id="get-a-count-of-worker-and-edge-nodes-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|filterExp|query|string|false|Filter expression to evaluate against Nodes for inclusion in the response.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    0
  ]
}
```

<h3 id="get-a-count-of-worker-and-edge-nodes-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of number objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-count-of-worker-and-edge-nodes-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[number]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get detailed metadata for Worker and Edge Nodes

<a id="opIdlistMasterWorkerEntry"></a>

> Code samples

`GET /master/workers`

Get detailed metadata for Worker and Edge Nodes.

<h3 id="get-detailed-metadata-for-worker-and-edge-nodes-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|filterExp|query|string|false|Filter expression to evaluate against Nodes for inclusion in the response.|
|sortExp|query|string|false|Sorting expression to evaluate against Nodes to specify the sort order for the response.|
|filter|query|string|false|JSON-stringified filter object to evaluate against Nodes for inclusion in the response.|
|sort|query|string|false|JSON-stringified sorting object to evaluate against Nodes to specify the sort order for the response.|
|limit|query|integer|false|Maximum number of Nodes to return in the response for this request. Use with <code>offset</code> to paginate the response into manageable batches.|
|offset|query|integer|false|Starting point from which to retrieve results for this request. Use with <code>limit</code> to paginate the response into manageable batches.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "deployable": true,
      "disconnected": true,
      "firstMsgTime": 0,
      "group": "string",
      "id": "string",
      "info": {
        "architecture": "string",
        "aws": {
          "enabled": true,
          "instanceId": "string",
          "region": "string",
          "tags": {},
          "type": "string",
          "zone": "string"
        },
        "conn_ip": "string",
        "cpus": 0,
        "cribl": {
          "config": {
            "featuresRev": "string",
            "hbPeriodSeconds": 0,
            "logStreamEnv": "string",
            "policyRev": "string",
            "version": "string"
          },
          "deploymentId": "string",
          "disableSNIRouting": true,
          "distMode": "edge",
          "edgeNodes": 0,
          "group": "string",
          "guid": "string",
          "installType": "string",
          "lookupVersions": {},
          "master": {
            "host": "string",
            "port": 0,
            "servername": "string",
            "tls": true
          },
          "pid": 0,
          "socksEnabled": true,
          "startTime": 0,
          "tags": [
            "string"
          ],
          "version": "string"
        },
        "env": {
          "property1": "string",
          "property2": "string"
        },
        "freeDiskSpace": 0,
        "hostOs": {
          "addresses": [
            "string"
          ],
          "enabled": true,
          "id": "string",
          "version": "string"
        },
        "hostname": "string",
        "isSaasWorker": true,
        "kube": {
          "enabled": true,
          "namespace": "string",
          "node": "string",
          "owner": {
            "kind": "string",
            "name": "string"
          },
          "pod": "string",
          "source": "string"
        },
        "localTime": 0,
        "metadata": {
          "aws": {
            "enabled": true,
            "instanceId": "string",
            "region": "string",
            "tags": {},
            "type": "string",
            "zone": "string"
          },
          "hostOs": {
            "addresses": [
              "string"
            ],
            "enabled": true,
            "id": "string",
            "version": "string"
          },
          "kube": {
            "enabled": true,
            "namespace": "string",
            "node": "string",
            "owner": {
              "kind": "string",
              "name": "string"
            },
            "pod": "string",
            "source": "string"
          },
          "os": {
            "addresses": [
              "string"
            ],
            "enabled": true,
            "id": "string",
            "version": "string"
          }
        },
        "node": "string",
        "os": {
          "addresses": [
            "string"
          ],
          "enabled": true,
          "id": "string",
          "version": "string"
        },
        "outpost": {
          "guid": "string",
          "host": "string"
        },
        "platform": "string",
        "release": "string",
        "totalDiskSpace": 0,
        "totalmem": 0
      },
      "lastMetrics": {},
      "lastMsgTime": 0,
      "metadata": {
        "aws": {
          "enabled": true,
          "instanceId": "string",
          "region": "string",
          "tags": {},
          "type": "string",
          "zone": "string"
        },
        "hostOs": {
          "addresses": [
            "string"
          ],
          "enabled": true,
          "id": "string",
          "version": "string"
        },
        "kube": {
          "enabled": true,
          "namespace": "string",
          "node": "string",
          "owner": {
            "kind": "string",
            "name": "string"
          },
          "pod": "string",
          "source": "string"
        },
        "os": {
          "addresses": [
            "string"
          ],
          "enabled": true,
          "id": "string",
          "version": "string"
        }
      },
      "nodeUpgradeStatus": {
        "active": 0,
        "failed": 0,
        "skipped": 0,
        "state": 0,
        "timestamp": 0
      },
      "status": "string",
      "type": "info",
      "workerProcesses": 0,
      "workers": {
        "count": 0
      }
    }
  ]
}
```

<h3 id="get-detailed-metadata-for-worker-and-edge-nodes-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of MasterWorkerEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-detailed-metadata-for-worker-and-edge-nodes-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[MasterWorkerEntry](#schemamasterworkerentry)]|false|none|none|
|»» deployable|boolean|false|none|none|
|»» disconnected|boolean|false|none|none|
|»» firstMsgTime|number|true|none|none|
|»» group|string|true|none|none|
|»» id|string|true|none|none|
|»» info|[NodeProvidedInfo](#schemanodeprovidedinfo)|true|none|none|
|»»» architecture|string|true|none|none|
|»»» aws|object|false|none|none|
|»»»» enabled|boolean|true|none|none|
|»»»» instanceId|string|true|none|none|
|»»»» region|string|true|none|none|
|»»»» tags|object|false|none|none|
|»»»» type|string|true|none|none|
|»»»» zone|string|true|none|none|
|»»» conn_ip|string|false|none|none|
|»»» cpus|number|true|none|none|
|»»» cribl|[HBCriblInfo](#schemahbcriblinfo)|true|none|none|
|»»»» config|object|true|none|none|
|»»»»» featuresRev|string|false|none|none|
|»»»»» hbPeriodSeconds|number|false|none|none|
|»»»»» logStreamEnv|string|false|none|none|
|»»»»» policyRev|string|false|none|none|
|»»»»» version|string|false|none|none|
|»»»» deploymentId|string|false|none|none|
|»»»» disableSNIRouting|boolean|false|none|none|
|»»»» distMode|string|true|none|none|
|»»»» edgeNodes|number|false|none|none|
|»»»» group|string|true|none|none|
|»»»» guid|string|true|none|none|
|»»»» installType|string|false|none|none|
|»»»» lookupVersions|[LookupVersions](#schemalookupversions)|false|none|none|
|»»»» master|[HBLeaderInfo](#schemahbleaderinfo)|false|none|none|
|»»»»» host|string|true|none|none|
|»»»»» port|number|true|none|none|
|»»»»» servername|string|false|none|none|
|»»»»» tls|boolean|false|none|none|
|»»»» pid|number|false|none|none|
|»»»» socksEnabled|boolean|false|none|none|
|»»»» startTime|number|true|none|none|
|»»»» tags|[string]|true|none|none|
|»»»» version|string|false|none|none|
|»»» env|object|true|none|none|
|»»»» **additionalProperties**|string|false|none|none|
|»»» freeDiskSpace|number|true|none|none|
|»»» hostOs|object|false|none|none|
|»»»» addresses|[string]|true|none|none|
|»»»» enabled|boolean|true|none|none|
|»»»» id|string|true|none|none|
|»»»» version|string|true|none|none|
|»»» hostname|string|true|none|none|
|»»» isSaasWorker|boolean|false|none|none|
|»»» kube|object|false|none|none|
|»»»» enabled|boolean|true|none|none|
|»»»» namespace|string|true|none|none|
|»»»» node|string|true|none|none|
|»»»» owner|object|false|none|none|
|»»»»» kind|string|true|none|none|
|»»»»» name|string|true|none|none|
|»»»» pod|string|true|none|none|
|»»»» source|string|true|none|none|
|»»» localTime|number|false|none|none|
|»»» metadata|[HeartbeatMetadata](#schemaheartbeatmetadata)|false|none|none|
|»»»» aws|object|false|none|none|
|»»»»» enabled|boolean|true|none|none|
|»»»»» instanceId|string|true|none|none|
|»»»»» region|string|true|none|none|
|»»»»» tags|object|false|none|none|
|»»»»» type|string|true|none|none|
|»»»»» zone|string|true|none|none|
|»»»» hostOs|object|false|none|none|
|»»»»» addresses|[string]|true|none|none|
|»»»»» enabled|boolean|true|none|none|
|»»»»» id|string|true|none|none|
|»»»»» version|string|true|none|none|
|»»»» kube|object|false|none|none|
|»»»»» enabled|boolean|true|none|none|
|»»»»» namespace|string|true|none|none|
|»»»»» node|string|true|none|none|
|»»»»» owner|object|false|none|none|
|»»»»»» kind|string|true|none|none|
|»»»»»» name|string|true|none|none|
|»»»»» pod|string|true|none|none|
|»»»»» source|string|true|none|none|
|»»»» os|object|false|none|none|
|»»»»» addresses|[string]|true|none|none|
|»»»»» enabled|boolean|true|none|none|
|»»»»» id|string|true|none|none|
|»»»»» version|string|true|none|none|
|»»» node|string|true|none|none|
|»»» os|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» addresses|[string]|true|none|none|
|»»»»» enabled|boolean|true|none|none|
|»»»»» id|string|true|none|none|
|»»»»» version|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» addresses|[string]|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» outpost|[OutpostNodeInfo](#schemaoutpostnodeinfo)|false|none|none|
|»»»» guid|string|true|none|none|
|»»»» host|string|true|none|none|
|»»» platform|string|true|none|none|
|»»» release|string|true|none|none|
|»»» totalDiskSpace|number|true|none|none|
|»»» totalmem|number|true|none|none|
|»» lastMetrics|object|false|none|none|
|»» lastMsgTime|number|true|none|none|
|»» metadata|[HeartbeatMetadata](#schemaheartbeatmetadata)|false|none|none|
|»» nodeUpgradeStatus|[NodeUpgradeStatus](#schemanodeupgradestatus)|false|none|none|
|»»» active|[NodeActiveUpgradeStatus](#schemanodeactiveupgradestatus)|false|none|none|
|»»» failed|[NodeFailedUpgradeStatus](#schemanodefailedupgradestatus)|false|none|none|
|»»» skipped|[NodeSkippedUpgradeStatus](#schemanodeskippedupgradestatus)|false|none|none|
|»»» state|[NodeUpgradeState](#schemanodeupgradestate)|true|none|none|
|»»» timestamp|number|true|none|none|
|»» status|string|false|none|none|
|»» type|string|false|none|none|
|»» workerProcesses|number|true|none|none|
|»» workers|object|false|none|none|
|»»» count|number|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|distMode|edge|
|distMode|worker|
|distMode|single|
|distMode|master|
|distMode|managed-edge|
|distMode|outpost|
|distMode|search-supervisor|
|active|0|
|active|1|
|active|2|
|failed|0|
|failed|1|
|skipped|0|
|skipped|1|
|skipped|2|
|skipped|3|
|state|0|
|state|1|
|state|2|
|state|3|
|type|info|
|type|req|
|type|resp|

<aside class="success">
This operation does not require authentication
</aside>

## Restart Worker and Edge Nodes

<a id="opIdupdateMasterWorkerEntry"></a>

> Code samples

`PATCH /master/workers/restart`

restarts worker nodes

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "message": "string",
      "status": "Restarting"
    }
  ]
}
```

<h3 id="restart-worker-and-edge-nodes-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of RestartResponse objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="restart-worker-and-edge-nodes-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[RestartResponse](#schemarestartresponse)]|false|none|none|
|»» id|string|true|none|none|
|»» message|string|false|none|none|
|»» status|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|Restarting|
|status|Error|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_DistributedSummary">DistributedSummary</h2>
<!-- backwards compatibility -->
<a id="schemadistributedsummary"></a>
<a id="schema_DistributedSummary"></a>
<a id="tocSdistributedsummary"></a>
<a id="tocsdistributedsummary"></a>

```json
{
  "groups": {
    "count": 0,
    "destinations": 0,
    "packs": 0,
    "pipelines": 0,
    "quickConnects": 0,
    "routes": 0,
    "sources": 0
  },
  "workers": {
    "alive": 0,
    "confVersions": 0,
    "count": 0,
    "disconnectedCount": 0,
    "groups": 0,
    "softwareVersions": 0,
    "unhealthy": 0
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|groups|object|true|none|none|
|» count|number|true|none|none|
|» destinations|number|true|none|none|
|» packs|number|true|none|none|
|» pipelines|number|true|none|none|
|» quickConnects|number|true|none|none|
|» routes|number|true|none|none|
|» sources|number|true|none|none|
|workers|object|true|none|none|
|» alive|number|true|none|none|
|» confVersions|number|true|none|none|
|» count|number|true|none|none|
|» disconnectedCount|number|true|none|none|
|» groups|number|true|none|none|
|» softwareVersions|number|true|none|none|
|» unhealthy|number|true|none|none|

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

<h2 id="tocS_WorkerTypes">WorkerTypes</h2>
<!-- backwards compatibility -->
<a id="schemaworkertypes"></a>
<a id="schema_WorkerTypes"></a>
<a id="tocSworkertypes"></a>
<a id="tocsworkertypes"></a>

```json
"worker"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|worker|
|*anonymous*|managed-edge|

<h2 id="tocS_MasterWorkerEntry">MasterWorkerEntry</h2>
<!-- backwards compatibility -->
<a id="schemamasterworkerentry"></a>
<a id="schema_MasterWorkerEntry"></a>
<a id="tocSmasterworkerentry"></a>
<a id="tocsmasterworkerentry"></a>

```json
{
  "deployable": true,
  "disconnected": true,
  "firstMsgTime": 0,
  "group": "string",
  "id": "string",
  "info": {
    "architecture": "string",
    "aws": {
      "enabled": true,
      "instanceId": "string",
      "region": "string",
      "tags": {},
      "type": "string",
      "zone": "string"
    },
    "conn_ip": "string",
    "cpus": 0,
    "cribl": {
      "config": {
        "featuresRev": "string",
        "hbPeriodSeconds": 0,
        "logStreamEnv": "string",
        "policyRev": "string",
        "version": "string"
      },
      "deploymentId": "string",
      "disableSNIRouting": true,
      "distMode": "edge",
      "edgeNodes": 0,
      "group": "string",
      "guid": "string",
      "installType": "string",
      "lookupVersions": {},
      "master": {
        "host": "string",
        "port": 0,
        "servername": "string",
        "tls": true
      },
      "pid": 0,
      "socksEnabled": true,
      "startTime": 0,
      "tags": [
        "string"
      ],
      "version": "string"
    },
    "env": {
      "property1": "string",
      "property2": "string"
    },
    "freeDiskSpace": 0,
    "hostOs": {
      "addresses": [
        "string"
      ],
      "enabled": true,
      "id": "string",
      "version": "string"
    },
    "hostname": "string",
    "isSaasWorker": true,
    "kube": {
      "enabled": true,
      "namespace": "string",
      "node": "string",
      "owner": {
        "kind": "string",
        "name": "string"
      },
      "pod": "string",
      "source": "string"
    },
    "localTime": 0,
    "metadata": {
      "aws": {
        "enabled": true,
        "instanceId": "string",
        "region": "string",
        "tags": {},
        "type": "string",
        "zone": "string"
      },
      "hostOs": {
        "addresses": [
          "string"
        ],
        "enabled": true,
        "id": "string",
        "version": "string"
      },
      "kube": {
        "enabled": true,
        "namespace": "string",
        "node": "string",
        "owner": {
          "kind": "string",
          "name": "string"
        },
        "pod": "string",
        "source": "string"
      },
      "os": {
        "addresses": [
          "string"
        ],
        "enabled": true,
        "id": "string",
        "version": "string"
      }
    },
    "node": "string",
    "os": {
      "addresses": [
        "string"
      ],
      "enabled": true,
      "id": "string",
      "version": "string"
    },
    "outpost": {
      "guid": "string",
      "host": "string"
    },
    "platform": "string",
    "release": "string",
    "totalDiskSpace": 0,
    "totalmem": 0
  },
  "lastMetrics": {},
  "lastMsgTime": 0,
  "metadata": {
    "aws": {
      "enabled": true,
      "instanceId": "string",
      "region": "string",
      "tags": {},
      "type": "string",
      "zone": "string"
    },
    "hostOs": {
      "addresses": [
        "string"
      ],
      "enabled": true,
      "id": "string",
      "version": "string"
    },
    "kube": {
      "enabled": true,
      "namespace": "string",
      "node": "string",
      "owner": {
        "kind": "string",
        "name": "string"
      },
      "pod": "string",
      "source": "string"
    },
    "os": {
      "addresses": [
        "string"
      ],
      "enabled": true,
      "id": "string",
      "version": "string"
    }
  },
  "nodeUpgradeStatus": {
    "active": 0,
    "failed": 0,
    "skipped": 0,
    "state": 0,
    "timestamp": 0
  },
  "status": "string",
  "type": "info",
  "workerProcesses": 0,
  "workers": {
    "count": 0
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|deployable|boolean|false|none|none|
|disconnected|boolean|false|none|none|
|firstMsgTime|number|true|none|none|
|group|string|true|none|none|
|id|string|true|none|none|
|info|[NodeProvidedInfo](#schemanodeprovidedinfo)|true|none|none|
|lastMetrics|object|false|none|none|
|lastMsgTime|number|true|none|none|
|metadata|[HeartbeatMetadata](#schemaheartbeatmetadata)|false|none|none|
|nodeUpgradeStatus|[NodeUpgradeStatus](#schemanodeupgradestatus)|false|none|none|
|status|string|false|none|none|
|type|string|false|none|none|
|workerProcesses|number|true|none|none|
|workers|object|false|none|none|
|» count|number|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|info|
|type|req|
|type|resp|

<h2 id="tocS_RestartResponse">RestartResponse</h2>
<!-- backwards compatibility -->
<a id="schemarestartresponse"></a>
<a id="schema_RestartResponse"></a>
<a id="tocSrestartresponse"></a>
<a id="tocsrestartresponse"></a>

```json
{
  "id": "string",
  "message": "string",
  "status": "Restarting"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|message|string|false|none|none|
|status|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|Restarting|
|status|Error|

<h2 id="tocS_NodeProvidedInfo">NodeProvidedInfo</h2>
<!-- backwards compatibility -->
<a id="schemanodeprovidedinfo"></a>
<a id="schema_NodeProvidedInfo"></a>
<a id="tocSnodeprovidedinfo"></a>
<a id="tocsnodeprovidedinfo"></a>

```json
{
  "architecture": "string",
  "aws": {
    "enabled": true,
    "instanceId": "string",
    "region": "string",
    "tags": {},
    "type": "string",
    "zone": "string"
  },
  "conn_ip": "string",
  "cpus": 0,
  "cribl": {
    "config": {
      "featuresRev": "string",
      "hbPeriodSeconds": 0,
      "logStreamEnv": "string",
      "policyRev": "string",
      "version": "string"
    },
    "deploymentId": "string",
    "disableSNIRouting": true,
    "distMode": "edge",
    "edgeNodes": 0,
    "group": "string",
    "guid": "string",
    "installType": "string",
    "lookupVersions": {},
    "master": {
      "host": "string",
      "port": 0,
      "servername": "string",
      "tls": true
    },
    "pid": 0,
    "socksEnabled": true,
    "startTime": 0,
    "tags": [
      "string"
    ],
    "version": "string"
  },
  "env": {
    "property1": "string",
    "property2": "string"
  },
  "freeDiskSpace": 0,
  "hostOs": {
    "addresses": [
      "string"
    ],
    "enabled": true,
    "id": "string",
    "version": "string"
  },
  "hostname": "string",
  "isSaasWorker": true,
  "kube": {
    "enabled": true,
    "namespace": "string",
    "node": "string",
    "owner": {
      "kind": "string",
      "name": "string"
    },
    "pod": "string",
    "source": "string"
  },
  "localTime": 0,
  "metadata": {
    "aws": {
      "enabled": true,
      "instanceId": "string",
      "region": "string",
      "tags": {},
      "type": "string",
      "zone": "string"
    },
    "hostOs": {
      "addresses": [
        "string"
      ],
      "enabled": true,
      "id": "string",
      "version": "string"
    },
    "kube": {
      "enabled": true,
      "namespace": "string",
      "node": "string",
      "owner": {
        "kind": "string",
        "name": "string"
      },
      "pod": "string",
      "source": "string"
    },
    "os": {
      "addresses": [
        "string"
      ],
      "enabled": true,
      "id": "string",
      "version": "string"
    }
  },
  "node": "string",
  "os": {
    "addresses": [
      "string"
    ],
    "enabled": true,
    "id": "string",
    "version": "string"
  },
  "outpost": {
    "guid": "string",
    "host": "string"
  },
  "platform": "string",
  "release": "string",
  "totalDiskSpace": 0,
  "totalmem": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|architecture|string|true|none|none|
|aws|object|false|none|none|
|» enabled|boolean|true|none|none|
|» instanceId|string|true|none|none|
|» region|string|true|none|none|
|» tags|object|false|none|none|
|» type|string|true|none|none|
|» zone|string|true|none|none|
|conn_ip|string|false|none|none|
|cpus|number|true|none|none|
|cribl|[HBCriblInfo](#schemahbcriblinfo)|true|none|none|
|env|object|true|none|none|
|» **additionalProperties**|string|false|none|none|
|freeDiskSpace|number|true|none|none|
|hostOs|object|false|none|none|
|» addresses|[string]|true|none|none|
|» enabled|boolean|true|none|none|
|» id|string|true|none|none|
|» version|string|true|none|none|
|hostname|string|true|none|none|
|isSaasWorker|boolean|false|none|none|
|kube|object|false|none|none|
|» enabled|boolean|true|none|none|
|» namespace|string|true|none|none|
|» node|string|true|none|none|
|» owner|object|false|none|none|
|»» kind|string|true|none|none|
|»» name|string|true|none|none|
|» pod|string|true|none|none|
|» source|string|true|none|none|
|localTime|number|false|none|none|
|metadata|[HeartbeatMetadata](#schemaheartbeatmetadata)|false|none|none|
|node|string|true|none|none|
|os|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» addresses|[string]|true|none|none|
|»» enabled|boolean|true|none|none|
|»» id|string|true|none|none|
|»» version|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» addresses|[string]|true|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|outpost|[OutpostNodeInfo](#schemaoutpostnodeinfo)|false|none|none|
|platform|string|true|none|none|
|release|string|true|none|none|
|totalDiskSpace|number|true|none|none|
|totalmem|number|true|none|none|

<h2 id="tocS_HeartbeatMetadata">HeartbeatMetadata</h2>
<!-- backwards compatibility -->
<a id="schemaheartbeatmetadata"></a>
<a id="schema_HeartbeatMetadata"></a>
<a id="tocSheartbeatmetadata"></a>
<a id="tocsheartbeatmetadata"></a>

```json
{
  "aws": {
    "enabled": true,
    "instanceId": "string",
    "region": "string",
    "tags": {},
    "type": "string",
    "zone": "string"
  },
  "hostOs": {
    "addresses": [
      "string"
    ],
    "enabled": true,
    "id": "string",
    "version": "string"
  },
  "kube": {
    "enabled": true,
    "namespace": "string",
    "node": "string",
    "owner": {
      "kind": "string",
      "name": "string"
    },
    "pod": "string",
    "source": "string"
  },
  "os": {
    "addresses": [
      "string"
    ],
    "enabled": true,
    "id": "string",
    "version": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|aws|object|false|none|none|
|» enabled|boolean|true|none|none|
|» instanceId|string|true|none|none|
|» region|string|true|none|none|
|» tags|object|false|none|none|
|» type|string|true|none|none|
|» zone|string|true|none|none|
|hostOs|object|false|none|none|
|» addresses|[string]|true|none|none|
|» enabled|boolean|true|none|none|
|» id|string|true|none|none|
|» version|string|true|none|none|
|kube|object|false|none|none|
|» enabled|boolean|true|none|none|
|» namespace|string|true|none|none|
|» node|string|true|none|none|
|» owner|object|false|none|none|
|»» kind|string|true|none|none|
|»» name|string|true|none|none|
|» pod|string|true|none|none|
|» source|string|true|none|none|
|os|object|false|none|none|
|» addresses|[string]|true|none|none|
|» enabled|boolean|true|none|none|
|» id|string|true|none|none|
|» version|string|true|none|none|

<h2 id="tocS_NodeUpgradeStatus">NodeUpgradeStatus</h2>
<!-- backwards compatibility -->
<a id="schemanodeupgradestatus"></a>
<a id="schema_NodeUpgradeStatus"></a>
<a id="tocSnodeupgradestatus"></a>
<a id="tocsnodeupgradestatus"></a>

```json
{
  "active": 0,
  "failed": 0,
  "skipped": 0,
  "state": 0,
  "timestamp": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|active|[NodeActiveUpgradeStatus](#schemanodeactiveupgradestatus)|false|none|none|
|failed|[NodeFailedUpgradeStatus](#schemanodefailedupgradestatus)|false|none|none|
|skipped|[NodeSkippedUpgradeStatus](#schemanodeskippedupgradestatus)|false|none|none|
|state|[NodeUpgradeState](#schemanodeupgradestate)|true|none|none|
|timestamp|number|true|none|none|

<h2 id="tocS_HBCriblInfo">HBCriblInfo</h2>
<!-- backwards compatibility -->
<a id="schemahbcriblinfo"></a>
<a id="schema_HBCriblInfo"></a>
<a id="tocShbcriblinfo"></a>
<a id="tocshbcriblinfo"></a>

```json
{
  "config": {
    "featuresRev": "string",
    "hbPeriodSeconds": 0,
    "logStreamEnv": "string",
    "policyRev": "string",
    "version": "string"
  },
  "deploymentId": "string",
  "disableSNIRouting": true,
  "distMode": "edge",
  "edgeNodes": 0,
  "group": "string",
  "guid": "string",
  "installType": "string",
  "lookupVersions": {},
  "master": {
    "host": "string",
    "port": 0,
    "servername": "string",
    "tls": true
  },
  "pid": 0,
  "socksEnabled": true,
  "startTime": 0,
  "tags": [
    "string"
  ],
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|config|object|true|none|none|
|» featuresRev|string|false|none|none|
|» hbPeriodSeconds|number|false|none|none|
|» logStreamEnv|string|false|none|none|
|» policyRev|string|false|none|none|
|» version|string|false|none|none|
|deploymentId|string|false|none|none|
|disableSNIRouting|boolean|false|none|none|
|distMode|string|true|none|none|
|edgeNodes|number|false|none|none|
|group|string|true|none|none|
|guid|string|true|none|none|
|installType|string|false|none|none|
|lookupVersions|[LookupVersions](#schemalookupversions)|false|none|none|
|master|[HBLeaderInfo](#schemahbleaderinfo)|false|none|none|
|pid|number|false|none|none|
|socksEnabled|boolean|false|none|none|
|startTime|number|true|none|none|
|tags|[string]|true|none|none|
|version|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|distMode|edge|
|distMode|worker|
|distMode|single|
|distMode|master|
|distMode|managed-edge|
|distMode|outpost|
|distMode|search-supervisor|

<h2 id="tocS_OutpostNodeInfo">OutpostNodeInfo</h2>
<!-- backwards compatibility -->
<a id="schemaoutpostnodeinfo"></a>
<a id="schema_OutpostNodeInfo"></a>
<a id="tocSoutpostnodeinfo"></a>
<a id="tocsoutpostnodeinfo"></a>

```json
{
  "guid": "string",
  "host": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|guid|string|true|none|none|
|host|string|true|none|none|

<h2 id="tocS_NodeActiveUpgradeStatus">NodeActiveUpgradeStatus</h2>
<!-- backwards compatibility -->
<a id="schemanodeactiveupgradestatus"></a>
<a id="schema_NodeActiveUpgradeStatus"></a>
<a id="tocSnodeactiveupgradestatus"></a>
<a id="tocsnodeactiveupgradestatus"></a>

```json
0

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|integer|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|0|
|*anonymous*|1|
|*anonymous*|2|

<h2 id="tocS_NodeFailedUpgradeStatus">NodeFailedUpgradeStatus</h2>
<!-- backwards compatibility -->
<a id="schemanodefailedupgradestatus"></a>
<a id="schema_NodeFailedUpgradeStatus"></a>
<a id="tocSnodefailedupgradestatus"></a>
<a id="tocsnodefailedupgradestatus"></a>

```json
0

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|integer|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|0|
|*anonymous*|1|

<h2 id="tocS_NodeSkippedUpgradeStatus">NodeSkippedUpgradeStatus</h2>
<!-- backwards compatibility -->
<a id="schemanodeskippedupgradestatus"></a>
<a id="schema_NodeSkippedUpgradeStatus"></a>
<a id="tocSnodeskippedupgradestatus"></a>
<a id="tocsnodeskippedupgradestatus"></a>

```json
0

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|integer|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|0|
|*anonymous*|1|
|*anonymous*|2|
|*anonymous*|3|

<h2 id="tocS_NodeUpgradeState">NodeUpgradeState</h2>
<!-- backwards compatibility -->
<a id="schemanodeupgradestate"></a>
<a id="schema_NodeUpgradeState"></a>
<a id="tocSnodeupgradestate"></a>
<a id="tocsnodeupgradestate"></a>

```json
0

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|integer|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|0|
|*anonymous*|1|
|*anonymous*|2|
|*anonymous*|3|

<h2 id="tocS_LookupVersions">LookupVersions</h2>
<!-- backwards compatibility -->
<a id="schemalookupversions"></a>
<a id="schema_LookupVersions"></a>
<a id="tocSlookupversions"></a>
<a id="tocslookupversions"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocS_HBLeaderInfo">HBLeaderInfo</h2>
<!-- backwards compatibility -->
<a id="schemahbleaderinfo"></a>
<a id="schema_HBLeaderInfo"></a>
<a id="tocShbleaderinfo"></a>
<a id="tocshbleaderinfo"></a>

```json
{
  "host": "string",
  "port": 0,
  "servername": "string",
  "tls": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|host|string|true|none|none|
|port|number|true|none|none|
|servername|string|false|none|none|
|tls|boolean|false|none|none|

