
<h1 id="cribl-core-api-health">Cribl Core API - Health v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-health-health">Health</h1>

## Retrieve health status of the server

<a id="opIdgetHealth"></a>

> Code samples

`GET /health`

Get the current health status of the server.

> Example responses

> 200 Response

```json
{
  "role": "standby",
  "startTime": 0,
  "status": "shutting down"
}
```

<h3 id="retrieve-health-status-of-the-server-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Healthy status|[HealthServerStatus](#schemahealthserverstatus)|
|420|Unknown|Server is shutting down or in standby mode|[HealthServerStatus](#schemahealthserverstatus)|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_HealthServerStatus">HealthServerStatus</h2>
<!-- backwards compatibility -->
<a id="schemahealthserverstatus"></a>
<a id="schema_HealthServerStatus"></a>
<a id="tocShealthserverstatus"></a>
<a id="tocshealthserverstatus"></a>

```json
{
  "role": "standby",
  "startTime": 0,
  "status": "shutting down"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|role|string|false|none|none|
|startTime|number|true|none|none|
|status|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|role|standby|
|role|primary|
|status|shutting down|
|status|healthy|
|status|standby|

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

