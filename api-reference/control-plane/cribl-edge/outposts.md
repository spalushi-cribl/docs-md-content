
<h1 id="cribl-edge-api-outposts">Cribl Edge API - Outposts v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-outposts-outposts">Outposts</h1>

## Retrieve detailed metadata for Outpost Nodes

<a id="opIdlistOutposts"></a>

> Code samples

`GET /master/outposts`

get Outpost nodes

<h3 id="retrieve-detailed-metadata-for-outpost-nodes-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|filter|query|string|false|Filter object (JSON stringified) to select nodes|
|sort|query|string|false|Sorting object (JSON stringified) expression evaluated against nodes|
|limit|query|integer|false|Maximum number of nodes to return|
|offset|query|integer|false|Pagination offset|
|filterExp|query|string|false|Filter expression evaluated against nodes (deprecated, use filter instead)|
|sortExp|query|string|false|Sorting expression evaluated against nodes (deprecated, use sort instead)|

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

<h3 id="retrieve-detailed-metadata-for-outpost-nodes-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of MasterWorkerEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="retrieve-detailed-metadata-for-outpost-nodes-responseschema">Response Schema</h3>

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

## Create MasterWorkerEntry

<a id="opIdcreateOutposts"></a>

> Code samples

`POST /master/outposts`

Create MasterWorkerEntry

> Body parameter

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

<h3 id="create-masterworkerentry-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[MasterWorkerEntry](#schemamasterworkerentry)|true|New MasterWorkerEntry object|
|» deployable|body|boolean|false|none|
|» disconnected|body|boolean|false|none|
|» firstMsgTime|body|number|true|none|
|» group|body|string|true|none|
|» id|body|string|true|none|
|» info|body|[NodeProvidedInfo](#schemanodeprovidedinfo)|true|none|
|»» architecture|body|string|true|none|
|»» aws|body|object|false|none|
|»»» enabled|body|boolean|true|none|
|»»» instanceId|body|string|true|none|
|»»» region|body|string|true|none|
|»»» tags|body|object|false|none|
|»»» type|body|string|true|none|
|»»» zone|body|string|true|none|
|»» conn_ip|body|string|false|none|
|»» cpus|body|number|true|none|
|»» cribl|body|[HBCriblInfo](#schemahbcriblinfo)|true|none|
|»»» config|body|object|true|none|
|»»»» featuresRev|body|string|false|none|
|»»»» hbPeriodSeconds|body|number|false|none|
|»»»» logStreamEnv|body|string|false|none|
|»»»» policyRev|body|string|false|none|
|»»»» version|body|string|false|none|
|»»» deploymentId|body|string|false|none|
|»»» disableSNIRouting|body|boolean|false|none|
|»»» distMode|body|string|true|none|
|»»» edgeNodes|body|number|false|none|
|»»» group|body|string|true|none|
|»»» guid|body|string|true|none|
|»»» installType|body|string|false|none|
|»»» lookupVersions|body|[LookupVersions](#schemalookupversions)|false|none|
|»»» master|body|[HBLeaderInfo](#schemahbleaderinfo)|false|none|
|»»»» host|body|string|true|none|
|»»»» port|body|number|true|none|
|»»»» servername|body|string|false|none|
|»»»» tls|body|boolean|false|none|
|»»» pid|body|number|false|none|
|»»» socksEnabled|body|boolean|false|none|
|»»» startTime|body|number|true|none|
|»»» tags|body|[string]|true|none|
|»»» version|body|string|false|none|
|»» env|body|object|true|none|
|»»» **additionalProperties**|body|string|false|none|
|»» freeDiskSpace|body|number|true|none|
|»» hostOs|body|object|false|none|
|»»» addresses|body|[string]|true|none|
|»»» enabled|body|boolean|true|none|
|»»» id|body|string|true|none|
|»»» version|body|string|true|none|
|»» hostname|body|string|true|none|
|»» isSaasWorker|body|boolean|false|none|
|»» kube|body|object|false|none|
|»»» enabled|body|boolean|true|none|
|»»» namespace|body|string|true|none|
|»»» node|body|string|true|none|
|»»» owner|body|object|false|none|
|»»»» kind|body|string|true|none|
|»»»» name|body|string|true|none|
|»»» pod|body|string|true|none|
|»»» source|body|string|true|none|
|»» localTime|body|number|false|none|
|»» metadata|body|[HeartbeatMetadata](#schemaheartbeatmetadata)|false|none|
|»»» aws|body|object|false|none|
|»»»» enabled|body|boolean|true|none|
|»»»» instanceId|body|string|true|none|
|»»»» region|body|string|true|none|
|»»»» tags|body|object|false|none|
|»»»» type|body|string|true|none|
|»»»» zone|body|string|true|none|
|»»» hostOs|body|object|false|none|
|»»»» addresses|body|[string]|true|none|
|»»»» enabled|body|boolean|true|none|
|»»»» id|body|string|true|none|
|»»»» version|body|string|true|none|
|»»» kube|body|object|false|none|
|»»»» enabled|body|boolean|true|none|
|»»»» namespace|body|string|true|none|
|»»»» node|body|string|true|none|
|»»»» owner|body|object|false|none|
|»»»»» kind|body|string|true|none|
|»»»»» name|body|string|true|none|
|»»»» pod|body|string|true|none|
|»»»» source|body|string|true|none|
|»»» os|body|object|false|none|
|»»»» addresses|body|[string]|true|none|
|»»»» enabled|body|boolean|true|none|
|»»»» id|body|string|true|none|
|»»»» version|body|string|true|none|
|»» node|body|string|true|none|
|»» os|body|any|false|none|
|»»» *anonymous*|body|object|false|none|
|»»»» addresses|body|[string]|true|none|
|»»»» enabled|body|boolean|true|none|
|»»»» id|body|string|true|none|
|»»»» version|body|string|true|none|
|»»» *anonymous*|body|object|false|none|
|»»»» addresses|body|[string]|true|none|
|»» outpost|body|[OutpostNodeInfo](#schemaoutpostnodeinfo)|false|none|
|»»» guid|body|string|true|none|
|»»» host|body|string|true|none|
|»» platform|body|string|true|none|
|»» release|body|string|true|none|
|»» totalDiskSpace|body|number|true|none|
|»» totalmem|body|number|true|none|
|» lastMetrics|body|object|false|none|
|» lastMsgTime|body|number|true|none|
|» metadata|body|[HeartbeatMetadata](#schemaheartbeatmetadata)|false|none|
|» nodeUpgradeStatus|body|[NodeUpgradeStatus](#schemanodeupgradestatus)|false|none|
|»» active|body|[NodeActiveUpgradeStatus](#schemanodeactiveupgradestatus)|false|none|
|»» failed|body|[NodeFailedUpgradeStatus](#schemanodefailedupgradestatus)|false|none|
|»» skipped|body|[NodeSkippedUpgradeStatus](#schemanodeskippedupgradestatus)|false|none|
|»» state|body|[NodeUpgradeState](#schemanodeupgradestate)|true|none|
|»» timestamp|body|number|true|none|
|» status|body|string|false|none|
|» type|body|string|false|none|
|» workerProcesses|body|number|true|none|
|» workers|body|object|false|none|
|»» count|body|number|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» distMode|edge|
|»»» distMode|worker|
|»»» distMode|single|
|»»» distMode|master|
|»»» distMode|managed-edge|
|»»» distMode|outpost|
|»»» distMode|search-supervisor|
|»» active|0|
|»» active|1|
|»» active|2|
|»» failed|0|
|»» failed|1|
|»» skipped|0|
|»» skipped|1|
|»» skipped|2|
|»» skipped|3|
|»» state|0|
|»» state|1|
|»» state|2|
|»» state|3|
|» type|info|
|» type|req|
|» type|resp|

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

<h3 id="create-masterworkerentry-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of MasterWorkerEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-masterworkerentry-responseschema">Response Schema</h3>

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

## Get MasterWorkerEntry by ID

<a id="opIdgetOutpostsById"></a>

> Code samples

`GET /master/outposts/{id}`

Get MasterWorkerEntry by ID

<h3 id="get-masterworkerentry-by-id-parameters">Parameters</h3>

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

<h3 id="get-masterworkerentry-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of MasterWorkerEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-masterworkerentry-by-id-responseschema">Response Schema</h3>

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

## Update MasterWorkerEntry

<a id="opIdupdateOutpostsById"></a>

> Code samples

`PATCH /master/outposts/{id}`

Update MasterWorkerEntry

> Body parameter

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

<h3 id="update-masterworkerentry-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[MasterWorkerEntry](#schemamasterworkerentry)|true|MasterWorkerEntry object to be updated|
|» deployable|body|boolean|false|none|
|» disconnected|body|boolean|false|none|
|» firstMsgTime|body|number|true|none|
|» group|body|string|true|none|
|» id|body|string|true|none|
|» info|body|[NodeProvidedInfo](#schemanodeprovidedinfo)|true|none|
|»» architecture|body|string|true|none|
|»» aws|body|object|false|none|
|»»» enabled|body|boolean|true|none|
|»»» instanceId|body|string|true|none|
|»»» region|body|string|true|none|
|»»» tags|body|object|false|none|
|»»» type|body|string|true|none|
|»»» zone|body|string|true|none|
|»» conn_ip|body|string|false|none|
|»» cpus|body|number|true|none|
|»» cribl|body|[HBCriblInfo](#schemahbcriblinfo)|true|none|
|»»» config|body|object|true|none|
|»»»» featuresRev|body|string|false|none|
|»»»» hbPeriodSeconds|body|number|false|none|
|»»»» logStreamEnv|body|string|false|none|
|»»»» policyRev|body|string|false|none|
|»»»» version|body|string|false|none|
|»»» deploymentId|body|string|false|none|
|»»» disableSNIRouting|body|boolean|false|none|
|»»» distMode|body|string|true|none|
|»»» edgeNodes|body|number|false|none|
|»»» group|body|string|true|none|
|»»» guid|body|string|true|none|
|»»» installType|body|string|false|none|
|»»» lookupVersions|body|[LookupVersions](#schemalookupversions)|false|none|
|»»» master|body|[HBLeaderInfo](#schemahbleaderinfo)|false|none|
|»»»» host|body|string|true|none|
|»»»» port|body|number|true|none|
|»»»» servername|body|string|false|none|
|»»»» tls|body|boolean|false|none|
|»»» pid|body|number|false|none|
|»»» socksEnabled|body|boolean|false|none|
|»»» startTime|body|number|true|none|
|»»» tags|body|[string]|true|none|
|»»» version|body|string|false|none|
|»» env|body|object|true|none|
|»»» **additionalProperties**|body|string|false|none|
|»» freeDiskSpace|body|number|true|none|
|»» hostOs|body|object|false|none|
|»»» addresses|body|[string]|true|none|
|»»» enabled|body|boolean|true|none|
|»»» id|body|string|true|none|
|»»» version|body|string|true|none|
|»» hostname|body|string|true|none|
|»» isSaasWorker|body|boolean|false|none|
|»» kube|body|object|false|none|
|»»» enabled|body|boolean|true|none|
|»»» namespace|body|string|true|none|
|»»» node|body|string|true|none|
|»»» owner|body|object|false|none|
|»»»» kind|body|string|true|none|
|»»»» name|body|string|true|none|
|»»» pod|body|string|true|none|
|»»» source|body|string|true|none|
|»» localTime|body|number|false|none|
|»» metadata|body|[HeartbeatMetadata](#schemaheartbeatmetadata)|false|none|
|»»» aws|body|object|false|none|
|»»»» enabled|body|boolean|true|none|
|»»»» instanceId|body|string|true|none|
|»»»» region|body|string|true|none|
|»»»» tags|body|object|false|none|
|»»»» type|body|string|true|none|
|»»»» zone|body|string|true|none|
|»»» hostOs|body|object|false|none|
|»»»» addresses|body|[string]|true|none|
|»»»» enabled|body|boolean|true|none|
|»»»» id|body|string|true|none|
|»»»» version|body|string|true|none|
|»»» kube|body|object|false|none|
|»»»» enabled|body|boolean|true|none|
|»»»» namespace|body|string|true|none|
|»»»» node|body|string|true|none|
|»»»» owner|body|object|false|none|
|»»»»» kind|body|string|true|none|
|»»»»» name|body|string|true|none|
|»»»» pod|body|string|true|none|
|»»»» source|body|string|true|none|
|»»» os|body|object|false|none|
|»»»» addresses|body|[string]|true|none|
|»»»» enabled|body|boolean|true|none|
|»»»» id|body|string|true|none|
|»»»» version|body|string|true|none|
|»» node|body|string|true|none|
|»» os|body|any|false|none|
|»»» *anonymous*|body|object|false|none|
|»»»» addresses|body|[string]|true|none|
|»»»» enabled|body|boolean|true|none|
|»»»» id|body|string|true|none|
|»»»» version|body|string|true|none|
|»»» *anonymous*|body|object|false|none|
|»»»» addresses|body|[string]|true|none|
|»» outpost|body|[OutpostNodeInfo](#schemaoutpostnodeinfo)|false|none|
|»»» guid|body|string|true|none|
|»»» host|body|string|true|none|
|»» platform|body|string|true|none|
|»» release|body|string|true|none|
|»» totalDiskSpace|body|number|true|none|
|»» totalmem|body|number|true|none|
|» lastMetrics|body|object|false|none|
|» lastMsgTime|body|number|true|none|
|» metadata|body|[HeartbeatMetadata](#schemaheartbeatmetadata)|false|none|
|» nodeUpgradeStatus|body|[NodeUpgradeStatus](#schemanodeupgradestatus)|false|none|
|»» active|body|[NodeActiveUpgradeStatus](#schemanodeactiveupgradestatus)|false|none|
|»» failed|body|[NodeFailedUpgradeStatus](#schemanodefailedupgradestatus)|false|none|
|»» skipped|body|[NodeSkippedUpgradeStatus](#schemanodeskippedupgradestatus)|false|none|
|»» state|body|[NodeUpgradeState](#schemanodeupgradestate)|true|none|
|»» timestamp|body|number|true|none|
|» status|body|string|false|none|
|» type|body|string|false|none|
|» workerProcesses|body|number|true|none|
|» workers|body|object|false|none|
|»» count|body|number|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» distMode|edge|
|»»» distMode|worker|
|»»» distMode|single|
|»»» distMode|master|
|»»» distMode|managed-edge|
|»»» distMode|outpost|
|»»» distMode|search-supervisor|
|»» active|0|
|»» active|1|
|»» active|2|
|»» failed|0|
|»» failed|1|
|»» skipped|0|
|»» skipped|1|
|»» skipped|2|
|»» skipped|3|
|»» state|0|
|»» state|1|
|»» state|2|
|»» state|3|
|» type|info|
|» type|req|
|» type|resp|

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

<h3 id="update-masterworkerentry-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of MasterWorkerEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-masterworkerentry-responseschema">Response Schema</h3>

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

## Delete MasterWorkerEntry

<a id="opIddeleteOutpostsById"></a>

> Code samples

`DELETE /master/outposts/{id}`

Delete MasterWorkerEntry

<h3 id="delete-masterworkerentry-parameters">Parameters</h3>

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

<h3 id="delete-masterworkerentry-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of MasterWorkerEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-masterworkerentry-responseschema">Response Schema</h3>

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

## Restart Outpost Nodes

<a id="opIdupdateOutpostsRestart"></a>

> Code samples

`PATCH /master/outposts/restart`

restarts outpost nodes

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

<h3 id="restart-outpost-nodes-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of RestartResponse objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="restart-outpost-nodes-responseschema">Response Schema</h3>

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

## Upgrade Outpost Nodes

<a id="opIdupdateOutpostsUpgrade"></a>

> Code samples

`PATCH /master/outposts/upgrade`

upgrades outpost nodes

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "message": "string",
      "status": "Upgrading"
    }
  ]
}
```

<h3 id="upgrade-outpost-nodes-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of OutpostUpgradeResponse objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="upgrade-outpost-nodes-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[OutpostUpgradeResponse](#schemaoutpostupgraderesponse)]|false|none|none|
|»» id|string|true|none|none|
|»» message|string|false|none|none|
|»» status|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|Upgrading|
|status|Error|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

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

<h2 id="tocS_OutpostUpgradeResponse">OutpostUpgradeResponse</h2>
<!-- backwards compatibility -->
<a id="schemaoutpostupgraderesponse"></a>
<a id="schema_OutpostUpgradeResponse"></a>
<a id="tocSoutpostupgraderesponse"></a>
<a id="tocsoutpostupgraderesponse"></a>

```json
{
  "id": "string",
  "message": "string",
  "status": "Upgrading"
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
|status|Upgrading|
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

