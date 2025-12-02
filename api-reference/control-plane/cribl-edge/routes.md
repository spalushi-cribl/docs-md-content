
<h1 id="cribl-edge-api-routes">Cribl Edge API - Routes v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-routes-routes">Routes</h1>

## List all Routes

<a id="opIdlistRoutes"></a>

> Code samples

`GET /routes`

Get a list of all Routes.

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "routes": [
        {
          "id": "string",
          "name": "string",
          "disabled": true,
          "filter": "true",
          "pipeline": "string",
          "enableOutputExpression": false,
          "output": null,
          "outputExpression": null,
          "description": "string",
          "final": true
        }
      ],
      "groups": {
        "property1": {
          "name": "string",
          "description": "string",
          "disabled": true
        },
        "property2": {
          "name": "string",
          "description": "string",
          "disabled": true
        }
      },
      "comments": [
        {
          "comment": "string"
        }
      ]
    }
  ]
}
```

<h3 id="list-all-routes-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Routes objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="list-all-routes-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Routes](#schemaroutes)]|false|none|none|
|»» id|string|false|none|Routes ID|
|»» routes|[[RoutesRoute](#schemaroutesroute)]|true|none|Pipeline routing rules|
|»»» id|string|false|none|none|
|»»» name|string|true|none|none|
|»»» disabled|boolean|false|none|Disable this routing rule|
|»»» filter|string|false|none|JavaScript expression to select data to route|
|»»» pipeline|string|true|none|Pipeline to send the matching data to|
|»»» enableOutputExpression|boolean|false|none|Enable to use a JavaScript expression that evaluates to the name of the Description below|
|»»» output|any|false|none|none|
|»»» outputExpression|any|false|none|none|
|»»» description|string|false|none|none|
|»»» final|boolean|false|none|Flag to control whether the event gets consumed by this Route (Final), or cloned into it|
|»» groups|object|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» name|string|true|none|none|
|»»»» description|string|false|none|Short description of this group|
|»»»» disabled|boolean|false|none|Whether this group is disabled|
|»» comments|[object]|false|none|Comments|
|»»» comment|string|false|none|Optional, short description of this Route's purpose|

<aside class="success">
This operation does not require authentication
</aside>

## Get a Routing table

<a id="opIdgetRoutesById"></a>

> Code samples

`GET /routes/{id}`

Get the specified Routing table.

<h3 id="get-a-routing-table-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|The <code>id</code> of the Routing table to get. The supported value is <code>default</code>.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "routes": [
        {
          "id": "string",
          "name": "string",
          "disabled": true,
          "filter": "true",
          "pipeline": "string",
          "enableOutputExpression": false,
          "output": null,
          "outputExpression": null,
          "description": "string",
          "final": true
        }
      ],
      "groups": {
        "property1": {
          "name": "string",
          "description": "string",
          "disabled": true
        },
        "property2": {
          "name": "string",
          "description": "string",
          "disabled": true
        }
      },
      "comments": [
        {
          "comment": "string"
        }
      ]
    }
  ]
}
```

<h3 id="get-a-routing-table-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Routes objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-routing-table-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Routes](#schemaroutes)]|false|none|none|
|»» id|string|false|none|Routes ID|
|»» routes|[[RoutesRoute](#schemaroutesroute)]|true|none|Pipeline routing rules|
|»»» id|string|false|none|none|
|»»» name|string|true|none|none|
|»»» disabled|boolean|false|none|Disable this routing rule|
|»»» filter|string|false|none|JavaScript expression to select data to route|
|»»» pipeline|string|true|none|Pipeline to send the matching data to|
|»»» enableOutputExpression|boolean|false|none|Enable to use a JavaScript expression that evaluates to the name of the Description below|
|»»» output|any|false|none|none|
|»»» outputExpression|any|false|none|none|
|»»» description|string|false|none|none|
|»»» final|boolean|false|none|Flag to control whether the event gets consumed by this Route (Final), or cloned into it|
|»» groups|object|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» name|string|true|none|none|
|»»»» description|string|false|none|Short description of this group|
|»»»» disabled|boolean|false|none|Whether this group is disabled|
|»» comments|[object]|false|none|Comments|
|»»» comment|string|false|none|Optional, short description of this Route's purpose|

<aside class="success">
This operation does not require authentication
</aside>

## Update a Route

<a id="opIdupdateRoutesById"></a>

> Code samples

`PATCH /routes/{id}`

Update a Route in the specified Routing table.</br></br>Provide a complete representation of the Routing table, including the Route that you want to update, in the request body. This endpoint does not support partial updates. Cribl removes any omitted Routes and fields when updating.</br></br>Confirm that the configuration in your request body is correct before sending the request. If the configuration is incorrect, the Routing table might not function as expected.

> Body parameter

```json
{
  "id": "string",
  "routes": [
    {
      "id": "string",
      "name": "string",
      "disabled": true,
      "filter": "true",
      "pipeline": "string",
      "enableOutputExpression": false,
      "output": null,
      "outputExpression": null,
      "description": "string",
      "final": true
    }
  ],
  "groups": {
    "property1": {
      "name": "string",
      "description": "string",
      "disabled": true
    },
    "property2": {
      "name": "string",
      "description": "string",
      "disabled": true
    }
  },
  "comments": [
    {
      "comment": "string"
    }
  ]
}
```

<h3 id="update-a-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|The <code>id</code> of the Routing table that contains the Route to update. The supported value is <code>default</code>.|
|body|body|[Routes](#schemaroutes)|true|Routes object|
|» id|body|string|false|Routes ID|
|» routes|body|[[RoutesRoute](#schemaroutesroute)]|true|Pipeline routing rules|
|»» id|body|string|false|none|
|»» name|body|string|true|none|
|»» disabled|body|boolean|false|Disable this routing rule|
|»» filter|body|string|false|JavaScript expression to select data to route|
|»» pipeline|body|string|true|Pipeline to send the matching data to|
|»» enableOutputExpression|body|boolean|false|Enable to use a JavaScript expression that evaluates to the name of the Description below|
|»» output|body|any|false|none|
|»» outputExpression|body|any|false|none|
|»» description|body|string|false|none|
|»» final|body|boolean|false|Flag to control whether the event gets consumed by this Route (Final), or cloned into it|
|» groups|body|object|false|none|
|»» **additionalProperties**|body|object|false|none|
|»»» name|body|string|true|none|
|»»» description|body|string|false|Short description of this group|
|»»» disabled|body|boolean|false|Whether this group is disabled|
|» comments|body|[object]|false|Comments|
|»» comment|body|string|false|Optional, short description of this Route's purpose|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "routes": [
        {
          "id": "string",
          "name": "string",
          "disabled": true,
          "filter": "true",
          "pipeline": "string",
          "enableOutputExpression": false,
          "output": null,
          "outputExpression": null,
          "description": "string",
          "final": true
        }
      ],
      "groups": {
        "property1": {
          "name": "string",
          "description": "string",
          "disabled": true
        },
        "property2": {
          "name": "string",
          "description": "string",
          "disabled": true
        }
      },
      "comments": [
        {
          "comment": "string"
        }
      ]
    }
  ]
}
```

<h3 id="update-a-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Routes objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-a-route-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Routes](#schemaroutes)]|false|none|none|
|»» id|string|false|none|Routes ID|
|»» routes|[[RoutesRoute](#schemaroutesroute)]|true|none|Pipeline routing rules|
|»»» id|string|false|none|none|
|»»» name|string|true|none|none|
|»»» disabled|boolean|false|none|Disable this routing rule|
|»»» filter|string|false|none|JavaScript expression to select data to route|
|»»» pipeline|string|true|none|Pipeline to send the matching data to|
|»»» enableOutputExpression|boolean|false|none|Enable to use a JavaScript expression that evaluates to the name of the Description below|
|»»» output|any|false|none|none|
|»»» outputExpression|any|false|none|none|
|»»» description|string|false|none|none|
|»»» final|boolean|false|none|Flag to control whether the event gets consumed by this Route (Final), or cloned into it|
|»» groups|object|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» name|string|true|none|none|
|»»»» description|string|false|none|Short description of this group|
|»»»» disabled|boolean|false|none|Whether this group is disabled|
|»» comments|[object]|false|none|Comments|
|»»» comment|string|false|none|Optional, short description of this Route's purpose|

<aside class="success">
This operation does not require authentication
</aside>

## Add a Route to the end of the Routing table

<a id="opIdcreateRoutesAppendById"></a>

> Code samples

`POST /routes/{id}/append`

Add a Route to the end of the specified Routing table.

> Body parameter

```json
[
  {
    "clones": [
      {
        "property1": "string",
        "property2": "string"
      }
    ],
    "context": "string",
    "description": "string",
    "disabled": true,
    "enableOutputExpression": true,
    "filter": "string",
    "final": true,
    "groupId": "string",
    "id": "string",
    "name": "string",
    "output": "string",
    "outputExpression": "string",
    "pipeline": "string"
  }
]
```

<h3 id="add-a-route-to-the-end-of-the-routing-table-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|The <code>id</code> of the Routing table to add the Route to. The supported value is <code>default</code>.|
|body|body|[RouteDefinitions](#schemaroutedefinitions)|true|RouteDefinitions object|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "routes": [
        {
          "id": "string",
          "name": "string",
          "disabled": true,
          "filter": "true",
          "pipeline": "string",
          "enableOutputExpression": false,
          "output": null,
          "outputExpression": null,
          "description": "string",
          "final": true
        }
      ],
      "groups": {
        "property1": {
          "name": "string",
          "description": "string",
          "disabled": true
        },
        "property2": {
          "name": "string",
          "description": "string",
          "disabled": true
        }
      },
      "comments": [
        {
          "comment": "string"
        }
      ]
    }
  ]
}
```

<h3 id="add-a-route-to-the-end-of-the-routing-table-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Routes objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="add-a-route-to-the-end-of-the-routing-table-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Routes](#schemaroutes)]|false|none|none|
|»» id|string|false|none|Routes ID|
|»» routes|[[RoutesRoute](#schemaroutesroute)]|true|none|Pipeline routing rules|
|»»» id|string|false|none|none|
|»»» name|string|true|none|none|
|»»» disabled|boolean|false|none|Disable this routing rule|
|»»» filter|string|false|none|JavaScript expression to select data to route|
|»»» pipeline|string|true|none|Pipeline to send the matching data to|
|»»» enableOutputExpression|boolean|false|none|Enable to use a JavaScript expression that evaluates to the name of the Description below|
|»»» output|any|false|none|none|
|»»» outputExpression|any|false|none|none|
|»»» description|string|false|none|none|
|»»» final|boolean|false|none|Flag to control whether the event gets consumed by this Route (Final), or cloned into it|
|»» groups|object|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» name|string|true|none|none|
|»»»» description|string|false|none|Short description of this group|
|»»»» disabled|boolean|false|none|Whether this group is disabled|
|»» comments|[object]|false|none|Comments|
|»»» comment|string|false|none|Optional, short description of this Route's purpose|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Routes">Routes</h2>
<!-- backwards compatibility -->
<a id="schemaroutes"></a>
<a id="schema_Routes"></a>
<a id="tocSroutes"></a>
<a id="tocsroutes"></a>

```json
{
  "id": "string",
  "routes": [
    {
      "id": "string",
      "name": "string",
      "disabled": true,
      "filter": "true",
      "pipeline": "string",
      "enableOutputExpression": false,
      "output": null,
      "outputExpression": null,
      "description": "string",
      "final": true
    }
  ],
  "groups": {
    "property1": {
      "name": "string",
      "description": "string",
      "disabled": true
    },
    "property2": {
      "name": "string",
      "description": "string",
      "disabled": true
    }
  },
  "comments": [
    {
      "comment": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|Routes ID|
|routes|[[RoutesRoute](#schemaroutesroute)]|true|none|Pipeline routing rules|
|groups|object|false|none|none|
|» **additionalProperties**|object|false|none|none|
|»» name|string|true|none|none|
|»» description|string|false|none|Short description of this group|
|»» disabled|boolean|false|none|Whether this group is disabled|
|comments|[object]|false|none|Comments|
|» comment|string|false|none|Optional, short description of this Route's purpose|

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

<h2 id="tocS_RouteDefinitions">RouteDefinitions</h2>
<!-- backwards compatibility -->
<a id="schemaroutedefinitions"></a>
<a id="schema_RouteDefinitions"></a>
<a id="tocSroutedefinitions"></a>
<a id="tocsroutedefinitions"></a>

```json
[
  {
    "clones": [
      {
        "property1": "string",
        "property2": "string"
      }
    ],
    "context": "string",
    "description": "string",
    "disabled": true,
    "enableOutputExpression": true,
    "filter": "string",
    "final": true,
    "groupId": "string",
    "id": "string",
    "name": "string",
    "output": "string",
    "outputExpression": "string",
    "pipeline": "string"
  }
]

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[RouteConf](#schemarouteconf)]|false|none|none|

<h2 id="tocS_RoutesRoute">RoutesRoute</h2>
<!-- backwards compatibility -->
<a id="schemaroutesroute"></a>
<a id="schema_RoutesRoute"></a>
<a id="tocSroutesroute"></a>
<a id="tocsroutesroute"></a>

```json
{
  "id": "string",
  "name": "string",
  "disabled": true,
  "filter": "true",
  "pipeline": "string",
  "enableOutputExpression": false,
  "output": null,
  "outputExpression": null,
  "description": "string",
  "final": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|name|string|true|none|none|
|disabled|boolean|false|none|Disable this routing rule|
|filter|string|false|none|JavaScript expression to select data to route|
|pipeline|string|true|none|Pipeline to send the matching data to|
|enableOutputExpression|boolean|false|none|Enable to use a JavaScript expression that evaluates to the name of the Description below|
|output|any|false|none|none|
|outputExpression|any|false|none|none|
|description|string|false|none|none|
|final|boolean|false|none|Flag to control whether the event gets consumed by this Route (Final), or cloned into it|

<h2 id="tocS_RouteConf">RouteConf</h2>
<!-- backwards compatibility -->
<a id="schemarouteconf"></a>
<a id="schema_RouteConf"></a>
<a id="tocSrouteconf"></a>
<a id="tocsrouteconf"></a>

```json
{
  "clones": [
    {
      "property1": "string",
      "property2": "string"
    }
  ],
  "context": "string",
  "description": "string",
  "disabled": true,
  "enableOutputExpression": true,
  "filter": "string",
  "final": true,
  "groupId": "string",
  "id": "string",
  "name": "string",
  "output": "string",
  "outputExpression": "string",
  "pipeline": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|clones|[[RouteCloneConf](#schemaroutecloneconf)]|false|none|[Key value pairs to set/overwrite in the clone]|
|context|string|false|none|none|
|description|string|false|none|none|
|disabled|boolean|false|none|none|
|enableOutputExpression|boolean|false|none|none|
|filter|string|false|none|none|
|final|boolean|true|none|none|
|groupId|string|false|none|none|
|id|string|true|none|none|
|name|string|true|none|none|
|output|string|false|none|none|
|outputExpression|string|false|none|none|
|pipeline|string|true|none|none|

<h2 id="tocS_RouteCloneConf">RouteCloneConf</h2>
<!-- backwards compatibility -->
<a id="schemaroutecloneconf"></a>
<a id="schema_RouteCloneConf"></a>
<a id="tocSroutecloneconf"></a>
<a id="tocsroutecloneconf"></a>

```json
{
  "property1": "string",
  "property2": "string"
}

```

Fields

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Clone fields|string|false|none|none|

