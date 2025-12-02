
<h1 id="cribl-core-api-ai-settings">Cribl Core API - AI Settings v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-ai-settings-ai-settings">AI Settings</h1>

## Returns setting for a specific feature

<a id="opIdgetAiSettingsFeaturesByFeature"></a>

> Code samples

`GET /ai/settings/features/{feature}`

Returns setting for a specific feature

<h3 id="returns-setting-for-a-specific-feature-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|feature|path|string|true|Feature name to get settings for|

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

<h3 id="returns-setting-for-a-specific-feature-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Response objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="returns-setting-for-a-specific-feature-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Updates setting for a specific feature

<a id="opIdcreateAiSettingsFeaturesByFeature"></a>

> Code samples

`POST /ai/settings/features/{feature}`

Updates setting for a specific feature

<h3 id="updates-setting-for-a-specific-feature-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|feature|path|string|true|Feature name to update settings for|

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

<h3 id="updates-setting-for-a-specific-feature-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Response objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="updates-setting-for-a-specific-feature-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Returns all feature settings

<a id="opIdgetAiSettingsFeatures"></a>

> Code samples

`GET /ai/settings/features`

Returns all feature settings

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

<h3 id="returns-all-feature-settings-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Response objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="returns-all-feature-settings-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Updates all AI settings at once

<a id="opIdcreateAiSettingsFeatures"></a>

> Code samples

`POST /ai/settings/features`

Updates all AI settings at once

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

<h3 id="updates-all-ai-settings-at-once-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Response objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="updates-all-ai-settings-at-once-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

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

