
<h1 id="cribl-edge-api-containers">Cribl Edge API - Containers v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-containers-containers">Containers</h1>

## Get a detailed list of containers running on the edge host.

<a id="opIdgetEdgeContainers"></a>

> Code samples

`GET /edge/containers`

Get a detailed list of containers running on the edge host.

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "command": "string",
      "containerd": {
        "container": {
          "mounts": [
            {
              "destination": "string",
              "source": "string"
            }
          ],
          "root": {
            "path": "string"
          }
        },
        "image": {},
        "metrics": {},
        "namespace": {},
        "task": {
          "process": {}
        }
      },
      "created": 0,
      "docker": {
        "Config": {},
        "GraphDriver": {
          "Data": {
            "MergedDir": "string"
          }
        },
        "LogPath": "string",
        "Mounts": [
          {
            "Destination": "string",
            "Source": "string"
          }
        ],
        "NetworkSettings": {},
        "Path": "string",
        "State": {},
        "Stats": {}
      },
      "id": "string",
      "image": "string",
      "ips": [
        "string"
      ],
      "name": "string",
      "ports": [
        {
          "privatePort": 0,
          "publicPort": 0
        }
      ],
      "status": "string",
      "type": "docker"
    }
  ]
}
```

<h3 id="get-a-detailed-list-of-containers-running-on-the-edge-host.-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Container objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-detailed-list-of-containers-running-on-the-edge-host.-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Container](#schemacontainer)]|false|none|none|
|»» command|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|[string]|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» containerd|[ContainerdInfo](#schemacontainerdinfo)|false|none|none|
|»»» container|[ContainerdContainer](#schemacontainerdcontainer)|true|none|none|
|»»»» mounts|[[ContainerdMount](#schemacontainerdmount)]|true|none|none|
|»»»»» destination|string|true|none|none|
|»»»»» source|string|true|none|none|
|»»»» root|[ContainerdRoot](#schemacontainerdroot)|true|none|none|
|»»»»» path|string|true|none|none|
|»»» image|object|false|none|none|
|»»» metrics|object|false|none|none|
|»»» namespace|object|false|none|none|
|»»» task|[ContainerdTask](#schemacontainerdtask)|false|none|none|
|»»»» process|object|false|none|none|
|»» created|number|true|none|none|
|»» docker|[DockerInfo](#schemadockerinfo)|false|none|none|
|»»» Config|object|false|none|none|
|»»» GraphDriver|[DockerGraphDriver](#schemadockergraphdriver)|false|none|none|
|»»»» Data|[DockerGraphDriverData](#schemadockergraphdriverdata)|false|none|none|
|»»»»» MergedDir|string|true|none|none|
|»»» LogPath|string|false|none|none|
|»»» Mounts|[[DockerMount](#schemadockermount)]|false|none|none|
|»»»» Destination|string|true|none|none|
|»»»» Source|string|true|none|none|
|»»» NetworkSettings|object|false|none|none|
|»»» Path|string|false|none|none|
|»»» State|object|false|none|none|
|»»» Stats|object|false|none|none|
|»» id|string|true|none|none|
|»» image|string|true|none|none|
|»» ips|[string]|false|none|none|
|»» name|string|true|none|none|
|»» ports|[[ContainerPort](#schemacontainerport)]|false|none|none|
|»»» privatePort|number|true|none|none|
|»»» publicPort|number|true|none|none|
|»» status|string|true|none|none|
|»» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|docker|
|type|containerd|

<aside class="success">
This operation does not require authentication
</aside>

## Get details for a single container on the edge host. Add stream=true to get a stream instead.

<a id="opIdgetEdgeContainersById"></a>

> Code samples

`GET /edge/containers/{id}`

Get details for a single container on the edge host. Add stream=true to get a stream instead.

<h3 id="get-details-for-a-single-container-on-the-edge-host.-add-stream=true-to-get-a-stream-instead.-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "command": "string",
      "containerd": {
        "container": {
          "mounts": [
            {
              "destination": "string",
              "source": "string"
            }
          ],
          "root": {
            "path": "string"
          }
        },
        "image": {},
        "metrics": {},
        "namespace": {},
        "task": {
          "process": {}
        }
      },
      "created": 0,
      "docker": {
        "Config": {},
        "GraphDriver": {
          "Data": {
            "MergedDir": "string"
          }
        },
        "LogPath": "string",
        "Mounts": [
          {
            "Destination": "string",
            "Source": "string"
          }
        ],
        "NetworkSettings": {},
        "Path": "string",
        "State": {},
        "Stats": {}
      },
      "id": "string",
      "image": "string",
      "ips": [
        "string"
      ],
      "name": "string",
      "ports": [
        {
          "privatePort": 0,
          "publicPort": 0
        }
      ],
      "status": "string",
      "type": "docker"
    }
  ]
}
```

<h3 id="get-details-for-a-single-container-on-the-edge-host.-add-stream=true-to-get-a-stream-instead.-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Container objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-details-for-a-single-container-on-the-edge-host.-add-stream=true-to-get-a-stream-instead.-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Container](#schemacontainer)]|false|none|none|
|»» command|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|[string]|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» containerd|[ContainerdInfo](#schemacontainerdinfo)|false|none|none|
|»»» container|[ContainerdContainer](#schemacontainerdcontainer)|true|none|none|
|»»»» mounts|[[ContainerdMount](#schemacontainerdmount)]|true|none|none|
|»»»»» destination|string|true|none|none|
|»»»»» source|string|true|none|none|
|»»»» root|[ContainerdRoot](#schemacontainerdroot)|true|none|none|
|»»»»» path|string|true|none|none|
|»»» image|object|false|none|none|
|»»» metrics|object|false|none|none|
|»»» namespace|object|false|none|none|
|»»» task|[ContainerdTask](#schemacontainerdtask)|false|none|none|
|»»»» process|object|false|none|none|
|»» created|number|true|none|none|
|»» docker|[DockerInfo](#schemadockerinfo)|false|none|none|
|»»» Config|object|false|none|none|
|»»» GraphDriver|[DockerGraphDriver](#schemadockergraphdriver)|false|none|none|
|»»»» Data|[DockerGraphDriverData](#schemadockergraphdriverdata)|false|none|none|
|»»»»» MergedDir|string|true|none|none|
|»»» LogPath|string|false|none|none|
|»»» Mounts|[[DockerMount](#schemadockermount)]|false|none|none|
|»»»» Destination|string|true|none|none|
|»»»» Source|string|true|none|none|
|»»» NetworkSettings|object|false|none|none|
|»»» Path|string|false|none|none|
|»»» State|object|false|none|none|
|»»» Stats|object|false|none|none|
|»» id|string|true|none|none|
|»» image|string|true|none|none|
|»» ips|[string]|false|none|none|
|»» name|string|true|none|none|
|»» ports|[[ContainerPort](#schemacontainerport)]|false|none|none|
|»»» privatePort|number|true|none|none|
|»»» publicPort|number|true|none|none|
|»» status|string|true|none|none|
|»» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|docker|
|type|containerd|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Container">Container</h2>
<!-- backwards compatibility -->
<a id="schemacontainer"></a>
<a id="schema_Container"></a>
<a id="tocScontainer"></a>
<a id="tocscontainer"></a>

```json
{
  "command": "string",
  "containerd": {
    "container": {
      "mounts": [
        {
          "destination": "string",
          "source": "string"
        }
      ],
      "root": {
        "path": "string"
      }
    },
    "image": {},
    "metrics": {},
    "namespace": {},
    "task": {
      "process": {}
    }
  },
  "created": 0,
  "docker": {
    "Config": {},
    "GraphDriver": {
      "Data": {
        "MergedDir": "string"
      }
    },
    "LogPath": "string",
    "Mounts": [
      {
        "Destination": "string",
        "Source": "string"
      }
    ],
    "NetworkSettings": {},
    "Path": "string",
    "State": {},
    "Stats": {}
  },
  "id": "string",
  "image": "string",
  "ips": [
    "string"
  ],
  "name": "string",
  "ports": [
    {
      "privatePort": 0,
      "publicPort": 0
    }
  ],
  "status": "string",
  "type": "docker"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|command|any|true|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|[string]|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|containerd|[ContainerdInfo](#schemacontainerdinfo)|false|none|none|
|created|number|true|none|none|
|docker|[DockerInfo](#schemadockerinfo)|false|none|none|
|id|string|true|none|none|
|image|string|true|none|none|
|ips|[string]|false|none|none|
|name|string|true|none|none|
|ports|[[ContainerPort](#schemacontainerport)]|false|none|none|
|status|string|true|none|none|
|type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|docker|
|type|containerd|

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

<h2 id="tocS_ContainerdInfo">ContainerdInfo</h2>
<!-- backwards compatibility -->
<a id="schemacontainerdinfo"></a>
<a id="schema_ContainerdInfo"></a>
<a id="tocScontainerdinfo"></a>
<a id="tocscontainerdinfo"></a>

```json
{
  "container": {
    "mounts": [
      {
        "destination": "string",
        "source": "string"
      }
    ],
    "root": {
      "path": "string"
    }
  },
  "image": {},
  "metrics": {},
  "namespace": {},
  "task": {
    "process": {}
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|container|[ContainerdContainer](#schemacontainerdcontainer)|true|none|none|
|image|object|false|none|none|
|metrics|object|false|none|none|
|namespace|object|false|none|none|
|task|[ContainerdTask](#schemacontainerdtask)|false|none|none|

<h2 id="tocS_DockerInfo">DockerInfo</h2>
<!-- backwards compatibility -->
<a id="schemadockerinfo"></a>
<a id="schema_DockerInfo"></a>
<a id="tocSdockerinfo"></a>
<a id="tocsdockerinfo"></a>

```json
{
  "Config": {},
  "GraphDriver": {
    "Data": {
      "MergedDir": "string"
    }
  },
  "LogPath": "string",
  "Mounts": [
    {
      "Destination": "string",
      "Source": "string"
    }
  ],
  "NetworkSettings": {},
  "Path": "string",
  "State": {},
  "Stats": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Config|object|false|none|none|
|GraphDriver|[DockerGraphDriver](#schemadockergraphdriver)|false|none|none|
|LogPath|string|false|none|none|
|Mounts|[[DockerMount](#schemadockermount)]|false|none|none|
|NetworkSettings|object|false|none|none|
|Path|string|false|none|none|
|State|object|false|none|none|
|Stats|object|false|none|none|

<h2 id="tocS_ContainerPort">ContainerPort</h2>
<!-- backwards compatibility -->
<a id="schemacontainerport"></a>
<a id="schema_ContainerPort"></a>
<a id="tocScontainerport"></a>
<a id="tocscontainerport"></a>

```json
{
  "privatePort": 0,
  "publicPort": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|privatePort|number|true|none|none|
|publicPort|number|true|none|none|

<h2 id="tocS_ContainerdContainer">ContainerdContainer</h2>
<!-- backwards compatibility -->
<a id="schemacontainerdcontainer"></a>
<a id="schema_ContainerdContainer"></a>
<a id="tocScontainerdcontainer"></a>
<a id="tocscontainerdcontainer"></a>

```json
{
  "mounts": [
    {
      "destination": "string",
      "source": "string"
    }
  ],
  "root": {
    "path": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|mounts|[[ContainerdMount](#schemacontainerdmount)]|true|none|none|
|root|[ContainerdRoot](#schemacontainerdroot)|true|none|none|

<h2 id="tocS_ContainerdTask">ContainerdTask</h2>
<!-- backwards compatibility -->
<a id="schemacontainerdtask"></a>
<a id="schema_ContainerdTask"></a>
<a id="tocScontainerdtask"></a>
<a id="tocscontainerdtask"></a>

```json
{
  "process": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|process|object|false|none|none|

<h2 id="tocS_DockerGraphDriver">DockerGraphDriver</h2>
<!-- backwards compatibility -->
<a id="schemadockergraphdriver"></a>
<a id="schema_DockerGraphDriver"></a>
<a id="tocSdockergraphdriver"></a>
<a id="tocsdockergraphdriver"></a>

```json
{
  "Data": {
    "MergedDir": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|[DockerGraphDriverData](#schemadockergraphdriverdata)|false|none|none|

<h2 id="tocS_DockerMount">DockerMount</h2>
<!-- backwards compatibility -->
<a id="schemadockermount"></a>
<a id="schema_DockerMount"></a>
<a id="tocSdockermount"></a>
<a id="tocsdockermount"></a>

```json
{
  "Destination": "string",
  "Source": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Destination|string|true|none|none|
|Source|string|true|none|none|

<h2 id="tocS_ContainerdMount">ContainerdMount</h2>
<!-- backwards compatibility -->
<a id="schemacontainerdmount"></a>
<a id="schema_ContainerdMount"></a>
<a id="tocScontainerdmount"></a>
<a id="tocscontainerdmount"></a>

```json
{
  "destination": "string",
  "source": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|destination|string|true|none|none|
|source|string|true|none|none|

<h2 id="tocS_ContainerdRoot">ContainerdRoot</h2>
<!-- backwards compatibility -->
<a id="schemacontainerdroot"></a>
<a id="schema_ContainerdRoot"></a>
<a id="tocScontainerdroot"></a>
<a id="tocscontainerdroot"></a>

```json
{
  "path": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|path|string|true|none|none|

<h2 id="tocS_DockerGraphDriverData">DockerGraphDriverData</h2>
<!-- backwards compatibility -->
<a id="schemadockergraphdriverdata"></a>
<a id="schema_DockerGraphDriverData"></a>
<a id="tocSdockergraphdriverdata"></a>
<a id="tocsdockergraphdriverdata"></a>

```json
{
  "MergedDir": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|MergedDir|string|true|none|none|

