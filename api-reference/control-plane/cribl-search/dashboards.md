
<h1 id="cribl-search-api-dashboards">Cribl Search API - Dashboards v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-search-api-dashboards-dashboards">Dashboards</h1>

## Get a list of SearchDashboard objects

<a id="opIdlistSearchDashboard"></a>

> Code samples

`GET /search/dashboards`

Get a list of SearchDashboard objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "autoApplyDebounceMs": 0,
      "autoApplyMode": "metric",
      "autoApplyUrlSync": "push",
      "cacheTTLSeconds": 0,
      "category": "string",
      "created": 0,
      "createdBy": "string",
      "description": "string",
      "displayCreatedBy": "string",
      "displayModifiedBy": "string",
      "elements": [
        {
          "config": {},
          "description": "string",
          "empty": true,
          "group": "string",
          "hidePanel": true,
          "horizontalChart": true,
          "id": "string",
          "index": 0,
          "layout": {
            "h": 0,
            "w": 0,
            "x": 0,
            "y": 0
          },
          "search": {
            "query": "string",
            "queryId": "string",
            "runMode": "newSearch",
            "type": "saved"
          },
          "title": "string",
          "titleAction": {
            "label": "string",
            "openInNewTab": true,
            "url": "string"
          },
          "type": "chart.area",
          "variant": "visualization"
        }
      ],
      "groups": {
        "property1": {
          "action": {
            "label": "string",
            "params": {
              "property1": "string",
              "property2": "string"
            },
            "target": "string"
          },
          "collapsed": true,
          "inputId": "string",
          "title": "string"
        },
        "property2": {
          "action": {
            "label": "string",
            "params": {
              "property1": "string",
              "property2": "string"
            },
            "target": "string"
          },
          "collapsed": true,
          "inputId": "string",
          "title": "string"
        }
      },
      "id": "string",
      "modified": 0,
      "modifiedBy": "string",
      "name": "string",
      "packId": "string",
      "refreshRate": 0,
      "resolvedDatasetIds": [
        "string"
      ],
      "schedule": {
        "cronSchedule": "string",
        "enabled": true,
        "keepLastN": 0,
        "notifications": {
          "disabled": true,
          "items": [
            {
              "id": "string",
              "disabled": false,
              "condition": "string",
              "targets": [],
              "targetConfigs": [
                {
                  "id": "string",
                  "conf": {
                    "subject": "string",
                    "body": "string",
                    "emailRecipient": {
                      "to": "string",
                      "cc": "string",
                      "bcc": "string"
                    }
                  }
                }
              ],
              "conf": {},
              "metadata": [
                {
                  "name": "string",
                  "value": "string"
                }
              ]
            }
          ]
        },
        "resumeMissed": true,
        "resumeOnBoot": true,
        "tz": "string"
      }
    }
  ]
}
```

<h3 id="get-a-list-of-searchdashboard-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchDashboard objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-searchdashboard-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchDashboard](#schemasearchdashboard)]|false|none|none|
|»» autoApplyDebounceMs|number|false|none|none|
|»» autoApplyMode|string|false|none|none|
|»» autoApplyUrlSync|string|false|none|none|
|»» cacheTTLSeconds|number|false|none|none|
|»» category|string|false|none|none|
|»» created|number|true|none|none|
|»» createdBy|string|true|none|none|
|»» description|string|false|none|none|
|»» displayCreatedBy|string|false|none|none|
|»» displayModifiedBy|string|false|none|none|
|»» elements|[oneOf]|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» config|[ElementConfigType](#schemaelementconfigtype)|false|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»»» h|number|true|none|none|
|»»»»» w|number|true|none|none|
|»»»»» x|number|true|none|none|
|»»»»» y|number|true|none|none|
|»»»» search|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» query|string|false|none|none|
|»»»»»» queryId|string|true|none|none|
|»»»»»» runMode|[SavesSearchRunMode](#schemasavessearchrunmode)|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» earliest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» expectedOutputType|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|none|
|»»»»»» latest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» parentSearchId|string|false|none|none|
|»»»»»» query|string|true|none|none|
|»»»»»» sampleRate|number|false|none|none|
|»»»»»» timezone|string|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» type|string|true|none|none|
|»»»»»» values|[string]|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» earliest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» expectedOutputType|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|none|
|»»»»»» latest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» queries|[[PanelQueryDefinition](#schemapanelquerydefinition)]|true|none|none|
|»»»»»»» alias|string|false|none|none|
|»»»»»»» localId|string|true|none|none|
|»»»»»»» query|string|true|none|none|
|»»»»»» timezone|string|false|none|none|
|»»»»»» type|string|true|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»»» label|string|true|none|none|
|»»»»» openInNewTab|boolean|false|none|none|
|»»»»» url|string|true|none|none|
|»»»» type|[VisualizationElementType](#schemavisualizationelementtype)|true|none|none|
|»»»» variant|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» inputId|string|true|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»» search|any|false|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»» type|[InputElementType](#schemainputelementtype)|true|none|none|
|»»»» value|object|false|none|none|
|»»»» variant|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» config|[MarkdownElementConfig](#schemamarkdownelementconfig)|false|none|none|
|»»»»» markdown|string|true|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»» type|[MarkdownElementType](#schemamarkdownelementtype)|true|none|none|
|»»»» variant|string|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» groups|[DashboardGroups](#schemadashboardgroups)|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» action|object|false|none|none|
|»»»»» label|string|true|none|none|
|»»»»» params|object|false|none|none|
|»»»»»» **additionalProperties**|string|false|none|none|
|»»»»» target|string|true|none|none|
|»»»» collapsed|boolean|false|none|none|
|»»»» inputId|string|false|none|none|
|»»»» title|string|true|none|none|
|»» id|string|true|none|none|
|»» modified|number|true|none|none|
|»» modifiedBy|string|false|none|none|
|»» name|string|true|none|none|
|»» packId|string|false|none|none|
|»» refreshRate|number|false|none|none|
|»» resolvedDatasetIds|[string]|false|none|none|
|»» schedule|[SavedQuerySchedule](#schemasavedqueryschedule)|false|none|none|
|»»» cronSchedule|string|true|none|none|
|»»» enabled|boolean|true|none|none|
|»»» keepLastN|number|true|none|none|
|»»» notifications|object|false|none|none|
|»»»» disabled|boolean|true|none|none|
|»»»» items|[[Notification](#schemanotification)]|false|none|none|
|»»»»» id|string|true|none|none|
|»»»»» disabled|boolean|false|none|none|
|»»»»» condition|string|true|none|none|
|»»»»» targets|[string]|false|none|Targets to send any Notifications to|
|»»»»» targetConfigs|[anyOf]|false|none|none|
|»»»»»» id|string|true|none|none|
|»»»»» conf|object|false|none|none|
|»»»»» metadata|[object]|false|none|Fields to add to events from this input|
|»»»»»» name|string|true|none|none|
|»»»»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»»» resumeMissed|boolean|false|none|none|
|»»» resumeOnBoot|boolean|false|none|none|
|»»» tz|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|autoApplyMode|metric|
|autoApplyMode|all|
|autoApplyMode|off|
|autoApplyUrlSync|push|
|autoApplyUrlSync|replace|
|autoApplyUrlSync|off|
|runMode|newSearch|
|runMode|lastRun|
|type|saved|
|expectedOutputType|range|
|expectedOutputType|instant|
|type|inline|
|type|values|
|type|empty|
|expectedOutputType|range|
|expectedOutputType|instant|
|type|metric|
|type|chart.area|
|type|chart.column|
|type|chart.funnel|
|type|chart.gauge|
|type|chart.horizontalBar|
|type|chart.line|
|type|chart.map|
|type|chart.pie|
|type|chart.scatter|
|type|counter.single|
|type|list.events|
|type|list.table|
|type|custom.throughputMetrics|
|type|custom.flowMatrix|
|variant|visualization|
|type|input.timerange|
|type|input.dropdown|
|type|input.text|
|type|input.number|
|variant|input|
|type|markdown.copilot|
|type|markdown.default|
|variant|markdown|

<aside class="success">
This operation does not require authentication
</aside>

## Create SearchDashboard

<a id="opIdcreateSearchDashboard"></a>

> Code samples

`POST /search/dashboards`

Create SearchDashboard

> Body parameter

```json
{
  "autoApplyDebounceMs": 0,
  "autoApplyMode": "metric",
  "autoApplyUrlSync": "push",
  "cacheTTLSeconds": 0,
  "category": "string",
  "created": 0,
  "createdBy": "string",
  "description": "string",
  "displayCreatedBy": "string",
  "displayModifiedBy": "string",
  "elements": [
    {
      "config": {},
      "description": "string",
      "empty": true,
      "group": "string",
      "hidePanel": true,
      "horizontalChart": true,
      "id": "string",
      "index": 0,
      "layout": {
        "h": 0,
        "w": 0,
        "x": 0,
        "y": 0
      },
      "search": {
        "query": "string",
        "queryId": "string",
        "runMode": "newSearch",
        "type": "saved"
      },
      "title": "string",
      "titleAction": {
        "label": "string",
        "openInNewTab": true,
        "url": "string"
      },
      "type": "chart.area",
      "variant": "visualization"
    }
  ],
  "groups": {
    "property1": {
      "action": {
        "label": "string",
        "params": {
          "property1": "string",
          "property2": "string"
        },
        "target": "string"
      },
      "collapsed": true,
      "inputId": "string",
      "title": "string"
    },
    "property2": {
      "action": {
        "label": "string",
        "params": {
          "property1": "string",
          "property2": "string"
        },
        "target": "string"
      },
      "collapsed": true,
      "inputId": "string",
      "title": "string"
    }
  },
  "id": "string",
  "modified": 0,
  "modifiedBy": "string",
  "name": "string",
  "packId": "string",
  "refreshRate": 0,
  "resolvedDatasetIds": [
    "string"
  ],
  "schedule": {
    "cronSchedule": "string",
    "enabled": true,
    "keepLastN": 0,
    "notifications": {
      "disabled": true,
      "items": [
        {
          "id": "string",
          "disabled": false,
          "condition": "string",
          "targets": [],
          "targetConfigs": [
            {
              "id": "string",
              "conf": {
                "subject": "string",
                "body": "string",
                "emailRecipient": {
                  "to": "string",
                  "cc": "string",
                  "bcc": "string"
                }
              }
            }
          ],
          "conf": {},
          "metadata": [
            {
              "name": "string",
              "value": "string"
            }
          ]
        }
      ]
    },
    "resumeMissed": true,
    "resumeOnBoot": true,
    "tz": "string"
  }
}
```

<h3 id="create-searchdashboard-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[SearchDashboard](#schemasearchdashboard)|true|New SearchDashboard object|
|» autoApplyDebounceMs|body|number|false|none|
|» autoApplyMode|body|string|false|none|
|» autoApplyUrlSync|body|string|false|none|
|» cacheTTLSeconds|body|number|false|none|
|» category|body|string|false|none|
|» created|body|number|true|none|
|» createdBy|body|string|true|none|
|» description|body|string|false|none|
|» displayCreatedBy|body|string|false|none|
|» displayModifiedBy|body|string|false|none|
|» elements|body|[oneOf]|true|none|
|»» *anonymous*|body|object|false|none|
|»»» config|body|[ElementConfigType](#schemaelementconfigtype)|false|none|
|»»» description|body|string|false|none|
|»»» empty|body|boolean|false|none|
|»»» group|body|string|false|none|
|»»» hidePanel|body|boolean|false|none|
|»»» horizontalChart|body|boolean|false|none|
|»»» id|body|string|true|none|
|»»» index|body|number|false|none|
|»»» layout|body|[DashboardLayout](#schemadashboardlayout)|true|none|
|»»»» h|body|number|true|none|
|»»»» w|body|number|true|none|
|»»»» x|body|number|true|none|
|»»»» y|body|number|true|none|
|»»» search|body|any|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» query|body|string|false|none|
|»»»»» queryId|body|string|true|none|
|»»»»» runMode|body|[SavesSearchRunMode](#schemasavessearchrunmode)|false|none|
|»»»»» type|body|string|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» earliest|body|any|true|none|
|»»»»»» *anonymous*|body|string|false|none|
|»»»»»» *anonymous*|body|number|false|none|
|»»»»» expectedOutputType|body|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|
|»»»»» latest|body|any|true|none|
|»»»»»» *anonymous*|body|string|false|none|
|»»»»»» *anonymous*|body|number|false|none|
|»»»»» parentSearchId|body|string|false|none|
|»»»»» query|body|string|true|none|
|»»»»» sampleRate|body|number|false|none|
|»»»»» timezone|body|string|false|none|
|»»»»» type|body|string|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» type|body|string|true|none|
|»»»»» values|body|[string]|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» type|body|string|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» earliest|body|any|true|none|
|»»»»»» *anonymous*|body|string|false|none|
|»»»»»» *anonymous*|body|number|false|none|
|»»»»» expectedOutputType|body|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|
|»»»»» latest|body|any|true|none|
|»»»»»» *anonymous*|body|string|false|none|
|»»»»»» *anonymous*|body|number|false|none|
|»»»»» queries|body|[[PanelQueryDefinition](#schemapanelquerydefinition)]|true|none|
|»»»»»» alias|body|string|false|none|
|»»»»»» localId|body|string|true|none|
|»»»»»» query|body|string|true|none|
|»»»»» timezone|body|string|false|none|
|»»»»» type|body|string|true|none|
|»»» title|body|string|false|none|
|»»» titleAction|body|[TitleAction](#schematitleaction)|false|none|
|»»»» label|body|string|true|none|
|»»»» openInNewTab|body|boolean|false|none|
|»»»» url|body|string|true|none|
|»»» type|body|[VisualizationElementType](#schemavisualizationelementtype)|true|none|
|»»» variant|body|string|false|none|
|»» *anonymous*|body|object|false|none|
|»»» description|body|string|false|none|
|»»» empty|body|boolean|false|none|
|»»» group|body|string|false|none|
|»»» hidePanel|body|boolean|false|none|
|»»» horizontalChart|body|boolean|false|none|
|»»» id|body|string|true|none|
|»»» index|body|number|false|none|
|»»» inputId|body|string|true|none|
|»»» layout|body|[DashboardLayout](#schemadashboardlayout)|true|none|
|»»» search|body|any|false|none|
|»»» title|body|string|false|none|
|»»» titleAction|body|[TitleAction](#schematitleaction)|false|none|
|»»» type|body|[InputElementType](#schemainputelementtype)|true|none|
|»»» value|body|object|false|none|
|»»» variant|body|string|false|none|
|»» *anonymous*|body|object|false|none|
|»»» config|body|[MarkdownElementConfig](#schemamarkdownelementconfig)|false|none|
|»»»» markdown|body|string|true|none|
|»»» description|body|string|false|none|
|»»» empty|body|boolean|false|none|
|»»» group|body|string|false|none|
|»»» hidePanel|body|boolean|false|none|
|»»» horizontalChart|body|boolean|false|none|
|»»» id|body|string|true|none|
|»»» index|body|number|false|none|
|»»» layout|body|[DashboardLayout](#schemadashboardlayout)|true|none|
|»»» title|body|string|false|none|
|»»» titleAction|body|[TitleAction](#schematitleaction)|false|none|
|»»» type|body|[MarkdownElementType](#schemamarkdownelementtype)|true|none|
|»»» variant|body|string|true|none|
|» groups|body|[DashboardGroups](#schemadashboardgroups)|false|none|
|»» **additionalProperties**|body|object|false|none|
|»»» action|body|object|false|none|
|»»»» label|body|string|true|none|
|»»»» params|body|object|false|none|
|»»»»» **additionalProperties**|body|string|false|none|
|»»»» target|body|string|true|none|
|»»» collapsed|body|boolean|false|none|
|»»» inputId|body|string|false|none|
|»»» title|body|string|true|none|
|» id|body|string|true|none|
|» modified|body|number|true|none|
|» modifiedBy|body|string|false|none|
|» name|body|string|true|none|
|» packId|body|string|false|none|
|» refreshRate|body|number|false|none|
|» resolvedDatasetIds|body|[string]|false|none|
|» schedule|body|[SavedQuerySchedule](#schemasavedqueryschedule)|false|none|
|»» cronSchedule|body|string|true|none|
|»» enabled|body|boolean|true|none|
|»» keepLastN|body|number|true|none|
|»» notifications|body|object|false|none|
|»»» disabled|body|boolean|true|none|
|»»» items|body|[[Notification](#schemanotification)]|false|none|
|»»»» id|body|string|true|none|
|»»»» disabled|body|boolean|false|none|
|»»»» condition|body|string|true|none|
|»»»» targets|body|[string]|false|Targets to send any Notifications to|
|»»»» targetConfigs|body|[anyOf]|false|none|
|»»»»» id|body|string|true|none|
|»»»» conf|body|object|false|none|
|»»»» metadata|body|[object]|false|Fields to add to events from this input|
|»»»»» name|body|string|true|none|
|»»»»» value|body|string|true|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»» resumeMissed|body|boolean|false|none|
|»» resumeOnBoot|body|boolean|false|none|
|»» tz|body|string|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» autoApplyMode|metric|
|» autoApplyMode|all|
|» autoApplyMode|off|
|» autoApplyUrlSync|push|
|» autoApplyUrlSync|replace|
|» autoApplyUrlSync|off|
|»»»»» runMode|newSearch|
|»»»»» runMode|lastRun|
|»»»»» type|saved|
|»»»»» expectedOutputType|range|
|»»»»» expectedOutputType|instant|
|»»»»» type|inline|
|»»»»» type|values|
|»»»»» type|empty|
|»»»»» expectedOutputType|range|
|»»»»» expectedOutputType|instant|
|»»»»» type|metric|
|»»» type|chart.area|
|»»» type|chart.column|
|»»» type|chart.funnel|
|»»» type|chart.gauge|
|»»» type|chart.horizontalBar|
|»»» type|chart.line|
|»»» type|chart.map|
|»»» type|chart.pie|
|»»» type|chart.scatter|
|»»» type|counter.single|
|»»» type|list.events|
|»»» type|list.table|
|»»» type|custom.throughputMetrics|
|»»» type|custom.flowMatrix|
|»»» variant|visualization|
|»»» type|input.timerange|
|»»» type|input.dropdown|
|»»» type|input.text|
|»»» type|input.number|
|»»» variant|input|
|»»» type|markdown.copilot|
|»»» type|markdown.default|
|»»» variant|markdown|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "autoApplyDebounceMs": 0,
      "autoApplyMode": "metric",
      "autoApplyUrlSync": "push",
      "cacheTTLSeconds": 0,
      "category": "string",
      "created": 0,
      "createdBy": "string",
      "description": "string",
      "displayCreatedBy": "string",
      "displayModifiedBy": "string",
      "elements": [
        {
          "config": {},
          "description": "string",
          "empty": true,
          "group": "string",
          "hidePanel": true,
          "horizontalChart": true,
          "id": "string",
          "index": 0,
          "layout": {
            "h": 0,
            "w": 0,
            "x": 0,
            "y": 0
          },
          "search": {
            "query": "string",
            "queryId": "string",
            "runMode": "newSearch",
            "type": "saved"
          },
          "title": "string",
          "titleAction": {
            "label": "string",
            "openInNewTab": true,
            "url": "string"
          },
          "type": "chart.area",
          "variant": "visualization"
        }
      ],
      "groups": {
        "property1": {
          "action": {
            "label": "string",
            "params": {
              "property1": "string",
              "property2": "string"
            },
            "target": "string"
          },
          "collapsed": true,
          "inputId": "string",
          "title": "string"
        },
        "property2": {
          "action": {
            "label": "string",
            "params": {
              "property1": "string",
              "property2": "string"
            },
            "target": "string"
          },
          "collapsed": true,
          "inputId": "string",
          "title": "string"
        }
      },
      "id": "string",
      "modified": 0,
      "modifiedBy": "string",
      "name": "string",
      "packId": "string",
      "refreshRate": 0,
      "resolvedDatasetIds": [
        "string"
      ],
      "schedule": {
        "cronSchedule": "string",
        "enabled": true,
        "keepLastN": 0,
        "notifications": {
          "disabled": true,
          "items": [
            {
              "id": "string",
              "disabled": false,
              "condition": "string",
              "targets": [],
              "targetConfigs": [
                {
                  "id": "string",
                  "conf": {
                    "subject": "string",
                    "body": "string",
                    "emailRecipient": {
                      "to": "string",
                      "cc": "string",
                      "bcc": "string"
                    }
                  }
                }
              ],
              "conf": {},
              "metadata": [
                {
                  "name": "string",
                  "value": "string"
                }
              ]
            }
          ]
        },
        "resumeMissed": true,
        "resumeOnBoot": true,
        "tz": "string"
      }
    }
  ]
}
```

<h3 id="create-searchdashboard-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchDashboard objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-searchdashboard-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchDashboard](#schemasearchdashboard)]|false|none|none|
|»» autoApplyDebounceMs|number|false|none|none|
|»» autoApplyMode|string|false|none|none|
|»» autoApplyUrlSync|string|false|none|none|
|»» cacheTTLSeconds|number|false|none|none|
|»» category|string|false|none|none|
|»» created|number|true|none|none|
|»» createdBy|string|true|none|none|
|»» description|string|false|none|none|
|»» displayCreatedBy|string|false|none|none|
|»» displayModifiedBy|string|false|none|none|
|»» elements|[oneOf]|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» config|[ElementConfigType](#schemaelementconfigtype)|false|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»»» h|number|true|none|none|
|»»»»» w|number|true|none|none|
|»»»»» x|number|true|none|none|
|»»»»» y|number|true|none|none|
|»»»» search|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» query|string|false|none|none|
|»»»»»» queryId|string|true|none|none|
|»»»»»» runMode|[SavesSearchRunMode](#schemasavessearchrunmode)|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» earliest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» expectedOutputType|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|none|
|»»»»»» latest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» parentSearchId|string|false|none|none|
|»»»»»» query|string|true|none|none|
|»»»»»» sampleRate|number|false|none|none|
|»»»»»» timezone|string|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» type|string|true|none|none|
|»»»»»» values|[string]|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» earliest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» expectedOutputType|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|none|
|»»»»»» latest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» queries|[[PanelQueryDefinition](#schemapanelquerydefinition)]|true|none|none|
|»»»»»»» alias|string|false|none|none|
|»»»»»»» localId|string|true|none|none|
|»»»»»»» query|string|true|none|none|
|»»»»»» timezone|string|false|none|none|
|»»»»»» type|string|true|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»»» label|string|true|none|none|
|»»»»» openInNewTab|boolean|false|none|none|
|»»»»» url|string|true|none|none|
|»»»» type|[VisualizationElementType](#schemavisualizationelementtype)|true|none|none|
|»»»» variant|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» inputId|string|true|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»» search|any|false|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»» type|[InputElementType](#schemainputelementtype)|true|none|none|
|»»»» value|object|false|none|none|
|»»»» variant|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» config|[MarkdownElementConfig](#schemamarkdownelementconfig)|false|none|none|
|»»»»» markdown|string|true|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»» type|[MarkdownElementType](#schemamarkdownelementtype)|true|none|none|
|»»»» variant|string|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» groups|[DashboardGroups](#schemadashboardgroups)|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» action|object|false|none|none|
|»»»»» label|string|true|none|none|
|»»»»» params|object|false|none|none|
|»»»»»» **additionalProperties**|string|false|none|none|
|»»»»» target|string|true|none|none|
|»»»» collapsed|boolean|false|none|none|
|»»»» inputId|string|false|none|none|
|»»»» title|string|true|none|none|
|»» id|string|true|none|none|
|»» modified|number|true|none|none|
|»» modifiedBy|string|false|none|none|
|»» name|string|true|none|none|
|»» packId|string|false|none|none|
|»» refreshRate|number|false|none|none|
|»» resolvedDatasetIds|[string]|false|none|none|
|»» schedule|[SavedQuerySchedule](#schemasavedqueryschedule)|false|none|none|
|»»» cronSchedule|string|true|none|none|
|»»» enabled|boolean|true|none|none|
|»»» keepLastN|number|true|none|none|
|»»» notifications|object|false|none|none|
|»»»» disabled|boolean|true|none|none|
|»»»» items|[[Notification](#schemanotification)]|false|none|none|
|»»»»» id|string|true|none|none|
|»»»»» disabled|boolean|false|none|none|
|»»»»» condition|string|true|none|none|
|»»»»» targets|[string]|false|none|Targets to send any Notifications to|
|»»»»» targetConfigs|[anyOf]|false|none|none|
|»»»»»» id|string|true|none|none|
|»»»»» conf|object|false|none|none|
|»»»»» metadata|[object]|false|none|Fields to add to events from this input|
|»»»»»» name|string|true|none|none|
|»»»»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»»» resumeMissed|boolean|false|none|none|
|»»» resumeOnBoot|boolean|false|none|none|
|»»» tz|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|autoApplyMode|metric|
|autoApplyMode|all|
|autoApplyMode|off|
|autoApplyUrlSync|push|
|autoApplyUrlSync|replace|
|autoApplyUrlSync|off|
|runMode|newSearch|
|runMode|lastRun|
|type|saved|
|expectedOutputType|range|
|expectedOutputType|instant|
|type|inline|
|type|values|
|type|empty|
|expectedOutputType|range|
|expectedOutputType|instant|
|type|metric|
|type|chart.area|
|type|chart.column|
|type|chart.funnel|
|type|chart.gauge|
|type|chart.horizontalBar|
|type|chart.line|
|type|chart.map|
|type|chart.pie|
|type|chart.scatter|
|type|counter.single|
|type|list.events|
|type|list.table|
|type|custom.throughputMetrics|
|type|custom.flowMatrix|
|variant|visualization|
|type|input.timerange|
|type|input.dropdown|
|type|input.text|
|type|input.number|
|variant|input|
|type|markdown.copilot|
|type|markdown.default|
|variant|markdown|

<aside class="success">
This operation does not require authentication
</aside>

## Delete SearchDashboard

<a id="opIddeleteSearchDashboardById"></a>

> Code samples

`DELETE /search/dashboards/{id}`

Delete SearchDashboard

<h3 id="delete-searchdashboard-parameters">Parameters</h3>

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
      "autoApplyDebounceMs": 0,
      "autoApplyMode": "metric",
      "autoApplyUrlSync": "push",
      "cacheTTLSeconds": 0,
      "category": "string",
      "created": 0,
      "createdBy": "string",
      "description": "string",
      "displayCreatedBy": "string",
      "displayModifiedBy": "string",
      "elements": [
        {
          "config": {},
          "description": "string",
          "empty": true,
          "group": "string",
          "hidePanel": true,
          "horizontalChart": true,
          "id": "string",
          "index": 0,
          "layout": {
            "h": 0,
            "w": 0,
            "x": 0,
            "y": 0
          },
          "search": {
            "query": "string",
            "queryId": "string",
            "runMode": "newSearch",
            "type": "saved"
          },
          "title": "string",
          "titleAction": {
            "label": "string",
            "openInNewTab": true,
            "url": "string"
          },
          "type": "chart.area",
          "variant": "visualization"
        }
      ],
      "groups": {
        "property1": {
          "action": {
            "label": "string",
            "params": {
              "property1": "string",
              "property2": "string"
            },
            "target": "string"
          },
          "collapsed": true,
          "inputId": "string",
          "title": "string"
        },
        "property2": {
          "action": {
            "label": "string",
            "params": {
              "property1": "string",
              "property2": "string"
            },
            "target": "string"
          },
          "collapsed": true,
          "inputId": "string",
          "title": "string"
        }
      },
      "id": "string",
      "modified": 0,
      "modifiedBy": "string",
      "name": "string",
      "packId": "string",
      "refreshRate": 0,
      "resolvedDatasetIds": [
        "string"
      ],
      "schedule": {
        "cronSchedule": "string",
        "enabled": true,
        "keepLastN": 0,
        "notifications": {
          "disabled": true,
          "items": [
            {
              "id": "string",
              "disabled": false,
              "condition": "string",
              "targets": [],
              "targetConfigs": [
                {
                  "id": "string",
                  "conf": {
                    "subject": "string",
                    "body": "string",
                    "emailRecipient": {
                      "to": "string",
                      "cc": "string",
                      "bcc": "string"
                    }
                  }
                }
              ],
              "conf": {},
              "metadata": [
                {
                  "name": "string",
                  "value": "string"
                }
              ]
            }
          ]
        },
        "resumeMissed": true,
        "resumeOnBoot": true,
        "tz": "string"
      }
    }
  ]
}
```

<h3 id="delete-searchdashboard-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchDashboard objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-searchdashboard-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchDashboard](#schemasearchdashboard)]|false|none|none|
|»» autoApplyDebounceMs|number|false|none|none|
|»» autoApplyMode|string|false|none|none|
|»» autoApplyUrlSync|string|false|none|none|
|»» cacheTTLSeconds|number|false|none|none|
|»» category|string|false|none|none|
|»» created|number|true|none|none|
|»» createdBy|string|true|none|none|
|»» description|string|false|none|none|
|»» displayCreatedBy|string|false|none|none|
|»» displayModifiedBy|string|false|none|none|
|»» elements|[oneOf]|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» config|[ElementConfigType](#schemaelementconfigtype)|false|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»»» h|number|true|none|none|
|»»»»» w|number|true|none|none|
|»»»»» x|number|true|none|none|
|»»»»» y|number|true|none|none|
|»»»» search|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» query|string|false|none|none|
|»»»»»» queryId|string|true|none|none|
|»»»»»» runMode|[SavesSearchRunMode](#schemasavessearchrunmode)|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» earliest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» expectedOutputType|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|none|
|»»»»»» latest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» parentSearchId|string|false|none|none|
|»»»»»» query|string|true|none|none|
|»»»»»» sampleRate|number|false|none|none|
|»»»»»» timezone|string|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» type|string|true|none|none|
|»»»»»» values|[string]|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» earliest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» expectedOutputType|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|none|
|»»»»»» latest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» queries|[[PanelQueryDefinition](#schemapanelquerydefinition)]|true|none|none|
|»»»»»»» alias|string|false|none|none|
|»»»»»»» localId|string|true|none|none|
|»»»»»»» query|string|true|none|none|
|»»»»»» timezone|string|false|none|none|
|»»»»»» type|string|true|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»»» label|string|true|none|none|
|»»»»» openInNewTab|boolean|false|none|none|
|»»»»» url|string|true|none|none|
|»»»» type|[VisualizationElementType](#schemavisualizationelementtype)|true|none|none|
|»»»» variant|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» inputId|string|true|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»» search|any|false|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»» type|[InputElementType](#schemainputelementtype)|true|none|none|
|»»»» value|object|false|none|none|
|»»»» variant|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» config|[MarkdownElementConfig](#schemamarkdownelementconfig)|false|none|none|
|»»»»» markdown|string|true|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»» type|[MarkdownElementType](#schemamarkdownelementtype)|true|none|none|
|»»»» variant|string|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» groups|[DashboardGroups](#schemadashboardgroups)|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» action|object|false|none|none|
|»»»»» label|string|true|none|none|
|»»»»» params|object|false|none|none|
|»»»»»» **additionalProperties**|string|false|none|none|
|»»»»» target|string|true|none|none|
|»»»» collapsed|boolean|false|none|none|
|»»»» inputId|string|false|none|none|
|»»»» title|string|true|none|none|
|»» id|string|true|none|none|
|»» modified|number|true|none|none|
|»» modifiedBy|string|false|none|none|
|»» name|string|true|none|none|
|»» packId|string|false|none|none|
|»» refreshRate|number|false|none|none|
|»» resolvedDatasetIds|[string]|false|none|none|
|»» schedule|[SavedQuerySchedule](#schemasavedqueryschedule)|false|none|none|
|»»» cronSchedule|string|true|none|none|
|»»» enabled|boolean|true|none|none|
|»»» keepLastN|number|true|none|none|
|»»» notifications|object|false|none|none|
|»»»» disabled|boolean|true|none|none|
|»»»» items|[[Notification](#schemanotification)]|false|none|none|
|»»»»» id|string|true|none|none|
|»»»»» disabled|boolean|false|none|none|
|»»»»» condition|string|true|none|none|
|»»»»» targets|[string]|false|none|Targets to send any Notifications to|
|»»»»» targetConfigs|[anyOf]|false|none|none|
|»»»»»» id|string|true|none|none|
|»»»»» conf|object|false|none|none|
|»»»»» metadata|[object]|false|none|Fields to add to events from this input|
|»»»»»» name|string|true|none|none|
|»»»»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»»» resumeMissed|boolean|false|none|none|
|»»» resumeOnBoot|boolean|false|none|none|
|»»» tz|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|autoApplyMode|metric|
|autoApplyMode|all|
|autoApplyMode|off|
|autoApplyUrlSync|push|
|autoApplyUrlSync|replace|
|autoApplyUrlSync|off|
|runMode|newSearch|
|runMode|lastRun|
|type|saved|
|expectedOutputType|range|
|expectedOutputType|instant|
|type|inline|
|type|values|
|type|empty|
|expectedOutputType|range|
|expectedOutputType|instant|
|type|metric|
|type|chart.area|
|type|chart.column|
|type|chart.funnel|
|type|chart.gauge|
|type|chart.horizontalBar|
|type|chart.line|
|type|chart.map|
|type|chart.pie|
|type|chart.scatter|
|type|counter.single|
|type|list.events|
|type|list.table|
|type|custom.throughputMetrics|
|type|custom.flowMatrix|
|variant|visualization|
|type|input.timerange|
|type|input.dropdown|
|type|input.text|
|type|input.number|
|variant|input|
|type|markdown.copilot|
|type|markdown.default|
|variant|markdown|

<aside class="success">
This operation does not require authentication
</aside>

## Get SearchDashboard by ID

<a id="opIdgetSearchDashboardById"></a>

> Code samples

`GET /search/dashboards/{id}`

Get SearchDashboard by ID

<h3 id="get-searchdashboard-by-id-parameters">Parameters</h3>

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
      "autoApplyDebounceMs": 0,
      "autoApplyMode": "metric",
      "autoApplyUrlSync": "push",
      "cacheTTLSeconds": 0,
      "category": "string",
      "created": 0,
      "createdBy": "string",
      "description": "string",
      "displayCreatedBy": "string",
      "displayModifiedBy": "string",
      "elements": [
        {
          "config": {},
          "description": "string",
          "empty": true,
          "group": "string",
          "hidePanel": true,
          "horizontalChart": true,
          "id": "string",
          "index": 0,
          "layout": {
            "h": 0,
            "w": 0,
            "x": 0,
            "y": 0
          },
          "search": {
            "query": "string",
            "queryId": "string",
            "runMode": "newSearch",
            "type": "saved"
          },
          "title": "string",
          "titleAction": {
            "label": "string",
            "openInNewTab": true,
            "url": "string"
          },
          "type": "chart.area",
          "variant": "visualization"
        }
      ],
      "groups": {
        "property1": {
          "action": {
            "label": "string",
            "params": {
              "property1": "string",
              "property2": "string"
            },
            "target": "string"
          },
          "collapsed": true,
          "inputId": "string",
          "title": "string"
        },
        "property2": {
          "action": {
            "label": "string",
            "params": {
              "property1": "string",
              "property2": "string"
            },
            "target": "string"
          },
          "collapsed": true,
          "inputId": "string",
          "title": "string"
        }
      },
      "id": "string",
      "modified": 0,
      "modifiedBy": "string",
      "name": "string",
      "packId": "string",
      "refreshRate": 0,
      "resolvedDatasetIds": [
        "string"
      ],
      "schedule": {
        "cronSchedule": "string",
        "enabled": true,
        "keepLastN": 0,
        "notifications": {
          "disabled": true,
          "items": [
            {
              "id": "string",
              "disabled": false,
              "condition": "string",
              "targets": [],
              "targetConfigs": [
                {
                  "id": "string",
                  "conf": {
                    "subject": "string",
                    "body": "string",
                    "emailRecipient": {
                      "to": "string",
                      "cc": "string",
                      "bcc": "string"
                    }
                  }
                }
              ],
              "conf": {},
              "metadata": [
                {
                  "name": "string",
                  "value": "string"
                }
              ]
            }
          ]
        },
        "resumeMissed": true,
        "resumeOnBoot": true,
        "tz": "string"
      }
    }
  ]
}
```

<h3 id="get-searchdashboard-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchDashboard objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-searchdashboard-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchDashboard](#schemasearchdashboard)]|false|none|none|
|»» autoApplyDebounceMs|number|false|none|none|
|»» autoApplyMode|string|false|none|none|
|»» autoApplyUrlSync|string|false|none|none|
|»» cacheTTLSeconds|number|false|none|none|
|»» category|string|false|none|none|
|»» created|number|true|none|none|
|»» createdBy|string|true|none|none|
|»» description|string|false|none|none|
|»» displayCreatedBy|string|false|none|none|
|»» displayModifiedBy|string|false|none|none|
|»» elements|[oneOf]|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» config|[ElementConfigType](#schemaelementconfigtype)|false|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»»» h|number|true|none|none|
|»»»»» w|number|true|none|none|
|»»»»» x|number|true|none|none|
|»»»»» y|number|true|none|none|
|»»»» search|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» query|string|false|none|none|
|»»»»»» queryId|string|true|none|none|
|»»»»»» runMode|[SavesSearchRunMode](#schemasavessearchrunmode)|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» earliest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» expectedOutputType|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|none|
|»»»»»» latest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» parentSearchId|string|false|none|none|
|»»»»»» query|string|true|none|none|
|»»»»»» sampleRate|number|false|none|none|
|»»»»»» timezone|string|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» type|string|true|none|none|
|»»»»»» values|[string]|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» earliest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» expectedOutputType|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|none|
|»»»»»» latest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» queries|[[PanelQueryDefinition](#schemapanelquerydefinition)]|true|none|none|
|»»»»»»» alias|string|false|none|none|
|»»»»»»» localId|string|true|none|none|
|»»»»»»» query|string|true|none|none|
|»»»»»» timezone|string|false|none|none|
|»»»»»» type|string|true|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»»» label|string|true|none|none|
|»»»»» openInNewTab|boolean|false|none|none|
|»»»»» url|string|true|none|none|
|»»»» type|[VisualizationElementType](#schemavisualizationelementtype)|true|none|none|
|»»»» variant|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» inputId|string|true|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»» search|any|false|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»» type|[InputElementType](#schemainputelementtype)|true|none|none|
|»»»» value|object|false|none|none|
|»»»» variant|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» config|[MarkdownElementConfig](#schemamarkdownelementconfig)|false|none|none|
|»»»»» markdown|string|true|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»» type|[MarkdownElementType](#schemamarkdownelementtype)|true|none|none|
|»»»» variant|string|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» groups|[DashboardGroups](#schemadashboardgroups)|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» action|object|false|none|none|
|»»»»» label|string|true|none|none|
|»»»»» params|object|false|none|none|
|»»»»»» **additionalProperties**|string|false|none|none|
|»»»»» target|string|true|none|none|
|»»»» collapsed|boolean|false|none|none|
|»»»» inputId|string|false|none|none|
|»»»» title|string|true|none|none|
|»» id|string|true|none|none|
|»» modified|number|true|none|none|
|»» modifiedBy|string|false|none|none|
|»» name|string|true|none|none|
|»» packId|string|false|none|none|
|»» refreshRate|number|false|none|none|
|»» resolvedDatasetIds|[string]|false|none|none|
|»» schedule|[SavedQuerySchedule](#schemasavedqueryschedule)|false|none|none|
|»»» cronSchedule|string|true|none|none|
|»»» enabled|boolean|true|none|none|
|»»» keepLastN|number|true|none|none|
|»»» notifications|object|false|none|none|
|»»»» disabled|boolean|true|none|none|
|»»»» items|[[Notification](#schemanotification)]|false|none|none|
|»»»»» id|string|true|none|none|
|»»»»» disabled|boolean|false|none|none|
|»»»»» condition|string|true|none|none|
|»»»»» targets|[string]|false|none|Targets to send any Notifications to|
|»»»»» targetConfigs|[anyOf]|false|none|none|
|»»»»»» id|string|true|none|none|
|»»»»» conf|object|false|none|none|
|»»»»» metadata|[object]|false|none|Fields to add to events from this input|
|»»»»»» name|string|true|none|none|
|»»»»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»»» resumeMissed|boolean|false|none|none|
|»»» resumeOnBoot|boolean|false|none|none|
|»»» tz|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|autoApplyMode|metric|
|autoApplyMode|all|
|autoApplyMode|off|
|autoApplyUrlSync|push|
|autoApplyUrlSync|replace|
|autoApplyUrlSync|off|
|runMode|newSearch|
|runMode|lastRun|
|type|saved|
|expectedOutputType|range|
|expectedOutputType|instant|
|type|inline|
|type|values|
|type|empty|
|expectedOutputType|range|
|expectedOutputType|instant|
|type|metric|
|type|chart.area|
|type|chart.column|
|type|chart.funnel|
|type|chart.gauge|
|type|chart.horizontalBar|
|type|chart.line|
|type|chart.map|
|type|chart.pie|
|type|chart.scatter|
|type|counter.single|
|type|list.events|
|type|list.table|
|type|custom.throughputMetrics|
|type|custom.flowMatrix|
|variant|visualization|
|type|input.timerange|
|type|input.dropdown|
|type|input.text|
|type|input.number|
|variant|input|
|type|markdown.copilot|
|type|markdown.default|
|variant|markdown|

<aside class="success">
This operation does not require authentication
</aside>

## Update SearchDashboard

<a id="opIdupdateSearchDashboardById"></a>

> Code samples

`PATCH /search/dashboards/{id}`

Update SearchDashboard

> Body parameter

```json
{
  "autoApplyDebounceMs": 0,
  "autoApplyMode": "metric",
  "autoApplyUrlSync": "push",
  "cacheTTLSeconds": 0,
  "category": "string",
  "created": 0,
  "createdBy": "string",
  "description": "string",
  "displayCreatedBy": "string",
  "displayModifiedBy": "string",
  "elements": [
    {
      "config": {},
      "description": "string",
      "empty": true,
      "group": "string",
      "hidePanel": true,
      "horizontalChart": true,
      "id": "string",
      "index": 0,
      "layout": {
        "h": 0,
        "w": 0,
        "x": 0,
        "y": 0
      },
      "search": {
        "query": "string",
        "queryId": "string",
        "runMode": "newSearch",
        "type": "saved"
      },
      "title": "string",
      "titleAction": {
        "label": "string",
        "openInNewTab": true,
        "url": "string"
      },
      "type": "chart.area",
      "variant": "visualization"
    }
  ],
  "groups": {
    "property1": {
      "action": {
        "label": "string",
        "params": {
          "property1": "string",
          "property2": "string"
        },
        "target": "string"
      },
      "collapsed": true,
      "inputId": "string",
      "title": "string"
    },
    "property2": {
      "action": {
        "label": "string",
        "params": {
          "property1": "string",
          "property2": "string"
        },
        "target": "string"
      },
      "collapsed": true,
      "inputId": "string",
      "title": "string"
    }
  },
  "id": "string",
  "modified": 0,
  "modifiedBy": "string",
  "name": "string",
  "packId": "string",
  "refreshRate": 0,
  "resolvedDatasetIds": [
    "string"
  ],
  "schedule": {
    "cronSchedule": "string",
    "enabled": true,
    "keepLastN": 0,
    "notifications": {
      "disabled": true,
      "items": [
        {
          "id": "string",
          "disabled": false,
          "condition": "string",
          "targets": [],
          "targetConfigs": [
            {
              "id": "string",
              "conf": {
                "subject": "string",
                "body": "string",
                "emailRecipient": {
                  "to": "string",
                  "cc": "string",
                  "bcc": "string"
                }
              }
            }
          ],
          "conf": {},
          "metadata": [
            {
              "name": "string",
              "value": "string"
            }
          ]
        }
      ]
    },
    "resumeMissed": true,
    "resumeOnBoot": true,
    "tz": "string"
  }
}
```

<h3 id="update-searchdashboard-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[SearchDashboard](#schemasearchdashboard)|true|SearchDashboard object to be updated|
|» autoApplyDebounceMs|body|number|false|none|
|» autoApplyMode|body|string|false|none|
|» autoApplyUrlSync|body|string|false|none|
|» cacheTTLSeconds|body|number|false|none|
|» category|body|string|false|none|
|» created|body|number|true|none|
|» createdBy|body|string|true|none|
|» description|body|string|false|none|
|» displayCreatedBy|body|string|false|none|
|» displayModifiedBy|body|string|false|none|
|» elements|body|[oneOf]|true|none|
|»» *anonymous*|body|object|false|none|
|»»» config|body|[ElementConfigType](#schemaelementconfigtype)|false|none|
|»»» description|body|string|false|none|
|»»» empty|body|boolean|false|none|
|»»» group|body|string|false|none|
|»»» hidePanel|body|boolean|false|none|
|»»» horizontalChart|body|boolean|false|none|
|»»» id|body|string|true|none|
|»»» index|body|number|false|none|
|»»» layout|body|[DashboardLayout](#schemadashboardlayout)|true|none|
|»»»» h|body|number|true|none|
|»»»» w|body|number|true|none|
|»»»» x|body|number|true|none|
|»»»» y|body|number|true|none|
|»»» search|body|any|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» query|body|string|false|none|
|»»»»» queryId|body|string|true|none|
|»»»»» runMode|body|[SavesSearchRunMode](#schemasavessearchrunmode)|false|none|
|»»»»» type|body|string|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» earliest|body|any|true|none|
|»»»»»» *anonymous*|body|string|false|none|
|»»»»»» *anonymous*|body|number|false|none|
|»»»»» expectedOutputType|body|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|
|»»»»» latest|body|any|true|none|
|»»»»»» *anonymous*|body|string|false|none|
|»»»»»» *anonymous*|body|number|false|none|
|»»»»» parentSearchId|body|string|false|none|
|»»»»» query|body|string|true|none|
|»»»»» sampleRate|body|number|false|none|
|»»»»» timezone|body|string|false|none|
|»»»»» type|body|string|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» type|body|string|true|none|
|»»»»» values|body|[string]|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» type|body|string|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» earliest|body|any|true|none|
|»»»»»» *anonymous*|body|string|false|none|
|»»»»»» *anonymous*|body|number|false|none|
|»»»»» expectedOutputType|body|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|
|»»»»» latest|body|any|true|none|
|»»»»»» *anonymous*|body|string|false|none|
|»»»»»» *anonymous*|body|number|false|none|
|»»»»» queries|body|[[PanelQueryDefinition](#schemapanelquerydefinition)]|true|none|
|»»»»»» alias|body|string|false|none|
|»»»»»» localId|body|string|true|none|
|»»»»»» query|body|string|true|none|
|»»»»» timezone|body|string|false|none|
|»»»»» type|body|string|true|none|
|»»» title|body|string|false|none|
|»»» titleAction|body|[TitleAction](#schematitleaction)|false|none|
|»»»» label|body|string|true|none|
|»»»» openInNewTab|body|boolean|false|none|
|»»»» url|body|string|true|none|
|»»» type|body|[VisualizationElementType](#schemavisualizationelementtype)|true|none|
|»»» variant|body|string|false|none|
|»» *anonymous*|body|object|false|none|
|»»» description|body|string|false|none|
|»»» empty|body|boolean|false|none|
|»»» group|body|string|false|none|
|»»» hidePanel|body|boolean|false|none|
|»»» horizontalChart|body|boolean|false|none|
|»»» id|body|string|true|none|
|»»» index|body|number|false|none|
|»»» inputId|body|string|true|none|
|»»» layout|body|[DashboardLayout](#schemadashboardlayout)|true|none|
|»»» search|body|any|false|none|
|»»» title|body|string|false|none|
|»»» titleAction|body|[TitleAction](#schematitleaction)|false|none|
|»»» type|body|[InputElementType](#schemainputelementtype)|true|none|
|»»» value|body|object|false|none|
|»»» variant|body|string|false|none|
|»» *anonymous*|body|object|false|none|
|»»» config|body|[MarkdownElementConfig](#schemamarkdownelementconfig)|false|none|
|»»»» markdown|body|string|true|none|
|»»» description|body|string|false|none|
|»»» empty|body|boolean|false|none|
|»»» group|body|string|false|none|
|»»» hidePanel|body|boolean|false|none|
|»»» horizontalChart|body|boolean|false|none|
|»»» id|body|string|true|none|
|»»» index|body|number|false|none|
|»»» layout|body|[DashboardLayout](#schemadashboardlayout)|true|none|
|»»» title|body|string|false|none|
|»»» titleAction|body|[TitleAction](#schematitleaction)|false|none|
|»»» type|body|[MarkdownElementType](#schemamarkdownelementtype)|true|none|
|»»» variant|body|string|true|none|
|» groups|body|[DashboardGroups](#schemadashboardgroups)|false|none|
|»» **additionalProperties**|body|object|false|none|
|»»» action|body|object|false|none|
|»»»» label|body|string|true|none|
|»»»» params|body|object|false|none|
|»»»»» **additionalProperties**|body|string|false|none|
|»»»» target|body|string|true|none|
|»»» collapsed|body|boolean|false|none|
|»»» inputId|body|string|false|none|
|»»» title|body|string|true|none|
|» id|body|string|true|none|
|» modified|body|number|true|none|
|» modifiedBy|body|string|false|none|
|» name|body|string|true|none|
|» packId|body|string|false|none|
|» refreshRate|body|number|false|none|
|» resolvedDatasetIds|body|[string]|false|none|
|» schedule|body|[SavedQuerySchedule](#schemasavedqueryschedule)|false|none|
|»» cronSchedule|body|string|true|none|
|»» enabled|body|boolean|true|none|
|»» keepLastN|body|number|true|none|
|»» notifications|body|object|false|none|
|»»» disabled|body|boolean|true|none|
|»»» items|body|[[Notification](#schemanotification)]|false|none|
|»»»» id|body|string|true|none|
|»»»» disabled|body|boolean|false|none|
|»»»» condition|body|string|true|none|
|»»»» targets|body|[string]|false|Targets to send any Notifications to|
|»»»» targetConfigs|body|[anyOf]|false|none|
|»»»»» id|body|string|true|none|
|»»»» conf|body|object|false|none|
|»»»» metadata|body|[object]|false|Fields to add to events from this input|
|»»»»» name|body|string|true|none|
|»»»»» value|body|string|true|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»» resumeMissed|body|boolean|false|none|
|»» resumeOnBoot|body|boolean|false|none|
|»» tz|body|string|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» autoApplyMode|metric|
|» autoApplyMode|all|
|» autoApplyMode|off|
|» autoApplyUrlSync|push|
|» autoApplyUrlSync|replace|
|» autoApplyUrlSync|off|
|»»»»» runMode|newSearch|
|»»»»» runMode|lastRun|
|»»»»» type|saved|
|»»»»» expectedOutputType|range|
|»»»»» expectedOutputType|instant|
|»»»»» type|inline|
|»»»»» type|values|
|»»»»» type|empty|
|»»»»» expectedOutputType|range|
|»»»»» expectedOutputType|instant|
|»»»»» type|metric|
|»»» type|chart.area|
|»»» type|chart.column|
|»»» type|chart.funnel|
|»»» type|chart.gauge|
|»»» type|chart.horizontalBar|
|»»» type|chart.line|
|»»» type|chart.map|
|»»» type|chart.pie|
|»»» type|chart.scatter|
|»»» type|counter.single|
|»»» type|list.events|
|»»» type|list.table|
|»»» type|custom.throughputMetrics|
|»»» type|custom.flowMatrix|
|»»» variant|visualization|
|»»» type|input.timerange|
|»»» type|input.dropdown|
|»»» type|input.text|
|»»» type|input.number|
|»»» variant|input|
|»»» type|markdown.copilot|
|»»» type|markdown.default|
|»»» variant|markdown|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "autoApplyDebounceMs": 0,
      "autoApplyMode": "metric",
      "autoApplyUrlSync": "push",
      "cacheTTLSeconds": 0,
      "category": "string",
      "created": 0,
      "createdBy": "string",
      "description": "string",
      "displayCreatedBy": "string",
      "displayModifiedBy": "string",
      "elements": [
        {
          "config": {},
          "description": "string",
          "empty": true,
          "group": "string",
          "hidePanel": true,
          "horizontalChart": true,
          "id": "string",
          "index": 0,
          "layout": {
            "h": 0,
            "w": 0,
            "x": 0,
            "y": 0
          },
          "search": {
            "query": "string",
            "queryId": "string",
            "runMode": "newSearch",
            "type": "saved"
          },
          "title": "string",
          "titleAction": {
            "label": "string",
            "openInNewTab": true,
            "url": "string"
          },
          "type": "chart.area",
          "variant": "visualization"
        }
      ],
      "groups": {
        "property1": {
          "action": {
            "label": "string",
            "params": {
              "property1": "string",
              "property2": "string"
            },
            "target": "string"
          },
          "collapsed": true,
          "inputId": "string",
          "title": "string"
        },
        "property2": {
          "action": {
            "label": "string",
            "params": {
              "property1": "string",
              "property2": "string"
            },
            "target": "string"
          },
          "collapsed": true,
          "inputId": "string",
          "title": "string"
        }
      },
      "id": "string",
      "modified": 0,
      "modifiedBy": "string",
      "name": "string",
      "packId": "string",
      "refreshRate": 0,
      "resolvedDatasetIds": [
        "string"
      ],
      "schedule": {
        "cronSchedule": "string",
        "enabled": true,
        "keepLastN": 0,
        "notifications": {
          "disabled": true,
          "items": [
            {
              "id": "string",
              "disabled": false,
              "condition": "string",
              "targets": [],
              "targetConfigs": [
                {
                  "id": "string",
                  "conf": {
                    "subject": "string",
                    "body": "string",
                    "emailRecipient": {
                      "to": "string",
                      "cc": "string",
                      "bcc": "string"
                    }
                  }
                }
              ],
              "conf": {},
              "metadata": [
                {
                  "name": "string",
                  "value": "string"
                }
              ]
            }
          ]
        },
        "resumeMissed": true,
        "resumeOnBoot": true,
        "tz": "string"
      }
    }
  ]
}
```

<h3 id="update-searchdashboard-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchDashboard objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-searchdashboard-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchDashboard](#schemasearchdashboard)]|false|none|none|
|»» autoApplyDebounceMs|number|false|none|none|
|»» autoApplyMode|string|false|none|none|
|»» autoApplyUrlSync|string|false|none|none|
|»» cacheTTLSeconds|number|false|none|none|
|»» category|string|false|none|none|
|»» created|number|true|none|none|
|»» createdBy|string|true|none|none|
|»» description|string|false|none|none|
|»» displayCreatedBy|string|false|none|none|
|»» displayModifiedBy|string|false|none|none|
|»» elements|[oneOf]|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» config|[ElementConfigType](#schemaelementconfigtype)|false|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»»» h|number|true|none|none|
|»»»»» w|number|true|none|none|
|»»»»» x|number|true|none|none|
|»»»»» y|number|true|none|none|
|»»»» search|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» query|string|false|none|none|
|»»»»»» queryId|string|true|none|none|
|»»»»»» runMode|[SavesSearchRunMode](#schemasavessearchrunmode)|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» earliest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» expectedOutputType|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|none|
|»»»»»» latest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» parentSearchId|string|false|none|none|
|»»»»»» query|string|true|none|none|
|»»»»»» sampleRate|number|false|none|none|
|»»»»»» timezone|string|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» type|string|true|none|none|
|»»»»»» values|[string]|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» type|string|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» earliest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» expectedOutputType|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|none|
|»»»»»» latest|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»»» queries|[[PanelQueryDefinition](#schemapanelquerydefinition)]|true|none|none|
|»»»»»»» alias|string|false|none|none|
|»»»»»»» localId|string|true|none|none|
|»»»»»»» query|string|true|none|none|
|»»»»»» timezone|string|false|none|none|
|»»»»»» type|string|true|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»»» label|string|true|none|none|
|»»»»» openInNewTab|boolean|false|none|none|
|»»»»» url|string|true|none|none|
|»»»» type|[VisualizationElementType](#schemavisualizationelementtype)|true|none|none|
|»»»» variant|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» inputId|string|true|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»» search|any|false|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»» type|[InputElementType](#schemainputelementtype)|true|none|none|
|»»»» value|object|false|none|none|
|»»»» variant|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» config|[MarkdownElementConfig](#schemamarkdownelementconfig)|false|none|none|
|»»»»» markdown|string|true|none|none|
|»»»» description|string|false|none|none|
|»»»» empty|boolean|false|none|none|
|»»»» group|string|false|none|none|
|»»»» hidePanel|boolean|false|none|none|
|»»»» horizontalChart|boolean|false|none|none|
|»»»» id|string|true|none|none|
|»»»» index|number|false|none|none|
|»»»» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|»»»» title|string|false|none|none|
|»»»» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|»»»» type|[MarkdownElementType](#schemamarkdownelementtype)|true|none|none|
|»»»» variant|string|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» groups|[DashboardGroups](#schemadashboardgroups)|false|none|none|
|»»» **additionalProperties**|object|false|none|none|
|»»»» action|object|false|none|none|
|»»»»» label|string|true|none|none|
|»»»»» params|object|false|none|none|
|»»»»»» **additionalProperties**|string|false|none|none|
|»»»»» target|string|true|none|none|
|»»»» collapsed|boolean|false|none|none|
|»»»» inputId|string|false|none|none|
|»»»» title|string|true|none|none|
|»» id|string|true|none|none|
|»» modified|number|true|none|none|
|»» modifiedBy|string|false|none|none|
|»» name|string|true|none|none|
|»» packId|string|false|none|none|
|»» refreshRate|number|false|none|none|
|»» resolvedDatasetIds|[string]|false|none|none|
|»» schedule|[SavedQuerySchedule](#schemasavedqueryschedule)|false|none|none|
|»»» cronSchedule|string|true|none|none|
|»»» enabled|boolean|true|none|none|
|»»» keepLastN|number|true|none|none|
|»»» notifications|object|false|none|none|
|»»»» disabled|boolean|true|none|none|
|»»»» items|[[Notification](#schemanotification)]|false|none|none|
|»»»»» id|string|true|none|none|
|»»»»» disabled|boolean|false|none|none|
|»»»»» condition|string|true|none|none|
|»»»»» targets|[string]|false|none|Targets to send any Notifications to|
|»»»»» targetConfigs|[anyOf]|false|none|none|
|»»»»»» id|string|true|none|none|
|»»»»» conf|object|false|none|none|
|»»»»» metadata|[object]|false|none|Fields to add to events from this input|
|»»»»»» name|string|true|none|none|
|»»»»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»»» resumeMissed|boolean|false|none|none|
|»»» resumeOnBoot|boolean|false|none|none|
|»»» tz|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|autoApplyMode|metric|
|autoApplyMode|all|
|autoApplyMode|off|
|autoApplyUrlSync|push|
|autoApplyUrlSync|replace|
|autoApplyUrlSync|off|
|runMode|newSearch|
|runMode|lastRun|
|type|saved|
|expectedOutputType|range|
|expectedOutputType|instant|
|type|inline|
|type|values|
|type|empty|
|expectedOutputType|range|
|expectedOutputType|instant|
|type|metric|
|type|chart.area|
|type|chart.column|
|type|chart.funnel|
|type|chart.gauge|
|type|chart.horizontalBar|
|type|chart.line|
|type|chart.map|
|type|chart.pie|
|type|chart.scatter|
|type|counter.single|
|type|list.events|
|type|list.table|
|type|custom.throughputMetrics|
|type|custom.flowMatrix|
|variant|visualization|
|type|input.timerange|
|type|input.dropdown|
|type|input.text|
|type|input.number|
|variant|input|
|type|markdown.copilot|
|type|markdown.default|
|variant|markdown|

<aside class="success">
This operation does not require authentication
</aside>

## Get SearchDashboard ACL

<a id="opIdgetSearchDashboardAclById"></a>

> Code samples

`GET /search/dashboards/{id}/acl`

List accesses to SearchDashboard granted to users

<h3 id="get-searchdashboard-acl-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID for ACL Get|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "perms": [
        {
          "gid": "string",
          "id": "string",
          "policy": "string",
          "type": "groups"
        }
      ],
      "user": "string"
    }
  ]
}
```

<h3 id="get-searchdashboard-acl-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UserAccessControlList objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-searchdashboard-acl-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UserAccessControlList](#schemauseraccesscontrollist)]|false|none|none|
|»» perms|[[ResourcePolicy](#schemaresourcepolicy)]|true|none|none|
|»»» gid|string|true|none|none|
|»»» id|string|false|none|none|
|»»» policy|string|true|none|none|
|»»» type|[RbacResource](#schemarbacresource)|true|none|none|
|»» user|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|groups|
|type|datasets|
|type|dataset-providers|
|type|projects|
|type|dashboards|
|type|macros|
|type|notebooks|

<aside class="success">
This operation does not require authentication
</aside>

## Modify SearchDashboard ACL

<a id="opIdcreateSearchDashboardAclApplyById"></a>

> Code samples

`POST /search/dashboards/{id}/acl/apply`

Add/remove accesses to SearchDashboard for users

> Body parameter

```json
{
  "add": {},
  "rm": {}
}
```

<h3 id="modify-searchdashboard-acl-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID for ACL Create|
|body|body|[AccessControlSchema](#schemaaccesscontrolschema)|true|AccessControlSchema object|
|» add|body|[AccessControl](#schemaaccesscontrol)|false|none|
|» rm|body|[AccessControl](#schemaaccesscontrol)|false|none|

> Example responses

> 500 Response

```json
{
  "message": "string"
}
```

<h3 id="modify-searchdashboard-acl-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|N/A|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get SearchDashboard Teams

<a id="opIdgetSearchDashboardAclTeamsById"></a>

> Code samples

`GET /search/dashboards/{id}/acl/teams`

List accesses to SearchDashboard granted to Teams

<h3 id="get-searchdashboard-teams-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID for ACL Teams Get|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "perms": [
        {
          "gid": "string",
          "id": "string",
          "policy": "string",
          "type": "groups"
        }
      ],
      "user": "string"
    }
  ]
}
```

<h3 id="get-searchdashboard-teams-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UserAccessControlList objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-searchdashboard-teams-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UserAccessControlList](#schemauseraccesscontrollist)]|false|none|none|
|»» perms|[[ResourcePolicy](#schemaresourcepolicy)]|true|none|none|
|»»» gid|string|true|none|none|
|»»» id|string|false|none|none|
|»»» policy|string|true|none|none|
|»»» type|[RbacResource](#schemarbacresource)|true|none|none|
|»» user|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|groups|
|type|datasets|
|type|dataset-providers|
|type|projects|
|type|dashboards|
|type|macros|
|type|notebooks|

<aside class="success">
This operation does not require authentication
</aside>

## Modify SearchDashboard Teams ACL

<a id="opIdcreateSearchDashboardAclTeamsApplyById"></a>

> Code samples

`POST /search/dashboards/{id}/acl/teams/apply`

Add/remove accesses to SearchDashboard for Teams

> Body parameter

```json
{
  "add": {},
  "rm": {}
}
```

<h3 id="modify-searchdashboard-teams-acl-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID for ACL Teams Create|
|body|body|[AccessControlSchema](#schemaaccesscontrolschema)|true|AccessControlSchema object|
|» add|body|[AccessControl](#schemaaccesscontrol)|false|none|
|» rm|body|[AccessControl](#schemaaccesscontrol)|false|none|

> Example responses

> 500 Response

```json
{
  "message": "string"
}
```

<h3 id="modify-searchdashboard-teams-acl-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|N/A|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_SearchDashboard">SearchDashboard</h2>
<!-- backwards compatibility -->
<a id="schemasearchdashboard"></a>
<a id="schema_SearchDashboard"></a>
<a id="tocSsearchdashboard"></a>
<a id="tocssearchdashboard"></a>

```json
{
  "autoApplyDebounceMs": 0,
  "autoApplyMode": "metric",
  "autoApplyUrlSync": "push",
  "cacheTTLSeconds": 0,
  "category": "string",
  "created": 0,
  "createdBy": "string",
  "description": "string",
  "displayCreatedBy": "string",
  "displayModifiedBy": "string",
  "elements": [
    {
      "config": {},
      "description": "string",
      "empty": true,
      "group": "string",
      "hidePanel": true,
      "horizontalChart": true,
      "id": "string",
      "index": 0,
      "layout": {
        "h": 0,
        "w": 0,
        "x": 0,
        "y": 0
      },
      "search": {
        "query": "string",
        "queryId": "string",
        "runMode": "newSearch",
        "type": "saved"
      },
      "title": "string",
      "titleAction": {
        "label": "string",
        "openInNewTab": true,
        "url": "string"
      },
      "type": "chart.area",
      "variant": "visualization"
    }
  ],
  "groups": {
    "property1": {
      "action": {
        "label": "string",
        "params": {
          "property1": "string",
          "property2": "string"
        },
        "target": "string"
      },
      "collapsed": true,
      "inputId": "string",
      "title": "string"
    },
    "property2": {
      "action": {
        "label": "string",
        "params": {
          "property1": "string",
          "property2": "string"
        },
        "target": "string"
      },
      "collapsed": true,
      "inputId": "string",
      "title": "string"
    }
  },
  "id": "string",
  "modified": 0,
  "modifiedBy": "string",
  "name": "string",
  "packId": "string",
  "refreshRate": 0,
  "resolvedDatasetIds": [
    "string"
  ],
  "schedule": {
    "cronSchedule": "string",
    "enabled": true,
    "keepLastN": 0,
    "notifications": {
      "disabled": true,
      "items": [
        {
          "id": "string",
          "disabled": false,
          "condition": "string",
          "targets": [],
          "targetConfigs": [
            {
              "id": "string",
              "conf": {
                "subject": "string",
                "body": "string",
                "emailRecipient": {
                  "to": "string",
                  "cc": "string",
                  "bcc": "string"
                }
              }
            }
          ],
          "conf": {},
          "metadata": [
            {
              "name": "string",
              "value": "string"
            }
          ]
        }
      ]
    },
    "resumeMissed": true,
    "resumeOnBoot": true,
    "tz": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|autoApplyDebounceMs|number|false|none|none|
|autoApplyMode|string|false|none|none|
|autoApplyUrlSync|string|false|none|none|
|cacheTTLSeconds|number|false|none|none|
|category|string|false|none|none|
|created|number|true|none|none|
|createdBy|string|true|none|none|
|description|string|false|none|none|
|displayCreatedBy|string|false|none|none|
|displayModifiedBy|string|false|none|none|
|elements|[DashboardElements](#schemadashboardelements)|true|none|none|
|groups|[DashboardGroups](#schemadashboardgroups)|false|none|none|
|id|string|true|none|none|
|modified|number|true|none|none|
|modifiedBy|string|false|none|none|
|name|string|true|none|none|
|packId|string|false|none|none|
|refreshRate|number|false|none|none|
|resolvedDatasetIds|[string]|false|none|none|
|schedule|[SavedQuerySchedule](#schemasavedqueryschedule)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|autoApplyMode|metric|
|autoApplyMode|all|
|autoApplyMode|off|
|autoApplyUrlSync|push|
|autoApplyUrlSync|replace|
|autoApplyUrlSync|off|

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

<h2 id="tocS_UserAccessControlList">UserAccessControlList</h2>
<!-- backwards compatibility -->
<a id="schemauseraccesscontrollist"></a>
<a id="schema_UserAccessControlList"></a>
<a id="tocSuseraccesscontrollist"></a>
<a id="tocsuseraccesscontrollist"></a>

```json
{
  "perms": [
    {
      "gid": "string",
      "id": "string",
      "policy": "string",
      "type": "groups"
    }
  ],
  "user": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|perms|[[ResourcePolicy](#schemaresourcepolicy)]|true|none|none|
|user|string|true|none|none|

<h2 id="tocS_AccessControlSchema">AccessControlSchema</h2>
<!-- backwards compatibility -->
<a id="schemaaccesscontrolschema"></a>
<a id="schema_AccessControlSchema"></a>
<a id="tocSaccesscontrolschema"></a>
<a id="tocsaccesscontrolschema"></a>

```json
{
  "add": {},
  "rm": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|add|[AccessControl](#schemaaccesscontrol)|false|none|none|
|rm|[AccessControl](#schemaaccesscontrol)|false|none|none|

<h2 id="tocS_DashboardElements">DashboardElements</h2>
<!-- backwards compatibility -->
<a id="schemadashboardelements"></a>
<a id="schema_DashboardElements"></a>
<a id="tocSdashboardelements"></a>
<a id="tocsdashboardelements"></a>

```json
[
  {
    "config": {},
    "description": "string",
    "empty": true,
    "group": "string",
    "hidePanel": true,
    "horizontalChart": true,
    "id": "string",
    "index": 0,
    "layout": {
      "h": 0,
      "w": 0,
      "x": 0,
      "y": 0
    },
    "search": {
      "query": "string",
      "queryId": "string",
      "runMode": "newSearch",
      "type": "saved"
    },
    "title": "string",
    "titleAction": {
      "label": "string",
      "openInNewTab": true,
      "url": "string"
    },
    "type": "chart.area",
    "variant": "visualization"
  }
]

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[DashboardElement](#schemadashboardelement)]|false|none|none|

<h2 id="tocS_DashboardGroups">DashboardGroups</h2>
<!-- backwards compatibility -->
<a id="schemadashboardgroups"></a>
<a id="schema_DashboardGroups"></a>
<a id="tocSdashboardgroups"></a>
<a id="tocsdashboardgroups"></a>

```json
{
  "property1": {
    "action": {
      "label": "string",
      "params": {
        "property1": "string",
        "property2": "string"
      },
      "target": "string"
    },
    "collapsed": true,
    "inputId": "string",
    "title": "string"
  },
  "property2": {
    "action": {
      "label": "string",
      "params": {
        "property1": "string",
        "property2": "string"
      },
      "target": "string"
    },
    "collapsed": true,
    "inputId": "string",
    "title": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|**additionalProperties**|object|false|none|none|
|» action|object|false|none|none|
|»» label|string|true|none|none|
|»» params|object|false|none|none|
|»»» **additionalProperties**|string|false|none|none|
|»» target|string|true|none|none|
|» collapsed|boolean|false|none|none|
|» inputId|string|false|none|none|
|» title|string|true|none|none|

<h2 id="tocS_SavedQuerySchedule">SavedQuerySchedule</h2>
<!-- backwards compatibility -->
<a id="schemasavedqueryschedule"></a>
<a id="schema_SavedQuerySchedule"></a>
<a id="tocSsavedqueryschedule"></a>
<a id="tocssavedqueryschedule"></a>

```json
{
  "cronSchedule": "string",
  "enabled": true,
  "keepLastN": 0,
  "notifications": {
    "disabled": true,
    "items": [
      {
        "id": "string",
        "disabled": false,
        "condition": "string",
        "targets": [],
        "targetConfigs": [
          {
            "id": "string",
            "conf": {
              "subject": "string",
              "body": "string",
              "emailRecipient": {
                "to": "string",
                "cc": "string",
                "bcc": "string"
              }
            }
          }
        ],
        "conf": {},
        "metadata": [
          {
            "name": "string",
            "value": "string"
          }
        ]
      }
    ]
  },
  "resumeMissed": true,
  "resumeOnBoot": true,
  "tz": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cronSchedule|string|true|none|none|
|enabled|boolean|true|none|none|
|keepLastN|number|true|none|none|
|notifications|object|false|none|none|
|» disabled|boolean|true|none|none|
|» items|[[Notification](#schemanotification)]|false|none|none|
|resumeMissed|boolean|false|none|none|
|resumeOnBoot|boolean|false|none|none|
|tz|string|true|none|none|

<h2 id="tocS_ResourcePolicy">ResourcePolicy</h2>
<!-- backwards compatibility -->
<a id="schemaresourcepolicy"></a>
<a id="schema_ResourcePolicy"></a>
<a id="tocSresourcepolicy"></a>
<a id="tocsresourcepolicy"></a>

```json
{
  "gid": "string",
  "id": "string",
  "policy": "string",
  "type": "groups"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|gid|string|true|none|none|
|id|string|false|none|none|
|policy|string|true|none|none|
|type|[RbacResource](#schemarbacresource)|true|none|none|

<h2 id="tocS_AccessControl">AccessControl</h2>
<!-- backwards compatibility -->
<a id="schemaaccesscontrol"></a>
<a id="schema_AccessControl"></a>
<a id="tocSaccesscontrol"></a>
<a id="tocsaccesscontrol"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocS_DashboardElement">DashboardElement</h2>
<!-- backwards compatibility -->
<a id="schemadashboardelement"></a>
<a id="schema_DashboardElement"></a>
<a id="tocSdashboardelement"></a>
<a id="tocsdashboardelement"></a>

```json
{
  "config": {},
  "description": "string",
  "empty": true,
  "group": "string",
  "hidePanel": true,
  "horizontalChart": true,
  "id": "string",
  "index": 0,
  "layout": {
    "h": 0,
    "w": 0,
    "x": 0,
    "y": 0
  },
  "search": {
    "query": "string",
    "queryId": "string",
    "runMode": "newSearch",
    "type": "saved"
  },
  "title": "string",
  "titleAction": {
    "label": "string",
    "openInNewTab": true,
    "url": "string"
  },
  "type": "chart.area",
  "variant": "visualization"
}

```

### Properties

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» config|[ElementConfigType](#schemaelementconfigtype)|false|none|none|
|» description|string|false|none|none|
|» empty|boolean|false|none|none|
|» group|string|false|none|none|
|» hidePanel|boolean|false|none|none|
|» horizontalChart|boolean|false|none|none|
|» id|string|true|none|none|
|» index|number|false|none|none|
|» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|» search|[SearchQuery](#schemasearchquery)|true|none|none|
|» title|string|false|none|none|
|» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|» type|[VisualizationElementType](#schemavisualizationelementtype)|true|none|none|
|» variant|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» description|string|false|none|none|
|» empty|boolean|false|none|none|
|» group|string|false|none|none|
|» hidePanel|boolean|false|none|none|
|» horizontalChart|boolean|false|none|none|
|» id|string|true|none|none|
|» index|number|false|none|none|
|» inputId|string|true|none|none|
|» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|» search|[SearchQuery](#schemasearchquery)|false|none|none|
|» title|string|false|none|none|
|» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|» type|[InputElementType](#schemainputelementtype)|true|none|none|
|» value|object|false|none|none|
|» variant|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» config|[MarkdownElementConfig](#schemamarkdownelementconfig)|false|none|none|
|» description|string|false|none|none|
|» empty|boolean|false|none|none|
|» group|string|false|none|none|
|» hidePanel|boolean|false|none|none|
|» horizontalChart|boolean|false|none|none|
|» id|string|true|none|none|
|» index|number|false|none|none|
|» layout|[DashboardLayout](#schemadashboardlayout)|true|none|none|
|» title|string|false|none|none|
|» titleAction|[TitleAction](#schematitleaction)|false|none|none|
|» type|[MarkdownElementType](#schemamarkdownelementtype)|true|none|none|
|» variant|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|variant|visualization|
|variant|input|
|variant|markdown|

<h2 id="tocS_Notification">Notification</h2>
<!-- backwards compatibility -->
<a id="schemanotification"></a>
<a id="schema_Notification"></a>
<a id="tocSnotification"></a>
<a id="tocsnotification"></a>

```json
{
  "id": "string",
  "disabled": false,
  "condition": "string",
  "targets": [],
  "targetConfigs": [
    {
      "id": "string",
      "conf": {
        "subject": "string",
        "body": "string",
        "emailRecipient": {
          "to": "string",
          "cc": "string",
          "bcc": "string"
        }
      }
    }
  ],
  "conf": {},
  "metadata": [
    {
      "name": "string",
      "value": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|disabled|boolean|false|none|none|
|condition|string|true|none|none|
|targets|[string]|false|none|Targets to send any Notifications to|
|targetConfigs|[anyOf]|false|none|none|
|» id|string|true|none|none|
|conf|object|false|none|none|
|metadata|[object]|false|none|Fields to add to events from this input|
|» name|string|true|none|none|
|» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|

<h2 id="tocS_RbacResource">RbacResource</h2>
<!-- backwards compatibility -->
<a id="schemarbacresource"></a>
<a id="schema_RbacResource"></a>
<a id="tocSrbacresource"></a>
<a id="tocsrbacresource"></a>

```json
"groups"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|groups|
|*anonymous*|datasets|
|*anonymous*|dataset-providers|
|*anonymous*|projects|
|*anonymous*|dashboards|
|*anonymous*|macros|
|*anonymous*|notebooks|

<h2 id="tocS_ElementConfigType">ElementConfigType</h2>
<!-- backwards compatibility -->
<a id="schemaelementconfigtype"></a>
<a id="schema_ElementConfigType"></a>
<a id="tocSelementconfigtype"></a>
<a id="tocselementconfigtype"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocS_DashboardLayout">DashboardLayout</h2>
<!-- backwards compatibility -->
<a id="schemadashboardlayout"></a>
<a id="schema_DashboardLayout"></a>
<a id="tocSdashboardlayout"></a>
<a id="tocsdashboardlayout"></a>

```json
{
  "h": 0,
  "w": 0,
  "x": 0,
  "y": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|h|number|true|none|none|
|w|number|true|none|none|
|x|number|true|none|none|
|y|number|true|none|none|

<h2 id="tocS_SearchQuery">SearchQuery</h2>
<!-- backwards compatibility -->
<a id="schemasearchquery"></a>
<a id="schema_SearchQuery"></a>
<a id="tocSsearchquery"></a>
<a id="tocssearchquery"></a>

```json
{
  "query": "string",
  "queryId": "string",
  "runMode": "newSearch",
  "type": "saved"
}

```

### Properties

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» query|string|false|none|none|
|» queryId|string|true|none|none|
|» runMode|[SavesSearchRunMode](#schemasavessearchrunmode)|false|none|none|
|» type|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» earliest|any|true|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» expectedOutputType|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|none|
|» latest|any|true|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» parentSearchId|string|false|none|none|
|» query|string|true|none|none|
|» sampleRate|number|false|none|none|
|» timezone|string|false|none|none|
|» type|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» type|string|true|none|none|
|» values|[string]|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» type|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» earliest|any|true|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» expectedOutputType|[ExpectedOutputType](#schemaexpectedoutputtype)|false|none|none|
|» latest|any|true|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» queries|[[PanelQueryDefinition](#schemapanelquerydefinition)]|true|none|none|
|» timezone|string|false|none|none|
|» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|saved|
|type|inline|
|type|values|
|type|empty|
|type|metric|

<h2 id="tocS_TitleAction">TitleAction</h2>
<!-- backwards compatibility -->
<a id="schematitleaction"></a>
<a id="schema_TitleAction"></a>
<a id="tocStitleaction"></a>
<a id="tocstitleaction"></a>

```json
{
  "label": "string",
  "openInNewTab": true,
  "url": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|label|string|true|none|none|
|openInNewTab|boolean|false|none|none|
|url|string|true|none|none|

<h2 id="tocS_VisualizationElementType">VisualizationElementType</h2>
<!-- backwards compatibility -->
<a id="schemavisualizationelementtype"></a>
<a id="schema_VisualizationElementType"></a>
<a id="tocSvisualizationelementtype"></a>
<a id="tocsvisualizationelementtype"></a>

```json
"chart.area"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|chart.area|
|*anonymous*|chart.column|
|*anonymous*|chart.funnel|
|*anonymous*|chart.gauge|
|*anonymous*|chart.horizontalBar|
|*anonymous*|chart.line|
|*anonymous*|chart.map|
|*anonymous*|chart.pie|
|*anonymous*|chart.scatter|
|*anonymous*|counter.single|
|*anonymous*|list.events|
|*anonymous*|list.table|
|*anonymous*|custom.throughputMetrics|
|*anonymous*|custom.flowMatrix|

<h2 id="tocS_InputElementType">InputElementType</h2>
<!-- backwards compatibility -->
<a id="schemainputelementtype"></a>
<a id="schema_InputElementType"></a>
<a id="tocSinputelementtype"></a>
<a id="tocsinputelementtype"></a>

```json
"input.timerange"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|input.timerange|
|*anonymous*|input.dropdown|
|*anonymous*|input.text|
|*anonymous*|input.number|

<h2 id="tocS_MarkdownElementConfig">MarkdownElementConfig</h2>
<!-- backwards compatibility -->
<a id="schemamarkdownelementconfig"></a>
<a id="schema_MarkdownElementConfig"></a>
<a id="tocSmarkdownelementconfig"></a>
<a id="tocsmarkdownelementconfig"></a>

```json
{
  "markdown": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|markdown|string|true|none|none|

<h2 id="tocS_MarkdownElementType">MarkdownElementType</h2>
<!-- backwards compatibility -->
<a id="schemamarkdownelementtype"></a>
<a id="schema_MarkdownElementType"></a>
<a id="tocSmarkdownelementtype"></a>
<a id="tocsmarkdownelementtype"></a>

```json
"markdown.copilot"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|markdown.copilot|
|*anonymous*|markdown.default|

<h2 id="tocS_SavesSearchRunMode">SavesSearchRunMode</h2>
<!-- backwards compatibility -->
<a id="schemasavessearchrunmode"></a>
<a id="schema_SavesSearchRunMode"></a>
<a id="tocSsavessearchrunmode"></a>
<a id="tocssavessearchrunmode"></a>

```json
"newSearch"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|newSearch|
|*anonymous*|lastRun|

<h2 id="tocS_ExpectedOutputType">ExpectedOutputType</h2>
<!-- backwards compatibility -->
<a id="schemaexpectedoutputtype"></a>
<a id="schema_ExpectedOutputType"></a>
<a id="tocSexpectedoutputtype"></a>
<a id="tocsexpectedoutputtype"></a>

```json
"range"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|range|
|*anonymous*|instant|

<h2 id="tocS_PanelQueryDefinition">PanelQueryDefinition</h2>
<!-- backwards compatibility -->
<a id="schemapanelquerydefinition"></a>
<a id="schema_PanelQueryDefinition"></a>
<a id="tocSpanelquerydefinition"></a>
<a id="tocspanelquerydefinition"></a>

```json
{
  "alias": "string",
  "localId": "string",
  "query": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|alias|string|false|none|none|
|localId|string|true|none|none|
|query|string|true|none|none|

