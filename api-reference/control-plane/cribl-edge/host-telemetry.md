
<h1 id="cribl-edge-api-host-telemetry">Cribl Edge API - Host Telemetry v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-host-telemetry-host-telemetry">Host Telemetry</h1>

## list log files

<a id="opIdgetEdgeLogs"></a>

> Code samples

`GET /edge/logs`

list log files

<h3 id="list-log-files-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|mode|query|string|false|Discovery Mode (default is "auto")|
|allow|query|any|false|List of allowed-filename wildcard patterns|
|path|query|string|false|Search directory for "manual" mode|
|depth|query|number|false|Search depth for "manual" mode|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "filePath": "string",
      "id": "string",
      "modTime": 0,
      "mode": "string",
      "owner": 0,
      "processInfo": [
        {
          "pid": 0,
          "process": "string"
        }
      ],
      "size": 0
    }
  ]
}
```

<h3 id="list-log-files-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of EdgeFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="list-log-files-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[EdgeFile](#schemaedgefile)]|false|none|none|
|»» filePath|string|true|none|none|
|»» id|string|true|none|none|
|»» modTime|number|true|none|none|
|»» mode|string|true|none|none|
|»» owner|number|true|none|none|
|»» processInfo|[[FileProcessInfo](#schemafileprocessinfo)]|false|none|none|
|»»» pid|number|true|none|none|
|»»» process|string|true|none|none|
|»» size|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get the host's metadata structure

<a id="opIdgetEdgeMetadata"></a>

> Code samples

`GET /edge/metadata`

Get the host's metadata structure

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "aws": {
        "hostname": "string",
        "identity": {},
        "public_ipv4": "string",
        "roles": [
          "string"
        ],
        "security_groups": [
          "string"
        ],
        "tags": {},
        "version": 0
      },
      "cribl": {
        "config_version": "string",
        "group": "string",
        "install_type": "string",
        "mode": "string",
        "tags": [
          "string"
        ],
        "version": "string"
      },
      "env": {},
      "hostOs": {
        "arch": "string",
        "cpu_count": 0,
        "cpu_speed_mhz": 0,
        "cpu_type": "string",
        "gid": 0,
        "homedir": "string",
        "hostname": "string",
        "interfaces": {},
        "machine_id": "string",
        "memory": 0,
        "os_id": "string",
        "os_name": "string",
        "os_version": "string",
        "os_version_id": "string",
        "platform": "string",
        "release": "string",
        "timezone": "string",
        "timezone_offset": "string",
        "uid": 0,
        "username": "string"
      },
      "kube": {
        "cluster": {},
        "container": {},
        "namespace": "string",
        "node": {},
        "pod": {},
        "source": "string"
      },
      "os": {
        "arch": "string",
        "cpu_count": 0,
        "cpu_speed_mhz": 0,
        "cpu_type": "string",
        "gid": 0,
        "homedir": "string",
        "hostname": "string",
        "interfaces": {},
        "machine_id": "string",
        "memory": 0,
        "os_id": "string",
        "os_name": "string",
        "os_version": "string",
        "os_version_id": "string",
        "platform": "string",
        "release": "string",
        "timezone": "string",
        "timezone_offset": "string",
        "uid": 0,
        "username": "string"
      },
      "timestamp": 0
    }
  ]
}
```

<h3 id="get-the-host's-metadata-structure-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Metadata objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-host's-metadata-structure-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Metadata](#schemametadata)]|false|none|none|
|»» aws|[AwsMetadata](#schemaawsmetadata)|false|none|none|
|»»» hostname|string|false|none|none|
|»»» identity|object|false|none|none|
|»»» public_ipv4|string|false|none|none|
|»»» roles|[string]|false|none|none|
|»»» security_groups|[string]|false|none|none|
|»»» tags|object|false|none|none|
|»»» version|number|true|none|none|
|»» cribl|[CriblMetadata](#schemacriblmetadata)|false|none|none|
|»»» config_version|string|false|none|none|
|»»» group|string|false|none|none|
|»»» install_type|string|false|none|none|
|»»» mode|string|true|none|none|
|»»» tags|[string]|false|none|none|
|»»» version|string|true|none|none|
|»» env|[EnvMetadata](#schemaenvmetadata)|false|none|none|
|»» hostOs|[OsMetadata](#schemaosmetadata)|false|none|none|
|»»» arch|string|true|none|none|
|»»» cpu_count|number|true|none|none|
|»»» cpu_speed_mhz|number|true|none|none|
|»»» cpu_type|string|true|none|none|
|»»» gid|number|true|none|none|
|»»» homedir|string|true|none|none|
|»»» hostname|string|true|none|none|
|»»» interfaces|[NetworkInterfaces](#schemanetworkinterfaces)|true|none|none|
|»»» machine_id|string|true|none|none|
|»»» memory|number|true|none|none|
|»»» os_id|string|true|none|none|
|»»» os_name|string|true|none|none|
|»»» os_version|string|true|none|none|
|»»» os_version_id|string|true|none|none|
|»»» platform|string|true|none|none|
|»»» release|string|true|none|none|
|»»» timezone|string|true|none|none|
|»»» timezone_offset|string|true|none|none|
|»»» uid|number|true|none|none|
|»»» username|string|true|none|none|
|»» kube|[KubeMetadata](#schemakubemetadata)|false|none|none|
|»»» cluster|object|false|none|none|
|»»» container|object|false|none|none|
|»»» namespace|string|true|none|none|
|»»» node|object|false|none|none|
|»»» pod|object|false|none|none|
|»»» source|string|true|none|none|
|»» os|[OsMetadata](#schemaosmetadata)|false|none|none|
|»» timestamp|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_EdgeFile">EdgeFile</h2>
<!-- backwards compatibility -->
<a id="schemaedgefile"></a>
<a id="schema_EdgeFile"></a>
<a id="tocSedgefile"></a>
<a id="tocsedgefile"></a>

```json
{
  "filePath": "string",
  "id": "string",
  "modTime": 0,
  "mode": "string",
  "owner": 0,
  "processInfo": [
    {
      "pid": 0,
      "process": "string"
    }
  ],
  "size": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|filePath|string|true|none|none|
|id|string|true|none|none|
|modTime|number|true|none|none|
|mode|string|true|none|none|
|owner|number|true|none|none|
|processInfo|[[FileProcessInfo](#schemafileprocessinfo)]|false|none|none|
|size|number|true|none|none|

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

<h2 id="tocS_Metadata">Metadata</h2>
<!-- backwards compatibility -->
<a id="schemametadata"></a>
<a id="schema_Metadata"></a>
<a id="tocSmetadata"></a>
<a id="tocsmetadata"></a>

```json
{
  "aws": {
    "hostname": "string",
    "identity": {},
    "public_ipv4": "string",
    "roles": [
      "string"
    ],
    "security_groups": [
      "string"
    ],
    "tags": {},
    "version": 0
  },
  "cribl": {
    "config_version": "string",
    "group": "string",
    "install_type": "string",
    "mode": "string",
    "tags": [
      "string"
    ],
    "version": "string"
  },
  "env": {},
  "hostOs": {
    "arch": "string",
    "cpu_count": 0,
    "cpu_speed_mhz": 0,
    "cpu_type": "string",
    "gid": 0,
    "homedir": "string",
    "hostname": "string",
    "interfaces": {},
    "machine_id": "string",
    "memory": 0,
    "os_id": "string",
    "os_name": "string",
    "os_version": "string",
    "os_version_id": "string",
    "platform": "string",
    "release": "string",
    "timezone": "string",
    "timezone_offset": "string",
    "uid": 0,
    "username": "string"
  },
  "kube": {
    "cluster": {},
    "container": {},
    "namespace": "string",
    "node": {},
    "pod": {},
    "source": "string"
  },
  "os": {
    "arch": "string",
    "cpu_count": 0,
    "cpu_speed_mhz": 0,
    "cpu_type": "string",
    "gid": 0,
    "homedir": "string",
    "hostname": "string",
    "interfaces": {},
    "machine_id": "string",
    "memory": 0,
    "os_id": "string",
    "os_name": "string",
    "os_version": "string",
    "os_version_id": "string",
    "platform": "string",
    "release": "string",
    "timezone": "string",
    "timezone_offset": "string",
    "uid": 0,
    "username": "string"
  },
  "timestamp": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|aws|[AwsMetadata](#schemaawsmetadata)|false|none|none|
|cribl|[CriblMetadata](#schemacriblmetadata)|false|none|none|
|env|[EnvMetadata](#schemaenvmetadata)|false|none|none|
|hostOs|[OsMetadata](#schemaosmetadata)|false|none|none|
|kube|[KubeMetadata](#schemakubemetadata)|false|none|none|
|os|[OsMetadata](#schemaosmetadata)|false|none|none|
|timestamp|number|true|none|none|

<h2 id="tocS_FileProcessInfo">FileProcessInfo</h2>
<!-- backwards compatibility -->
<a id="schemafileprocessinfo"></a>
<a id="schema_FileProcessInfo"></a>
<a id="tocSfileprocessinfo"></a>
<a id="tocsfileprocessinfo"></a>

```json
{
  "pid": 0,
  "process": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|pid|number|true|none|none|
|process|string|true|none|none|

<h2 id="tocS_AwsMetadata">AwsMetadata</h2>
<!-- backwards compatibility -->
<a id="schemaawsmetadata"></a>
<a id="schema_AwsMetadata"></a>
<a id="tocSawsmetadata"></a>
<a id="tocsawsmetadata"></a>

```json
{
  "hostname": "string",
  "identity": {},
  "public_ipv4": "string",
  "roles": [
    "string"
  ],
  "security_groups": [
    "string"
  ],
  "tags": {},
  "version": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|hostname|string|false|none|none|
|identity|object|false|none|none|
|public_ipv4|string|false|none|none|
|roles|[string]|false|none|none|
|security_groups|[string]|false|none|none|
|tags|object|false|none|none|
|version|number|true|none|none|

<h2 id="tocS_CriblMetadata">CriblMetadata</h2>
<!-- backwards compatibility -->
<a id="schemacriblmetadata"></a>
<a id="schema_CriblMetadata"></a>
<a id="tocScriblmetadata"></a>
<a id="tocscriblmetadata"></a>

```json
{
  "config_version": "string",
  "group": "string",
  "install_type": "string",
  "mode": "string",
  "tags": [
    "string"
  ],
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|config_version|string|false|none|none|
|group|string|false|none|none|
|install_type|string|false|none|none|
|mode|string|true|none|none|
|tags|[string]|false|none|none|
|version|string|true|none|none|

<h2 id="tocS_EnvMetadata">EnvMetadata</h2>
<!-- backwards compatibility -->
<a id="schemaenvmetadata"></a>
<a id="schema_EnvMetadata"></a>
<a id="tocSenvmetadata"></a>
<a id="tocsenvmetadata"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocS_OsMetadata">OsMetadata</h2>
<!-- backwards compatibility -->
<a id="schemaosmetadata"></a>
<a id="schema_OsMetadata"></a>
<a id="tocSosmetadata"></a>
<a id="tocsosmetadata"></a>

```json
{
  "arch": "string",
  "cpu_count": 0,
  "cpu_speed_mhz": 0,
  "cpu_type": "string",
  "gid": 0,
  "homedir": "string",
  "hostname": "string",
  "interfaces": {},
  "machine_id": "string",
  "memory": 0,
  "os_id": "string",
  "os_name": "string",
  "os_version": "string",
  "os_version_id": "string",
  "platform": "string",
  "release": "string",
  "timezone": "string",
  "timezone_offset": "string",
  "uid": 0,
  "username": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|arch|string|true|none|none|
|cpu_count|number|true|none|none|
|cpu_speed_mhz|number|true|none|none|
|cpu_type|string|true|none|none|
|gid|number|true|none|none|
|homedir|string|true|none|none|
|hostname|string|true|none|none|
|interfaces|[NetworkInterfaces](#schemanetworkinterfaces)|true|none|none|
|machine_id|string|true|none|none|
|memory|number|true|none|none|
|os_id|string|true|none|none|
|os_name|string|true|none|none|
|os_version|string|true|none|none|
|os_version_id|string|true|none|none|
|platform|string|true|none|none|
|release|string|true|none|none|
|timezone|string|true|none|none|
|timezone_offset|string|true|none|none|
|uid|number|true|none|none|
|username|string|true|none|none|

<h2 id="tocS_KubeMetadata">KubeMetadata</h2>
<!-- backwards compatibility -->
<a id="schemakubemetadata"></a>
<a id="schema_KubeMetadata"></a>
<a id="tocSkubemetadata"></a>
<a id="tocskubemetadata"></a>

```json
{
  "cluster": {},
  "container": {},
  "namespace": "string",
  "node": {},
  "pod": {},
  "source": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cluster|object|false|none|none|
|container|object|false|none|none|
|namespace|string|true|none|none|
|node|object|false|none|none|
|pod|object|false|none|none|
|source|string|true|none|none|

<h2 id="tocS_NetworkInterfaces">NetworkInterfaces</h2>
<!-- backwards compatibility -->
<a id="schemanetworkinterfaces"></a>
<a id="schema_NetworkInterfaces"></a>
<a id="tocSnetworkinterfaces"></a>
<a id="tocsnetworkinterfaces"></a>

```json
{}

```

### Properties

*None*

