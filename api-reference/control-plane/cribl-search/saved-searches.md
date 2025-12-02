
<h1 id="cribl-search-api-saved-searches">Cribl Search API - Saved Searches v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-search-api-saved-searches-saved-searches">Saved Searches</h1>

## Get a list of SavedQuery objects

<a id="opIdlistSavedQuery"></a>

> Code samples

`GET /search/saved`

Get a list of SavedQuery objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "chartConfig": {
        "applyThreshold": true,
        "axis": {
          "xAxis": "string",
          "yAxis": [
            "string"
          ],
          "yAxisExcluded": [
            "string"
          ]
        },
        "color": "string",
        "colorPalette": 0,
        "colorPaletteReversed": true,
        "colorThresholds": {
          "thresholds": [
            {
              "color": "string",
              "threshold": 0
            }
          ]
        },
        "customData": {
          "connectNulls": "string",
          "dataFields": [
            "string"
          ],
          "isPointColor": true,
          "limitToTopN": 0,
          "lines": true,
          "nameField": "string",
          "pointColorPalette": 0,
          "pointColorPaletteReversed": true,
          "pointScale": "string",
          "pointScaleDataField": "string",
          "seriesCount": 0,
          "splitBy": "string",
          "stack": true,
          "summarizeOthers": true,
          "trellis": true
        },
        "decimals": 0,
        "label": "string",
        "legend": {
          "position": "string",
          "selected": {
            "property1": true,
            "property2": true
          },
          "truncate": true
        },
        "mapDetails": {
          "latitudeField": "string",
          "longitudeField": "string",
          "mapSourceID": "string",
          "mapType": "string",
          "nameField": "string",
          "pointScale": "string",
          "valueField": "string"
        },
        "onClickAction": {
          "search": "string",
          "selectedDashboardId": "string",
          "selectedInputId": "string",
          "selectedLinkId": "string",
          "selectedTimerangeInputId": "string",
          "type": "string"
        },
        "prefix": "string",
        "separator": true,
        "series": [
          {
            "areaStyle": {
              "opacity": 0,
              "shadowBlur": 0,
              "shadowColor": "string",
              "shadowOffsetX": 0,
              "shadowOffsetY": 0
            },
            "color": "string",
            "data": [
              {}
            ],
            "map": "string",
            "name": "string",
            "type": "area",
            "yAxisField": "string"
          }
        ],
        "seriesInfo": {
          "property1": "area",
          "property2": "area"
        },
        "shouldApplyUserChartSettings": true,
        "style": true,
        "suffix": "string",
        "type": "string",
        "xAxis": {
          "dataField": "string",
          "inverse": true,
          "labelInterval": "string",
          "labelOrientation": 0,
          "name": "string",
          "offset": 0,
          "position": "string",
          "type": "string"
        },
        "yAxis": {
          "dataField": [
            "string"
          ],
          "interval": 0,
          "max": 0,
          "min": 0,
          "position": "string",
          "scale": "string",
          "splitLine": true,
          "type": "string"
        }
      },
      "description": "string",
      "displayUsername": "string",
      "earliest": "string",
      "id": "string",
      "isPrivate": true,
      "isSystem": true,
      "latest": "string",
      "lib": "cribl",
      "name": "string",
      "query": "string",
      "resolvedDatasetIds": [
        "string"
      ],
      "sampleRate": 0,
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
      },
      "searchJobSource": "command",
      "tableConfig": {},
      "user": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-savedquery-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SavedQuery objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-savedquery-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SavedQuery](#schemasavedquery)]|false|none|none|
|»» chartConfig|[ChartConfig](#schemachartconfig)|false|none|none|
|»»» applyThreshold|boolean|false|none|none|
|»»» axis|object|false|none|none|
|»»»» xAxis|string|false|none|none|
|»»»» yAxis|[string]|false|none|none|
|»»»» yAxisExcluded|[string]|false|none|none|
|»»» color|string|false|none|none|
|»»» colorPalette|number|true|none|none|
|»»» colorPaletteReversed|boolean|false|none|none|
|»»» colorThresholds|object|false|none|none|
|»»»» thresholds|[object]|true|none|none|
|»»»»» color|string|true|none|none|
|»»»»» threshold|number|true|none|none|
|»»» customData|object|false|none|none|
|»»»» connectNulls|string|false|none|none|
|»»»» dataFields|[string]|false|none|none|
|»»»» isPointColor|boolean|false|none|none|
|»»»» limitToTopN|number|false|none|none|
|»»»» lines|boolean|false|none|none|
|»»»» nameField|string|false|none|none|
|»»»» pointColorPalette|number|false|none|none|
|»»»» pointColorPaletteReversed|boolean|false|none|none|
|»»»» pointScale|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» pointScaleDataField|string|false|none|none|
|»»»» seriesCount|number|false|none|none|
|»»»» splitBy|string|false|none|none|
|»»»» stack|boolean|false|none|none|
|»»»» summarizeOthers|boolean|false|none|none|
|»»»» trellis|boolean|false|none|none|
|»»» decimals|number|false|none|none|
|»»» label|string|false|none|none|
|»»» legend|object|false|none|none|
|»»»» position|string|false|none|none|
|»»»» selected|object|false|none|none|
|»»»»» **additionalProperties**|boolean|false|none|none|
|»»»» truncate|boolean|false|none|none|
|»»» mapDetails|object|false|none|none|
|»»»» latitudeField|string|false|none|none|
|»»»» longitudeField|string|false|none|none|
|»»»» mapSourceID|string|false|none|none|
|»»»» mapType|string|false|none|none|
|»»»» nameField|string|false|none|none|
|»»»» pointScale|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» valueField|string|false|none|none|
|»»» onClickAction|object|false|none|none|
|»»»» search|string|false|none|none|
|»»»» selectedDashboardId|string|false|none|none|
|»»»» selectedInputId|string|false|none|none|
|»»»» selectedLinkId|string|false|none|none|
|»»»» selectedTimerangeInputId|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» prefix|string|false|none|none|
|»»» separator|boolean|false|none|none|
|»»» series|[[ChartSeries](#schemachartseries)]|false|none|none|
|»»»» areaStyle|[AreaStyleOption](#schemaareastyleoption)|false|none|none|
|»»»»» opacity|number|false|none|none|
|»»»»» shadowBlur|number|false|none|none|
|»»»»» shadowColor|string|false|none|none|
|»»»»» shadowOffsetX|number|false|none|none|
|»»»»» shadowOffsetY|number|false|none|none|
|»»»» color|string|false|none|none|
|»»»» data|[object]|false|none|none|
|»»»» map|string|false|none|none|
|»»»» name|string|true|none|none|
|»»»» type|[ChartType](#schemacharttype)|false|none|none|
|»»»» yAxisField|string|false|none|none|
|»»» seriesInfo|object|false|none|none|
|»»»» **additionalProperties**|[ChartType](#schemacharttype)|false|none|none|
|»»» shouldApplyUserChartSettings|boolean|false|none|none|
|»»» style|boolean|false|none|none|
|»»» suffix|string|false|none|none|
|»»» type|string|true|none|none|
|»»» xAxis|object|false|none|none|
|»»»» dataField|string|false|none|none|
|»»»» inverse|boolean|false|none|none|
|»»»» labelInterval|string|false|none|none|
|»»»» labelOrientation|number|false|none|none|
|»»»» name|string|false|none|none|
|»»»» offset|number|false|none|none|
|»»»» position|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» yAxis|object|false|none|none|
|»»»» dataField|[string]|false|none|none|
|»»»» interval|number|false|none|none|
|»»»» max|number|false|none|none|
|»»»» min|number|false|none|none|
|»»»» position|string|false|none|none|
|»»»» scale|string|false|none|none|
|»»»» splitLine|boolean|false|none|none|
|»»»» type|string|false|none|none|
|»» description|string|false|none|none|
|»» displayUsername|string|false|none|none|
|»» earliest|string|false|none|none|
|»» id|string|true|none|none|
|»» isPrivate|boolean|false|none|none|
|»» isSystem|boolean|false|none|none|
|»» latest|string|false|none|none|
|»» lib|[CriblLib](#schemacribllib)|false|none|none|
|»» name|string|true|none|none|
|»» query|string|true|none|none|
|»» resolvedDatasetIds|[string]|false|none|none|
|»» sampleRate|number|false|none|none|
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
|»» searchJobSource|string|false|none|none|
|»» tableConfig|object|false|none|none|
|»» user|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|area|
|type|column|
|type|events|
|type|funnel|
|type|gauge|
|type|horizontalBar|
|type|line|
|type|map|
|type|pie|
|type|scatter|
|type|single|
|type|table|
|**additionalProperties**|area|
|**additionalProperties**|column|
|**additionalProperties**|events|
|**additionalProperties**|funnel|
|**additionalProperties**|gauge|
|**additionalProperties**|horizontalBar|
|**additionalProperties**|line|
|**additionalProperties**|map|
|**additionalProperties**|pie|
|**additionalProperties**|scatter|
|**additionalProperties**|single|
|**additionalProperties**|table|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|
|searchJobSource|command|
|searchJobSource|standard|
|searchJobSource|scheduled|
|searchJobSource|datatypePreview|
|searchJobSource|dashboard|
|searchJobSource|notebook|
|searchJobSource|systemInsights|

<aside class="success">
This operation does not require authentication
</aside>

## Create SavedQuery

<a id="opIdcreateSavedQuery"></a>

> Code samples

`POST /search/saved`

Create SavedQuery

> Body parameter

```json
{
  "chartConfig": {
    "applyThreshold": true,
    "axis": {
      "xAxis": "string",
      "yAxis": [
        "string"
      ],
      "yAxisExcluded": [
        "string"
      ]
    },
    "color": "string",
    "colorPalette": 0,
    "colorPaletteReversed": true,
    "colorThresholds": {
      "thresholds": [
        {
          "color": "string",
          "threshold": 0
        }
      ]
    },
    "customData": {
      "connectNulls": "string",
      "dataFields": [
        "string"
      ],
      "isPointColor": true,
      "limitToTopN": 0,
      "lines": true,
      "nameField": "string",
      "pointColorPalette": 0,
      "pointColorPaletteReversed": true,
      "pointScale": "string",
      "pointScaleDataField": "string",
      "seriesCount": 0,
      "splitBy": "string",
      "stack": true,
      "summarizeOthers": true,
      "trellis": true
    },
    "decimals": 0,
    "label": "string",
    "legend": {
      "position": "string",
      "selected": {
        "property1": true,
        "property2": true
      },
      "truncate": true
    },
    "mapDetails": {
      "latitudeField": "string",
      "longitudeField": "string",
      "mapSourceID": "string",
      "mapType": "string",
      "nameField": "string",
      "pointScale": "string",
      "valueField": "string"
    },
    "onClickAction": {
      "search": "string",
      "selectedDashboardId": "string",
      "selectedInputId": "string",
      "selectedLinkId": "string",
      "selectedTimerangeInputId": "string",
      "type": "string"
    },
    "prefix": "string",
    "separator": true,
    "series": [
      {
        "areaStyle": {
          "opacity": 0,
          "shadowBlur": 0,
          "shadowColor": "string",
          "shadowOffsetX": 0,
          "shadowOffsetY": 0
        },
        "color": "string",
        "data": [
          {}
        ],
        "map": "string",
        "name": "string",
        "type": "area",
        "yAxisField": "string"
      }
    ],
    "seriesInfo": {
      "property1": "area",
      "property2": "area"
    },
    "shouldApplyUserChartSettings": true,
    "style": true,
    "suffix": "string",
    "type": "string",
    "xAxis": {
      "dataField": "string",
      "inverse": true,
      "labelInterval": "string",
      "labelOrientation": 0,
      "name": "string",
      "offset": 0,
      "position": "string",
      "type": "string"
    },
    "yAxis": {
      "dataField": [
        "string"
      ],
      "interval": 0,
      "max": 0,
      "min": 0,
      "position": "string",
      "scale": "string",
      "splitLine": true,
      "type": "string"
    }
  },
  "description": "string",
  "displayUsername": "string",
  "earliest": "string",
  "id": "string",
  "isPrivate": true,
  "isSystem": true,
  "latest": "string",
  "lib": "cribl",
  "name": "string",
  "query": "string",
  "resolvedDatasetIds": [
    "string"
  ],
  "sampleRate": 0,
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
  },
  "searchJobSource": "command",
  "tableConfig": {},
  "user": "string"
}
```

<h3 id="create-savedquery-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[SavedQuery](#schemasavedquery)|true|New SavedQuery object|
|» chartConfig|body|[ChartConfig](#schemachartconfig)|false|none|
|»» applyThreshold|body|boolean|false|none|
|»» axis|body|object|false|none|
|»»» xAxis|body|string|false|none|
|»»» yAxis|body|[string]|false|none|
|»»» yAxisExcluded|body|[string]|false|none|
|»» color|body|string|false|none|
|»» colorPalette|body|number|true|none|
|»» colorPaletteReversed|body|boolean|false|none|
|»» colorThresholds|body|object|false|none|
|»»» thresholds|body|[object]|true|none|
|»»»» color|body|string|true|none|
|»»»» threshold|body|number|true|none|
|»» customData|body|object|false|none|
|»»» connectNulls|body|string|false|none|
|»»» dataFields|body|[string]|false|none|
|»»» isPointColor|body|boolean|false|none|
|»»» limitToTopN|body|number|false|none|
|»»» lines|body|boolean|false|none|
|»»» nameField|body|string|false|none|
|»»» pointColorPalette|body|number|false|none|
|»»» pointColorPaletteReversed|body|boolean|false|none|
|»»» pointScale|body|any|false|none|
|»»»» *anonymous*|body|string|false|none|
|»»»» *anonymous*|body|number|false|none|
|»»» pointScaleDataField|body|string|false|none|
|»»» seriesCount|body|number|false|none|
|»»» splitBy|body|string|false|none|
|»»» stack|body|boolean|false|none|
|»»» summarizeOthers|body|boolean|false|none|
|»»» trellis|body|boolean|false|none|
|»» decimals|body|number|false|none|
|»» label|body|string|false|none|
|»» legend|body|object|false|none|
|»»» position|body|string|false|none|
|»»» selected|body|object|false|none|
|»»»» **additionalProperties**|body|boolean|false|none|
|»»» truncate|body|boolean|false|none|
|»» mapDetails|body|object|false|none|
|»»» latitudeField|body|string|false|none|
|»»» longitudeField|body|string|false|none|
|»»» mapSourceID|body|string|false|none|
|»»» mapType|body|string|false|none|
|»»» nameField|body|string|false|none|
|»»» pointScale|body|any|false|none|
|»»»» *anonymous*|body|string|false|none|
|»»»» *anonymous*|body|number|false|none|
|»»» valueField|body|string|false|none|
|»» onClickAction|body|object|false|none|
|»»» search|body|string|false|none|
|»»» selectedDashboardId|body|string|false|none|
|»»» selectedInputId|body|string|false|none|
|»»» selectedLinkId|body|string|false|none|
|»»» selectedTimerangeInputId|body|string|false|none|
|»»» type|body|string|false|none|
|»» prefix|body|string|false|none|
|»» separator|body|boolean|false|none|
|»» series|body|[[ChartSeries](#schemachartseries)]|false|none|
|»»» areaStyle|body|[AreaStyleOption](#schemaareastyleoption)|false|none|
|»»»» opacity|body|number|false|none|
|»»»» shadowBlur|body|number|false|none|
|»»»» shadowColor|body|string|false|none|
|»»»» shadowOffsetX|body|number|false|none|
|»»»» shadowOffsetY|body|number|false|none|
|»»» color|body|string|false|none|
|»»» data|body|[object]|false|none|
|»»» map|body|string|false|none|
|»»» name|body|string|true|none|
|»»» type|body|[ChartType](#schemacharttype)|false|none|
|»»» yAxisField|body|string|false|none|
|»» seriesInfo|body|object|false|none|
|»»» **additionalProperties**|body|[ChartType](#schemacharttype)|false|none|
|»» shouldApplyUserChartSettings|body|boolean|false|none|
|»» style|body|boolean|false|none|
|»» suffix|body|string|false|none|
|»» type|body|string|true|none|
|»» xAxis|body|object|false|none|
|»»» dataField|body|string|false|none|
|»»» inverse|body|boolean|false|none|
|»»» labelInterval|body|string|false|none|
|»»» labelOrientation|body|number|false|none|
|»»» name|body|string|false|none|
|»»» offset|body|number|false|none|
|»»» position|body|string|false|none|
|»»» type|body|string|false|none|
|»» yAxis|body|object|false|none|
|»»» dataField|body|[string]|false|none|
|»»» interval|body|number|false|none|
|»»» max|body|number|false|none|
|»»» min|body|number|false|none|
|»»» position|body|string|false|none|
|»»» scale|body|string|false|none|
|»»» splitLine|body|boolean|false|none|
|»»» type|body|string|false|none|
|» description|body|string|false|none|
|» displayUsername|body|string|false|none|
|» earliest|body|string|false|none|
|» id|body|string|true|none|
|» isPrivate|body|boolean|false|none|
|» isSystem|body|boolean|false|none|
|» latest|body|string|false|none|
|» lib|body|[CriblLib](#schemacribllib)|false|none|
|» name|body|string|true|none|
|» query|body|string|true|none|
|» resolvedDatasetIds|body|[string]|false|none|
|» sampleRate|body|number|false|none|
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
|» searchJobSource|body|string|false|none|
|» tableConfig|body|object|false|none|
|» user|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» type|area|
|»»» type|column|
|»»» type|events|
|»»» type|funnel|
|»»» type|gauge|
|»»» type|horizontalBar|
|»»» type|line|
|»»» type|map|
|»»» type|pie|
|»»» type|scatter|
|»»» type|single|
|»»» type|table|
|»»» **additionalProperties**|area|
|»»» **additionalProperties**|column|
|»»» **additionalProperties**|events|
|»»» **additionalProperties**|funnel|
|»»» **additionalProperties**|gauge|
|»»» **additionalProperties**|horizontalBar|
|»»» **additionalProperties**|line|
|»»» **additionalProperties**|map|
|»»» **additionalProperties**|pie|
|»»» **additionalProperties**|scatter|
|»»» **additionalProperties**|single|
|»»» **additionalProperties**|table|
|» lib|cribl|
|» lib|cribl-custom|
|» lib|custom|
|» searchJobSource|command|
|» searchJobSource|standard|
|» searchJobSource|scheduled|
|» searchJobSource|datatypePreview|
|» searchJobSource|dashboard|
|» searchJobSource|notebook|
|» searchJobSource|systemInsights|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "chartConfig": {
        "applyThreshold": true,
        "axis": {
          "xAxis": "string",
          "yAxis": [
            "string"
          ],
          "yAxisExcluded": [
            "string"
          ]
        },
        "color": "string",
        "colorPalette": 0,
        "colorPaletteReversed": true,
        "colorThresholds": {
          "thresholds": [
            {
              "color": "string",
              "threshold": 0
            }
          ]
        },
        "customData": {
          "connectNulls": "string",
          "dataFields": [
            "string"
          ],
          "isPointColor": true,
          "limitToTopN": 0,
          "lines": true,
          "nameField": "string",
          "pointColorPalette": 0,
          "pointColorPaletteReversed": true,
          "pointScale": "string",
          "pointScaleDataField": "string",
          "seriesCount": 0,
          "splitBy": "string",
          "stack": true,
          "summarizeOthers": true,
          "trellis": true
        },
        "decimals": 0,
        "label": "string",
        "legend": {
          "position": "string",
          "selected": {
            "property1": true,
            "property2": true
          },
          "truncate": true
        },
        "mapDetails": {
          "latitudeField": "string",
          "longitudeField": "string",
          "mapSourceID": "string",
          "mapType": "string",
          "nameField": "string",
          "pointScale": "string",
          "valueField": "string"
        },
        "onClickAction": {
          "search": "string",
          "selectedDashboardId": "string",
          "selectedInputId": "string",
          "selectedLinkId": "string",
          "selectedTimerangeInputId": "string",
          "type": "string"
        },
        "prefix": "string",
        "separator": true,
        "series": [
          {
            "areaStyle": {
              "opacity": 0,
              "shadowBlur": 0,
              "shadowColor": "string",
              "shadowOffsetX": 0,
              "shadowOffsetY": 0
            },
            "color": "string",
            "data": [
              {}
            ],
            "map": "string",
            "name": "string",
            "type": "area",
            "yAxisField": "string"
          }
        ],
        "seriesInfo": {
          "property1": "area",
          "property2": "area"
        },
        "shouldApplyUserChartSettings": true,
        "style": true,
        "suffix": "string",
        "type": "string",
        "xAxis": {
          "dataField": "string",
          "inverse": true,
          "labelInterval": "string",
          "labelOrientation": 0,
          "name": "string",
          "offset": 0,
          "position": "string",
          "type": "string"
        },
        "yAxis": {
          "dataField": [
            "string"
          ],
          "interval": 0,
          "max": 0,
          "min": 0,
          "position": "string",
          "scale": "string",
          "splitLine": true,
          "type": "string"
        }
      },
      "description": "string",
      "displayUsername": "string",
      "earliest": "string",
      "id": "string",
      "isPrivate": true,
      "isSystem": true,
      "latest": "string",
      "lib": "cribl",
      "name": "string",
      "query": "string",
      "resolvedDatasetIds": [
        "string"
      ],
      "sampleRate": 0,
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
      },
      "searchJobSource": "command",
      "tableConfig": {},
      "user": "string"
    }
  ]
}
```

<h3 id="create-savedquery-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SavedQuery objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-savedquery-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SavedQuery](#schemasavedquery)]|false|none|none|
|»» chartConfig|[ChartConfig](#schemachartconfig)|false|none|none|
|»»» applyThreshold|boolean|false|none|none|
|»»» axis|object|false|none|none|
|»»»» xAxis|string|false|none|none|
|»»»» yAxis|[string]|false|none|none|
|»»»» yAxisExcluded|[string]|false|none|none|
|»»» color|string|false|none|none|
|»»» colorPalette|number|true|none|none|
|»»» colorPaletteReversed|boolean|false|none|none|
|»»» colorThresholds|object|false|none|none|
|»»»» thresholds|[object]|true|none|none|
|»»»»» color|string|true|none|none|
|»»»»» threshold|number|true|none|none|
|»»» customData|object|false|none|none|
|»»»» connectNulls|string|false|none|none|
|»»»» dataFields|[string]|false|none|none|
|»»»» isPointColor|boolean|false|none|none|
|»»»» limitToTopN|number|false|none|none|
|»»»» lines|boolean|false|none|none|
|»»»» nameField|string|false|none|none|
|»»»» pointColorPalette|number|false|none|none|
|»»»» pointColorPaletteReversed|boolean|false|none|none|
|»»»» pointScale|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» pointScaleDataField|string|false|none|none|
|»»»» seriesCount|number|false|none|none|
|»»»» splitBy|string|false|none|none|
|»»»» stack|boolean|false|none|none|
|»»»» summarizeOthers|boolean|false|none|none|
|»»»» trellis|boolean|false|none|none|
|»»» decimals|number|false|none|none|
|»»» label|string|false|none|none|
|»»» legend|object|false|none|none|
|»»»» position|string|false|none|none|
|»»»» selected|object|false|none|none|
|»»»»» **additionalProperties**|boolean|false|none|none|
|»»»» truncate|boolean|false|none|none|
|»»» mapDetails|object|false|none|none|
|»»»» latitudeField|string|false|none|none|
|»»»» longitudeField|string|false|none|none|
|»»»» mapSourceID|string|false|none|none|
|»»»» mapType|string|false|none|none|
|»»»» nameField|string|false|none|none|
|»»»» pointScale|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» valueField|string|false|none|none|
|»»» onClickAction|object|false|none|none|
|»»»» search|string|false|none|none|
|»»»» selectedDashboardId|string|false|none|none|
|»»»» selectedInputId|string|false|none|none|
|»»»» selectedLinkId|string|false|none|none|
|»»»» selectedTimerangeInputId|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» prefix|string|false|none|none|
|»»» separator|boolean|false|none|none|
|»»» series|[[ChartSeries](#schemachartseries)]|false|none|none|
|»»»» areaStyle|[AreaStyleOption](#schemaareastyleoption)|false|none|none|
|»»»»» opacity|number|false|none|none|
|»»»»» shadowBlur|number|false|none|none|
|»»»»» shadowColor|string|false|none|none|
|»»»»» shadowOffsetX|number|false|none|none|
|»»»»» shadowOffsetY|number|false|none|none|
|»»»» color|string|false|none|none|
|»»»» data|[object]|false|none|none|
|»»»» map|string|false|none|none|
|»»»» name|string|true|none|none|
|»»»» type|[ChartType](#schemacharttype)|false|none|none|
|»»»» yAxisField|string|false|none|none|
|»»» seriesInfo|object|false|none|none|
|»»»» **additionalProperties**|[ChartType](#schemacharttype)|false|none|none|
|»»» shouldApplyUserChartSettings|boolean|false|none|none|
|»»» style|boolean|false|none|none|
|»»» suffix|string|false|none|none|
|»»» type|string|true|none|none|
|»»» xAxis|object|false|none|none|
|»»»» dataField|string|false|none|none|
|»»»» inverse|boolean|false|none|none|
|»»»» labelInterval|string|false|none|none|
|»»»» labelOrientation|number|false|none|none|
|»»»» name|string|false|none|none|
|»»»» offset|number|false|none|none|
|»»»» position|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» yAxis|object|false|none|none|
|»»»» dataField|[string]|false|none|none|
|»»»» interval|number|false|none|none|
|»»»» max|number|false|none|none|
|»»»» min|number|false|none|none|
|»»»» position|string|false|none|none|
|»»»» scale|string|false|none|none|
|»»»» splitLine|boolean|false|none|none|
|»»»» type|string|false|none|none|
|»» description|string|false|none|none|
|»» displayUsername|string|false|none|none|
|»» earliest|string|false|none|none|
|»» id|string|true|none|none|
|»» isPrivate|boolean|false|none|none|
|»» isSystem|boolean|false|none|none|
|»» latest|string|false|none|none|
|»» lib|[CriblLib](#schemacribllib)|false|none|none|
|»» name|string|true|none|none|
|»» query|string|true|none|none|
|»» resolvedDatasetIds|[string]|false|none|none|
|»» sampleRate|number|false|none|none|
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
|»» searchJobSource|string|false|none|none|
|»» tableConfig|object|false|none|none|
|»» user|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|area|
|type|column|
|type|events|
|type|funnel|
|type|gauge|
|type|horizontalBar|
|type|line|
|type|map|
|type|pie|
|type|scatter|
|type|single|
|type|table|
|**additionalProperties**|area|
|**additionalProperties**|column|
|**additionalProperties**|events|
|**additionalProperties**|funnel|
|**additionalProperties**|gauge|
|**additionalProperties**|horizontalBar|
|**additionalProperties**|line|
|**additionalProperties**|map|
|**additionalProperties**|pie|
|**additionalProperties**|scatter|
|**additionalProperties**|single|
|**additionalProperties**|table|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|
|searchJobSource|command|
|searchJobSource|standard|
|searchJobSource|scheduled|
|searchJobSource|datatypePreview|
|searchJobSource|dashboard|
|searchJobSource|notebook|
|searchJobSource|systemInsights|

<aside class="success">
This operation does not require authentication
</aside>

## Delete SavedQuery

<a id="opIddeleteSavedQueryById"></a>

> Code samples

`DELETE /search/saved/{id}`

Delete SavedQuery

<h3 id="delete-savedquery-parameters">Parameters</h3>

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
      "chartConfig": {
        "applyThreshold": true,
        "axis": {
          "xAxis": "string",
          "yAxis": [
            "string"
          ],
          "yAxisExcluded": [
            "string"
          ]
        },
        "color": "string",
        "colorPalette": 0,
        "colorPaletteReversed": true,
        "colorThresholds": {
          "thresholds": [
            {
              "color": "string",
              "threshold": 0
            }
          ]
        },
        "customData": {
          "connectNulls": "string",
          "dataFields": [
            "string"
          ],
          "isPointColor": true,
          "limitToTopN": 0,
          "lines": true,
          "nameField": "string",
          "pointColorPalette": 0,
          "pointColorPaletteReversed": true,
          "pointScale": "string",
          "pointScaleDataField": "string",
          "seriesCount": 0,
          "splitBy": "string",
          "stack": true,
          "summarizeOthers": true,
          "trellis": true
        },
        "decimals": 0,
        "label": "string",
        "legend": {
          "position": "string",
          "selected": {
            "property1": true,
            "property2": true
          },
          "truncate": true
        },
        "mapDetails": {
          "latitudeField": "string",
          "longitudeField": "string",
          "mapSourceID": "string",
          "mapType": "string",
          "nameField": "string",
          "pointScale": "string",
          "valueField": "string"
        },
        "onClickAction": {
          "search": "string",
          "selectedDashboardId": "string",
          "selectedInputId": "string",
          "selectedLinkId": "string",
          "selectedTimerangeInputId": "string",
          "type": "string"
        },
        "prefix": "string",
        "separator": true,
        "series": [
          {
            "areaStyle": {
              "opacity": 0,
              "shadowBlur": 0,
              "shadowColor": "string",
              "shadowOffsetX": 0,
              "shadowOffsetY": 0
            },
            "color": "string",
            "data": [
              {}
            ],
            "map": "string",
            "name": "string",
            "type": "area",
            "yAxisField": "string"
          }
        ],
        "seriesInfo": {
          "property1": "area",
          "property2": "area"
        },
        "shouldApplyUserChartSettings": true,
        "style": true,
        "suffix": "string",
        "type": "string",
        "xAxis": {
          "dataField": "string",
          "inverse": true,
          "labelInterval": "string",
          "labelOrientation": 0,
          "name": "string",
          "offset": 0,
          "position": "string",
          "type": "string"
        },
        "yAxis": {
          "dataField": [
            "string"
          ],
          "interval": 0,
          "max": 0,
          "min": 0,
          "position": "string",
          "scale": "string",
          "splitLine": true,
          "type": "string"
        }
      },
      "description": "string",
      "displayUsername": "string",
      "earliest": "string",
      "id": "string",
      "isPrivate": true,
      "isSystem": true,
      "latest": "string",
      "lib": "cribl",
      "name": "string",
      "query": "string",
      "resolvedDatasetIds": [
        "string"
      ],
      "sampleRate": 0,
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
      },
      "searchJobSource": "command",
      "tableConfig": {},
      "user": "string"
    }
  ]
}
```

<h3 id="delete-savedquery-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SavedQuery objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-savedquery-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SavedQuery](#schemasavedquery)]|false|none|none|
|»» chartConfig|[ChartConfig](#schemachartconfig)|false|none|none|
|»»» applyThreshold|boolean|false|none|none|
|»»» axis|object|false|none|none|
|»»»» xAxis|string|false|none|none|
|»»»» yAxis|[string]|false|none|none|
|»»»» yAxisExcluded|[string]|false|none|none|
|»»» color|string|false|none|none|
|»»» colorPalette|number|true|none|none|
|»»» colorPaletteReversed|boolean|false|none|none|
|»»» colorThresholds|object|false|none|none|
|»»»» thresholds|[object]|true|none|none|
|»»»»» color|string|true|none|none|
|»»»»» threshold|number|true|none|none|
|»»» customData|object|false|none|none|
|»»»» connectNulls|string|false|none|none|
|»»»» dataFields|[string]|false|none|none|
|»»»» isPointColor|boolean|false|none|none|
|»»»» limitToTopN|number|false|none|none|
|»»»» lines|boolean|false|none|none|
|»»»» nameField|string|false|none|none|
|»»»» pointColorPalette|number|false|none|none|
|»»»» pointColorPaletteReversed|boolean|false|none|none|
|»»»» pointScale|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» pointScaleDataField|string|false|none|none|
|»»»» seriesCount|number|false|none|none|
|»»»» splitBy|string|false|none|none|
|»»»» stack|boolean|false|none|none|
|»»»» summarizeOthers|boolean|false|none|none|
|»»»» trellis|boolean|false|none|none|
|»»» decimals|number|false|none|none|
|»»» label|string|false|none|none|
|»»» legend|object|false|none|none|
|»»»» position|string|false|none|none|
|»»»» selected|object|false|none|none|
|»»»»» **additionalProperties**|boolean|false|none|none|
|»»»» truncate|boolean|false|none|none|
|»»» mapDetails|object|false|none|none|
|»»»» latitudeField|string|false|none|none|
|»»»» longitudeField|string|false|none|none|
|»»»» mapSourceID|string|false|none|none|
|»»»» mapType|string|false|none|none|
|»»»» nameField|string|false|none|none|
|»»»» pointScale|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» valueField|string|false|none|none|
|»»» onClickAction|object|false|none|none|
|»»»» search|string|false|none|none|
|»»»» selectedDashboardId|string|false|none|none|
|»»»» selectedInputId|string|false|none|none|
|»»»» selectedLinkId|string|false|none|none|
|»»»» selectedTimerangeInputId|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» prefix|string|false|none|none|
|»»» separator|boolean|false|none|none|
|»»» series|[[ChartSeries](#schemachartseries)]|false|none|none|
|»»»» areaStyle|[AreaStyleOption](#schemaareastyleoption)|false|none|none|
|»»»»» opacity|number|false|none|none|
|»»»»» shadowBlur|number|false|none|none|
|»»»»» shadowColor|string|false|none|none|
|»»»»» shadowOffsetX|number|false|none|none|
|»»»»» shadowOffsetY|number|false|none|none|
|»»»» color|string|false|none|none|
|»»»» data|[object]|false|none|none|
|»»»» map|string|false|none|none|
|»»»» name|string|true|none|none|
|»»»» type|[ChartType](#schemacharttype)|false|none|none|
|»»»» yAxisField|string|false|none|none|
|»»» seriesInfo|object|false|none|none|
|»»»» **additionalProperties**|[ChartType](#schemacharttype)|false|none|none|
|»»» shouldApplyUserChartSettings|boolean|false|none|none|
|»»» style|boolean|false|none|none|
|»»» suffix|string|false|none|none|
|»»» type|string|true|none|none|
|»»» xAxis|object|false|none|none|
|»»»» dataField|string|false|none|none|
|»»»» inverse|boolean|false|none|none|
|»»»» labelInterval|string|false|none|none|
|»»»» labelOrientation|number|false|none|none|
|»»»» name|string|false|none|none|
|»»»» offset|number|false|none|none|
|»»»» position|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» yAxis|object|false|none|none|
|»»»» dataField|[string]|false|none|none|
|»»»» interval|number|false|none|none|
|»»»» max|number|false|none|none|
|»»»» min|number|false|none|none|
|»»»» position|string|false|none|none|
|»»»» scale|string|false|none|none|
|»»»» splitLine|boolean|false|none|none|
|»»»» type|string|false|none|none|
|»» description|string|false|none|none|
|»» displayUsername|string|false|none|none|
|»» earliest|string|false|none|none|
|»» id|string|true|none|none|
|»» isPrivate|boolean|false|none|none|
|»» isSystem|boolean|false|none|none|
|»» latest|string|false|none|none|
|»» lib|[CriblLib](#schemacribllib)|false|none|none|
|»» name|string|true|none|none|
|»» query|string|true|none|none|
|»» resolvedDatasetIds|[string]|false|none|none|
|»» sampleRate|number|false|none|none|
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
|»» searchJobSource|string|false|none|none|
|»» tableConfig|object|false|none|none|
|»» user|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|area|
|type|column|
|type|events|
|type|funnel|
|type|gauge|
|type|horizontalBar|
|type|line|
|type|map|
|type|pie|
|type|scatter|
|type|single|
|type|table|
|**additionalProperties**|area|
|**additionalProperties**|column|
|**additionalProperties**|events|
|**additionalProperties**|funnel|
|**additionalProperties**|gauge|
|**additionalProperties**|horizontalBar|
|**additionalProperties**|line|
|**additionalProperties**|map|
|**additionalProperties**|pie|
|**additionalProperties**|scatter|
|**additionalProperties**|single|
|**additionalProperties**|table|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|
|searchJobSource|command|
|searchJobSource|standard|
|searchJobSource|scheduled|
|searchJobSource|datatypePreview|
|searchJobSource|dashboard|
|searchJobSource|notebook|
|searchJobSource|systemInsights|

<aside class="success">
This operation does not require authentication
</aside>

## Get SavedQuery by ID

<a id="opIdgetSavedQueryById"></a>

> Code samples

`GET /search/saved/{id}`

Get SavedQuery by ID

<h3 id="get-savedquery-by-id-parameters">Parameters</h3>

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
      "chartConfig": {
        "applyThreshold": true,
        "axis": {
          "xAxis": "string",
          "yAxis": [
            "string"
          ],
          "yAxisExcluded": [
            "string"
          ]
        },
        "color": "string",
        "colorPalette": 0,
        "colorPaletteReversed": true,
        "colorThresholds": {
          "thresholds": [
            {
              "color": "string",
              "threshold": 0
            }
          ]
        },
        "customData": {
          "connectNulls": "string",
          "dataFields": [
            "string"
          ],
          "isPointColor": true,
          "limitToTopN": 0,
          "lines": true,
          "nameField": "string",
          "pointColorPalette": 0,
          "pointColorPaletteReversed": true,
          "pointScale": "string",
          "pointScaleDataField": "string",
          "seriesCount": 0,
          "splitBy": "string",
          "stack": true,
          "summarizeOthers": true,
          "trellis": true
        },
        "decimals": 0,
        "label": "string",
        "legend": {
          "position": "string",
          "selected": {
            "property1": true,
            "property2": true
          },
          "truncate": true
        },
        "mapDetails": {
          "latitudeField": "string",
          "longitudeField": "string",
          "mapSourceID": "string",
          "mapType": "string",
          "nameField": "string",
          "pointScale": "string",
          "valueField": "string"
        },
        "onClickAction": {
          "search": "string",
          "selectedDashboardId": "string",
          "selectedInputId": "string",
          "selectedLinkId": "string",
          "selectedTimerangeInputId": "string",
          "type": "string"
        },
        "prefix": "string",
        "separator": true,
        "series": [
          {
            "areaStyle": {
              "opacity": 0,
              "shadowBlur": 0,
              "shadowColor": "string",
              "shadowOffsetX": 0,
              "shadowOffsetY": 0
            },
            "color": "string",
            "data": [
              {}
            ],
            "map": "string",
            "name": "string",
            "type": "area",
            "yAxisField": "string"
          }
        ],
        "seriesInfo": {
          "property1": "area",
          "property2": "area"
        },
        "shouldApplyUserChartSettings": true,
        "style": true,
        "suffix": "string",
        "type": "string",
        "xAxis": {
          "dataField": "string",
          "inverse": true,
          "labelInterval": "string",
          "labelOrientation": 0,
          "name": "string",
          "offset": 0,
          "position": "string",
          "type": "string"
        },
        "yAxis": {
          "dataField": [
            "string"
          ],
          "interval": 0,
          "max": 0,
          "min": 0,
          "position": "string",
          "scale": "string",
          "splitLine": true,
          "type": "string"
        }
      },
      "description": "string",
      "displayUsername": "string",
      "earliest": "string",
      "id": "string",
      "isPrivate": true,
      "isSystem": true,
      "latest": "string",
      "lib": "cribl",
      "name": "string",
      "query": "string",
      "resolvedDatasetIds": [
        "string"
      ],
      "sampleRate": 0,
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
      },
      "searchJobSource": "command",
      "tableConfig": {},
      "user": "string"
    }
  ]
}
```

<h3 id="get-savedquery-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SavedQuery objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-savedquery-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SavedQuery](#schemasavedquery)]|false|none|none|
|»» chartConfig|[ChartConfig](#schemachartconfig)|false|none|none|
|»»» applyThreshold|boolean|false|none|none|
|»»» axis|object|false|none|none|
|»»»» xAxis|string|false|none|none|
|»»»» yAxis|[string]|false|none|none|
|»»»» yAxisExcluded|[string]|false|none|none|
|»»» color|string|false|none|none|
|»»» colorPalette|number|true|none|none|
|»»» colorPaletteReversed|boolean|false|none|none|
|»»» colorThresholds|object|false|none|none|
|»»»» thresholds|[object]|true|none|none|
|»»»»» color|string|true|none|none|
|»»»»» threshold|number|true|none|none|
|»»» customData|object|false|none|none|
|»»»» connectNulls|string|false|none|none|
|»»»» dataFields|[string]|false|none|none|
|»»»» isPointColor|boolean|false|none|none|
|»»»» limitToTopN|number|false|none|none|
|»»»» lines|boolean|false|none|none|
|»»»» nameField|string|false|none|none|
|»»»» pointColorPalette|number|false|none|none|
|»»»» pointColorPaletteReversed|boolean|false|none|none|
|»»»» pointScale|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» pointScaleDataField|string|false|none|none|
|»»»» seriesCount|number|false|none|none|
|»»»» splitBy|string|false|none|none|
|»»»» stack|boolean|false|none|none|
|»»»» summarizeOthers|boolean|false|none|none|
|»»»» trellis|boolean|false|none|none|
|»»» decimals|number|false|none|none|
|»»» label|string|false|none|none|
|»»» legend|object|false|none|none|
|»»»» position|string|false|none|none|
|»»»» selected|object|false|none|none|
|»»»»» **additionalProperties**|boolean|false|none|none|
|»»»» truncate|boolean|false|none|none|
|»»» mapDetails|object|false|none|none|
|»»»» latitudeField|string|false|none|none|
|»»»» longitudeField|string|false|none|none|
|»»»» mapSourceID|string|false|none|none|
|»»»» mapType|string|false|none|none|
|»»»» nameField|string|false|none|none|
|»»»» pointScale|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» valueField|string|false|none|none|
|»»» onClickAction|object|false|none|none|
|»»»» search|string|false|none|none|
|»»»» selectedDashboardId|string|false|none|none|
|»»»» selectedInputId|string|false|none|none|
|»»»» selectedLinkId|string|false|none|none|
|»»»» selectedTimerangeInputId|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» prefix|string|false|none|none|
|»»» separator|boolean|false|none|none|
|»»» series|[[ChartSeries](#schemachartseries)]|false|none|none|
|»»»» areaStyle|[AreaStyleOption](#schemaareastyleoption)|false|none|none|
|»»»»» opacity|number|false|none|none|
|»»»»» shadowBlur|number|false|none|none|
|»»»»» shadowColor|string|false|none|none|
|»»»»» shadowOffsetX|number|false|none|none|
|»»»»» shadowOffsetY|number|false|none|none|
|»»»» color|string|false|none|none|
|»»»» data|[object]|false|none|none|
|»»»» map|string|false|none|none|
|»»»» name|string|true|none|none|
|»»»» type|[ChartType](#schemacharttype)|false|none|none|
|»»»» yAxisField|string|false|none|none|
|»»» seriesInfo|object|false|none|none|
|»»»» **additionalProperties**|[ChartType](#schemacharttype)|false|none|none|
|»»» shouldApplyUserChartSettings|boolean|false|none|none|
|»»» style|boolean|false|none|none|
|»»» suffix|string|false|none|none|
|»»» type|string|true|none|none|
|»»» xAxis|object|false|none|none|
|»»»» dataField|string|false|none|none|
|»»»» inverse|boolean|false|none|none|
|»»»» labelInterval|string|false|none|none|
|»»»» labelOrientation|number|false|none|none|
|»»»» name|string|false|none|none|
|»»»» offset|number|false|none|none|
|»»»» position|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» yAxis|object|false|none|none|
|»»»» dataField|[string]|false|none|none|
|»»»» interval|number|false|none|none|
|»»»» max|number|false|none|none|
|»»»» min|number|false|none|none|
|»»»» position|string|false|none|none|
|»»»» scale|string|false|none|none|
|»»»» splitLine|boolean|false|none|none|
|»»»» type|string|false|none|none|
|»» description|string|false|none|none|
|»» displayUsername|string|false|none|none|
|»» earliest|string|false|none|none|
|»» id|string|true|none|none|
|»» isPrivate|boolean|false|none|none|
|»» isSystem|boolean|false|none|none|
|»» latest|string|false|none|none|
|»» lib|[CriblLib](#schemacribllib)|false|none|none|
|»» name|string|true|none|none|
|»» query|string|true|none|none|
|»» resolvedDatasetIds|[string]|false|none|none|
|»» sampleRate|number|false|none|none|
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
|»» searchJobSource|string|false|none|none|
|»» tableConfig|object|false|none|none|
|»» user|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|area|
|type|column|
|type|events|
|type|funnel|
|type|gauge|
|type|horizontalBar|
|type|line|
|type|map|
|type|pie|
|type|scatter|
|type|single|
|type|table|
|**additionalProperties**|area|
|**additionalProperties**|column|
|**additionalProperties**|events|
|**additionalProperties**|funnel|
|**additionalProperties**|gauge|
|**additionalProperties**|horizontalBar|
|**additionalProperties**|line|
|**additionalProperties**|map|
|**additionalProperties**|pie|
|**additionalProperties**|scatter|
|**additionalProperties**|single|
|**additionalProperties**|table|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|
|searchJobSource|command|
|searchJobSource|standard|
|searchJobSource|scheduled|
|searchJobSource|datatypePreview|
|searchJobSource|dashboard|
|searchJobSource|notebook|
|searchJobSource|systemInsights|

<aside class="success">
This operation does not require authentication
</aside>

## Update SavedQuery

<a id="opIdupdateSavedQueryById"></a>

> Code samples

`PATCH /search/saved/{id}`

Update SavedQuery

> Body parameter

```json
{
  "chartConfig": {
    "applyThreshold": true,
    "axis": {
      "xAxis": "string",
      "yAxis": [
        "string"
      ],
      "yAxisExcluded": [
        "string"
      ]
    },
    "color": "string",
    "colorPalette": 0,
    "colorPaletteReversed": true,
    "colorThresholds": {
      "thresholds": [
        {
          "color": "string",
          "threshold": 0
        }
      ]
    },
    "customData": {
      "connectNulls": "string",
      "dataFields": [
        "string"
      ],
      "isPointColor": true,
      "limitToTopN": 0,
      "lines": true,
      "nameField": "string",
      "pointColorPalette": 0,
      "pointColorPaletteReversed": true,
      "pointScale": "string",
      "pointScaleDataField": "string",
      "seriesCount": 0,
      "splitBy": "string",
      "stack": true,
      "summarizeOthers": true,
      "trellis": true
    },
    "decimals": 0,
    "label": "string",
    "legend": {
      "position": "string",
      "selected": {
        "property1": true,
        "property2": true
      },
      "truncate": true
    },
    "mapDetails": {
      "latitudeField": "string",
      "longitudeField": "string",
      "mapSourceID": "string",
      "mapType": "string",
      "nameField": "string",
      "pointScale": "string",
      "valueField": "string"
    },
    "onClickAction": {
      "search": "string",
      "selectedDashboardId": "string",
      "selectedInputId": "string",
      "selectedLinkId": "string",
      "selectedTimerangeInputId": "string",
      "type": "string"
    },
    "prefix": "string",
    "separator": true,
    "series": [
      {
        "areaStyle": {
          "opacity": 0,
          "shadowBlur": 0,
          "shadowColor": "string",
          "shadowOffsetX": 0,
          "shadowOffsetY": 0
        },
        "color": "string",
        "data": [
          {}
        ],
        "map": "string",
        "name": "string",
        "type": "area",
        "yAxisField": "string"
      }
    ],
    "seriesInfo": {
      "property1": "area",
      "property2": "area"
    },
    "shouldApplyUserChartSettings": true,
    "style": true,
    "suffix": "string",
    "type": "string",
    "xAxis": {
      "dataField": "string",
      "inverse": true,
      "labelInterval": "string",
      "labelOrientation": 0,
      "name": "string",
      "offset": 0,
      "position": "string",
      "type": "string"
    },
    "yAxis": {
      "dataField": [
        "string"
      ],
      "interval": 0,
      "max": 0,
      "min": 0,
      "position": "string",
      "scale": "string",
      "splitLine": true,
      "type": "string"
    }
  },
  "description": "string",
  "displayUsername": "string",
  "earliest": "string",
  "id": "string",
  "isPrivate": true,
  "isSystem": true,
  "latest": "string",
  "lib": "cribl",
  "name": "string",
  "query": "string",
  "resolvedDatasetIds": [
    "string"
  ],
  "sampleRate": 0,
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
  },
  "searchJobSource": "command",
  "tableConfig": {},
  "user": "string"
}
```

<h3 id="update-savedquery-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[SavedQuery](#schemasavedquery)|true|SavedQuery object to be updated|
|» chartConfig|body|[ChartConfig](#schemachartconfig)|false|none|
|»» applyThreshold|body|boolean|false|none|
|»» axis|body|object|false|none|
|»»» xAxis|body|string|false|none|
|»»» yAxis|body|[string]|false|none|
|»»» yAxisExcluded|body|[string]|false|none|
|»» color|body|string|false|none|
|»» colorPalette|body|number|true|none|
|»» colorPaletteReversed|body|boolean|false|none|
|»» colorThresholds|body|object|false|none|
|»»» thresholds|body|[object]|true|none|
|»»»» color|body|string|true|none|
|»»»» threshold|body|number|true|none|
|»» customData|body|object|false|none|
|»»» connectNulls|body|string|false|none|
|»»» dataFields|body|[string]|false|none|
|»»» isPointColor|body|boolean|false|none|
|»»» limitToTopN|body|number|false|none|
|»»» lines|body|boolean|false|none|
|»»» nameField|body|string|false|none|
|»»» pointColorPalette|body|number|false|none|
|»»» pointColorPaletteReversed|body|boolean|false|none|
|»»» pointScale|body|any|false|none|
|»»»» *anonymous*|body|string|false|none|
|»»»» *anonymous*|body|number|false|none|
|»»» pointScaleDataField|body|string|false|none|
|»»» seriesCount|body|number|false|none|
|»»» splitBy|body|string|false|none|
|»»» stack|body|boolean|false|none|
|»»» summarizeOthers|body|boolean|false|none|
|»»» trellis|body|boolean|false|none|
|»» decimals|body|number|false|none|
|»» label|body|string|false|none|
|»» legend|body|object|false|none|
|»»» position|body|string|false|none|
|»»» selected|body|object|false|none|
|»»»» **additionalProperties**|body|boolean|false|none|
|»»» truncate|body|boolean|false|none|
|»» mapDetails|body|object|false|none|
|»»» latitudeField|body|string|false|none|
|»»» longitudeField|body|string|false|none|
|»»» mapSourceID|body|string|false|none|
|»»» mapType|body|string|false|none|
|»»» nameField|body|string|false|none|
|»»» pointScale|body|any|false|none|
|»»»» *anonymous*|body|string|false|none|
|»»»» *anonymous*|body|number|false|none|
|»»» valueField|body|string|false|none|
|»» onClickAction|body|object|false|none|
|»»» search|body|string|false|none|
|»»» selectedDashboardId|body|string|false|none|
|»»» selectedInputId|body|string|false|none|
|»»» selectedLinkId|body|string|false|none|
|»»» selectedTimerangeInputId|body|string|false|none|
|»»» type|body|string|false|none|
|»» prefix|body|string|false|none|
|»» separator|body|boolean|false|none|
|»» series|body|[[ChartSeries](#schemachartseries)]|false|none|
|»»» areaStyle|body|[AreaStyleOption](#schemaareastyleoption)|false|none|
|»»»» opacity|body|number|false|none|
|»»»» shadowBlur|body|number|false|none|
|»»»» shadowColor|body|string|false|none|
|»»»» shadowOffsetX|body|number|false|none|
|»»»» shadowOffsetY|body|number|false|none|
|»»» color|body|string|false|none|
|»»» data|body|[object]|false|none|
|»»» map|body|string|false|none|
|»»» name|body|string|true|none|
|»»» type|body|[ChartType](#schemacharttype)|false|none|
|»»» yAxisField|body|string|false|none|
|»» seriesInfo|body|object|false|none|
|»»» **additionalProperties**|body|[ChartType](#schemacharttype)|false|none|
|»» shouldApplyUserChartSettings|body|boolean|false|none|
|»» style|body|boolean|false|none|
|»» suffix|body|string|false|none|
|»» type|body|string|true|none|
|»» xAxis|body|object|false|none|
|»»» dataField|body|string|false|none|
|»»» inverse|body|boolean|false|none|
|»»» labelInterval|body|string|false|none|
|»»» labelOrientation|body|number|false|none|
|»»» name|body|string|false|none|
|»»» offset|body|number|false|none|
|»»» position|body|string|false|none|
|»»» type|body|string|false|none|
|»» yAxis|body|object|false|none|
|»»» dataField|body|[string]|false|none|
|»»» interval|body|number|false|none|
|»»» max|body|number|false|none|
|»»» min|body|number|false|none|
|»»» position|body|string|false|none|
|»»» scale|body|string|false|none|
|»»» splitLine|body|boolean|false|none|
|»»» type|body|string|false|none|
|» description|body|string|false|none|
|» displayUsername|body|string|false|none|
|» earliest|body|string|false|none|
|» id|body|string|true|none|
|» isPrivate|body|boolean|false|none|
|» isSystem|body|boolean|false|none|
|» latest|body|string|false|none|
|» lib|body|[CriblLib](#schemacribllib)|false|none|
|» name|body|string|true|none|
|» query|body|string|true|none|
|» resolvedDatasetIds|body|[string]|false|none|
|» sampleRate|body|number|false|none|
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
|» searchJobSource|body|string|false|none|
|» tableConfig|body|object|false|none|
|» user|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» type|area|
|»»» type|column|
|»»» type|events|
|»»» type|funnel|
|»»» type|gauge|
|»»» type|horizontalBar|
|»»» type|line|
|»»» type|map|
|»»» type|pie|
|»»» type|scatter|
|»»» type|single|
|»»» type|table|
|»»» **additionalProperties**|area|
|»»» **additionalProperties**|column|
|»»» **additionalProperties**|events|
|»»» **additionalProperties**|funnel|
|»»» **additionalProperties**|gauge|
|»»» **additionalProperties**|horizontalBar|
|»»» **additionalProperties**|line|
|»»» **additionalProperties**|map|
|»»» **additionalProperties**|pie|
|»»» **additionalProperties**|scatter|
|»»» **additionalProperties**|single|
|»»» **additionalProperties**|table|
|» lib|cribl|
|» lib|cribl-custom|
|» lib|custom|
|» searchJobSource|command|
|» searchJobSource|standard|
|» searchJobSource|scheduled|
|» searchJobSource|datatypePreview|
|» searchJobSource|dashboard|
|» searchJobSource|notebook|
|» searchJobSource|systemInsights|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "chartConfig": {
        "applyThreshold": true,
        "axis": {
          "xAxis": "string",
          "yAxis": [
            "string"
          ],
          "yAxisExcluded": [
            "string"
          ]
        },
        "color": "string",
        "colorPalette": 0,
        "colorPaletteReversed": true,
        "colorThresholds": {
          "thresholds": [
            {
              "color": "string",
              "threshold": 0
            }
          ]
        },
        "customData": {
          "connectNulls": "string",
          "dataFields": [
            "string"
          ],
          "isPointColor": true,
          "limitToTopN": 0,
          "lines": true,
          "nameField": "string",
          "pointColorPalette": 0,
          "pointColorPaletteReversed": true,
          "pointScale": "string",
          "pointScaleDataField": "string",
          "seriesCount": 0,
          "splitBy": "string",
          "stack": true,
          "summarizeOthers": true,
          "trellis": true
        },
        "decimals": 0,
        "label": "string",
        "legend": {
          "position": "string",
          "selected": {
            "property1": true,
            "property2": true
          },
          "truncate": true
        },
        "mapDetails": {
          "latitudeField": "string",
          "longitudeField": "string",
          "mapSourceID": "string",
          "mapType": "string",
          "nameField": "string",
          "pointScale": "string",
          "valueField": "string"
        },
        "onClickAction": {
          "search": "string",
          "selectedDashboardId": "string",
          "selectedInputId": "string",
          "selectedLinkId": "string",
          "selectedTimerangeInputId": "string",
          "type": "string"
        },
        "prefix": "string",
        "separator": true,
        "series": [
          {
            "areaStyle": {
              "opacity": 0,
              "shadowBlur": 0,
              "shadowColor": "string",
              "shadowOffsetX": 0,
              "shadowOffsetY": 0
            },
            "color": "string",
            "data": [
              {}
            ],
            "map": "string",
            "name": "string",
            "type": "area",
            "yAxisField": "string"
          }
        ],
        "seriesInfo": {
          "property1": "area",
          "property2": "area"
        },
        "shouldApplyUserChartSettings": true,
        "style": true,
        "suffix": "string",
        "type": "string",
        "xAxis": {
          "dataField": "string",
          "inverse": true,
          "labelInterval": "string",
          "labelOrientation": 0,
          "name": "string",
          "offset": 0,
          "position": "string",
          "type": "string"
        },
        "yAxis": {
          "dataField": [
            "string"
          ],
          "interval": 0,
          "max": 0,
          "min": 0,
          "position": "string",
          "scale": "string",
          "splitLine": true,
          "type": "string"
        }
      },
      "description": "string",
      "displayUsername": "string",
      "earliest": "string",
      "id": "string",
      "isPrivate": true,
      "isSystem": true,
      "latest": "string",
      "lib": "cribl",
      "name": "string",
      "query": "string",
      "resolvedDatasetIds": [
        "string"
      ],
      "sampleRate": 0,
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
      },
      "searchJobSource": "command",
      "tableConfig": {},
      "user": "string"
    }
  ]
}
```

<h3 id="update-savedquery-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SavedQuery objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-savedquery-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SavedQuery](#schemasavedquery)]|false|none|none|
|»» chartConfig|[ChartConfig](#schemachartconfig)|false|none|none|
|»»» applyThreshold|boolean|false|none|none|
|»»» axis|object|false|none|none|
|»»»» xAxis|string|false|none|none|
|»»»» yAxis|[string]|false|none|none|
|»»»» yAxisExcluded|[string]|false|none|none|
|»»» color|string|false|none|none|
|»»» colorPalette|number|true|none|none|
|»»» colorPaletteReversed|boolean|false|none|none|
|»»» colorThresholds|object|false|none|none|
|»»»» thresholds|[object]|true|none|none|
|»»»»» color|string|true|none|none|
|»»»»» threshold|number|true|none|none|
|»»» customData|object|false|none|none|
|»»»» connectNulls|string|false|none|none|
|»»»» dataFields|[string]|false|none|none|
|»»»» isPointColor|boolean|false|none|none|
|»»»» limitToTopN|number|false|none|none|
|»»»» lines|boolean|false|none|none|
|»»»» nameField|string|false|none|none|
|»»»» pointColorPalette|number|false|none|none|
|»»»» pointColorPaletteReversed|boolean|false|none|none|
|»»»» pointScale|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» pointScaleDataField|string|false|none|none|
|»»»» seriesCount|number|false|none|none|
|»»»» splitBy|string|false|none|none|
|»»»» stack|boolean|false|none|none|
|»»»» summarizeOthers|boolean|false|none|none|
|»»»» trellis|boolean|false|none|none|
|»»» decimals|number|false|none|none|
|»»» label|string|false|none|none|
|»»» legend|object|false|none|none|
|»»»» position|string|false|none|none|
|»»»» selected|object|false|none|none|
|»»»»» **additionalProperties**|boolean|false|none|none|
|»»»» truncate|boolean|false|none|none|
|»»» mapDetails|object|false|none|none|
|»»»» latitudeField|string|false|none|none|
|»»»» longitudeField|string|false|none|none|
|»»»» mapSourceID|string|false|none|none|
|»»»» mapType|string|false|none|none|
|»»»» nameField|string|false|none|none|
|»»»» pointScale|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» valueField|string|false|none|none|
|»»» onClickAction|object|false|none|none|
|»»»» search|string|false|none|none|
|»»»» selectedDashboardId|string|false|none|none|
|»»»» selectedInputId|string|false|none|none|
|»»»» selectedLinkId|string|false|none|none|
|»»»» selectedTimerangeInputId|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» prefix|string|false|none|none|
|»»» separator|boolean|false|none|none|
|»»» series|[[ChartSeries](#schemachartseries)]|false|none|none|
|»»»» areaStyle|[AreaStyleOption](#schemaareastyleoption)|false|none|none|
|»»»»» opacity|number|false|none|none|
|»»»»» shadowBlur|number|false|none|none|
|»»»»» shadowColor|string|false|none|none|
|»»»»» shadowOffsetX|number|false|none|none|
|»»»»» shadowOffsetY|number|false|none|none|
|»»»» color|string|false|none|none|
|»»»» data|[object]|false|none|none|
|»»»» map|string|false|none|none|
|»»»» name|string|true|none|none|
|»»»» type|[ChartType](#schemacharttype)|false|none|none|
|»»»» yAxisField|string|false|none|none|
|»»» seriesInfo|object|false|none|none|
|»»»» **additionalProperties**|[ChartType](#schemacharttype)|false|none|none|
|»»» shouldApplyUserChartSettings|boolean|false|none|none|
|»»» style|boolean|false|none|none|
|»»» suffix|string|false|none|none|
|»»» type|string|true|none|none|
|»»» xAxis|object|false|none|none|
|»»»» dataField|string|false|none|none|
|»»»» inverse|boolean|false|none|none|
|»»»» labelInterval|string|false|none|none|
|»»»» labelOrientation|number|false|none|none|
|»»»» name|string|false|none|none|
|»»»» offset|number|false|none|none|
|»»»» position|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» yAxis|object|false|none|none|
|»»»» dataField|[string]|false|none|none|
|»»»» interval|number|false|none|none|
|»»»» max|number|false|none|none|
|»»»» min|number|false|none|none|
|»»»» position|string|false|none|none|
|»»»» scale|string|false|none|none|
|»»»» splitLine|boolean|false|none|none|
|»»»» type|string|false|none|none|
|»» description|string|false|none|none|
|»» displayUsername|string|false|none|none|
|»» earliest|string|false|none|none|
|»» id|string|true|none|none|
|»» isPrivate|boolean|false|none|none|
|»» isSystem|boolean|false|none|none|
|»» latest|string|false|none|none|
|»» lib|[CriblLib](#schemacribllib)|false|none|none|
|»» name|string|true|none|none|
|»» query|string|true|none|none|
|»» resolvedDatasetIds|[string]|false|none|none|
|»» sampleRate|number|false|none|none|
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
|»» searchJobSource|string|false|none|none|
|»» tableConfig|object|false|none|none|
|»» user|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|area|
|type|column|
|type|events|
|type|funnel|
|type|gauge|
|type|horizontalBar|
|type|line|
|type|map|
|type|pie|
|type|scatter|
|type|single|
|type|table|
|**additionalProperties**|area|
|**additionalProperties**|column|
|**additionalProperties**|events|
|**additionalProperties**|funnel|
|**additionalProperties**|gauge|
|**additionalProperties**|horizontalBar|
|**additionalProperties**|line|
|**additionalProperties**|map|
|**additionalProperties**|pie|
|**additionalProperties**|scatter|
|**additionalProperties**|single|
|**additionalProperties**|table|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|
|searchJobSource|command|
|searchJobSource|standard|
|searchJobSource|scheduled|
|searchJobSource|datatypePreview|
|searchJobSource|dashboard|
|searchJobSource|notebook|
|searchJobSource|systemInsights|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_SavedQuery">SavedQuery</h2>
<!-- backwards compatibility -->
<a id="schemasavedquery"></a>
<a id="schema_SavedQuery"></a>
<a id="tocSsavedquery"></a>
<a id="tocssavedquery"></a>

```json
{
  "chartConfig": {
    "applyThreshold": true,
    "axis": {
      "xAxis": "string",
      "yAxis": [
        "string"
      ],
      "yAxisExcluded": [
        "string"
      ]
    },
    "color": "string",
    "colorPalette": 0,
    "colorPaletteReversed": true,
    "colorThresholds": {
      "thresholds": [
        {
          "color": "string",
          "threshold": 0
        }
      ]
    },
    "customData": {
      "connectNulls": "string",
      "dataFields": [
        "string"
      ],
      "isPointColor": true,
      "limitToTopN": 0,
      "lines": true,
      "nameField": "string",
      "pointColorPalette": 0,
      "pointColorPaletteReversed": true,
      "pointScale": "string",
      "pointScaleDataField": "string",
      "seriesCount": 0,
      "splitBy": "string",
      "stack": true,
      "summarizeOthers": true,
      "trellis": true
    },
    "decimals": 0,
    "label": "string",
    "legend": {
      "position": "string",
      "selected": {
        "property1": true,
        "property2": true
      },
      "truncate": true
    },
    "mapDetails": {
      "latitudeField": "string",
      "longitudeField": "string",
      "mapSourceID": "string",
      "mapType": "string",
      "nameField": "string",
      "pointScale": "string",
      "valueField": "string"
    },
    "onClickAction": {
      "search": "string",
      "selectedDashboardId": "string",
      "selectedInputId": "string",
      "selectedLinkId": "string",
      "selectedTimerangeInputId": "string",
      "type": "string"
    },
    "prefix": "string",
    "separator": true,
    "series": [
      {
        "areaStyle": {
          "opacity": 0,
          "shadowBlur": 0,
          "shadowColor": "string",
          "shadowOffsetX": 0,
          "shadowOffsetY": 0
        },
        "color": "string",
        "data": [
          {}
        ],
        "map": "string",
        "name": "string",
        "type": "area",
        "yAxisField": "string"
      }
    ],
    "seriesInfo": {
      "property1": "area",
      "property2": "area"
    },
    "shouldApplyUserChartSettings": true,
    "style": true,
    "suffix": "string",
    "type": "string",
    "xAxis": {
      "dataField": "string",
      "inverse": true,
      "labelInterval": "string",
      "labelOrientation": 0,
      "name": "string",
      "offset": 0,
      "position": "string",
      "type": "string"
    },
    "yAxis": {
      "dataField": [
        "string"
      ],
      "interval": 0,
      "max": 0,
      "min": 0,
      "position": "string",
      "scale": "string",
      "splitLine": true,
      "type": "string"
    }
  },
  "description": "string",
  "displayUsername": "string",
  "earliest": "string",
  "id": "string",
  "isPrivate": true,
  "isSystem": true,
  "latest": "string",
  "lib": "cribl",
  "name": "string",
  "query": "string",
  "resolvedDatasetIds": [
    "string"
  ],
  "sampleRate": 0,
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
  },
  "searchJobSource": "command",
  "tableConfig": {},
  "user": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|chartConfig|[ChartConfig](#schemachartconfig)|false|none|none|
|description|string|false|none|none|
|displayUsername|string|false|none|none|
|earliest|string|false|none|none|
|id|string|true|none|none|
|isPrivate|boolean|false|none|none|
|isSystem|boolean|false|none|none|
|latest|string|false|none|none|
|lib|[CriblLib](#schemacribllib)|false|none|none|
|name|string|true|none|none|
|query|string|true|none|none|
|resolvedDatasetIds|[string]|false|none|none|
|sampleRate|number|false|none|none|
|schedule|[SavedQuerySchedule](#schemasavedqueryschedule)|false|none|none|
|searchJobSource|string|false|none|none|
|tableConfig|object|false|none|none|
|user|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|searchJobSource|command|
|searchJobSource|standard|
|searchJobSource|scheduled|
|searchJobSource|datatypePreview|
|searchJobSource|dashboard|
|searchJobSource|notebook|
|searchJobSource|systemInsights|

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

<h2 id="tocS_ChartConfig">ChartConfig</h2>
<!-- backwards compatibility -->
<a id="schemachartconfig"></a>
<a id="schema_ChartConfig"></a>
<a id="tocSchartconfig"></a>
<a id="tocschartconfig"></a>

```json
{
  "applyThreshold": true,
  "axis": {
    "xAxis": "string",
    "yAxis": [
      "string"
    ],
    "yAxisExcluded": [
      "string"
    ]
  },
  "color": "string",
  "colorPalette": 0,
  "colorPaletteReversed": true,
  "colorThresholds": {
    "thresholds": [
      {
        "color": "string",
        "threshold": 0
      }
    ]
  },
  "customData": {
    "connectNulls": "string",
    "dataFields": [
      "string"
    ],
    "isPointColor": true,
    "limitToTopN": 0,
    "lines": true,
    "nameField": "string",
    "pointColorPalette": 0,
    "pointColorPaletteReversed": true,
    "pointScale": "string",
    "pointScaleDataField": "string",
    "seriesCount": 0,
    "splitBy": "string",
    "stack": true,
    "summarizeOthers": true,
    "trellis": true
  },
  "decimals": 0,
  "label": "string",
  "legend": {
    "position": "string",
    "selected": {
      "property1": true,
      "property2": true
    },
    "truncate": true
  },
  "mapDetails": {
    "latitudeField": "string",
    "longitudeField": "string",
    "mapSourceID": "string",
    "mapType": "string",
    "nameField": "string",
    "pointScale": "string",
    "valueField": "string"
  },
  "onClickAction": {
    "search": "string",
    "selectedDashboardId": "string",
    "selectedInputId": "string",
    "selectedLinkId": "string",
    "selectedTimerangeInputId": "string",
    "type": "string"
  },
  "prefix": "string",
  "separator": true,
  "series": [
    {
      "areaStyle": {
        "opacity": 0,
        "shadowBlur": 0,
        "shadowColor": "string",
        "shadowOffsetX": 0,
        "shadowOffsetY": 0
      },
      "color": "string",
      "data": [
        {}
      ],
      "map": "string",
      "name": "string",
      "type": "area",
      "yAxisField": "string"
    }
  ],
  "seriesInfo": {
    "property1": "area",
    "property2": "area"
  },
  "shouldApplyUserChartSettings": true,
  "style": true,
  "suffix": "string",
  "type": "string",
  "xAxis": {
    "dataField": "string",
    "inverse": true,
    "labelInterval": "string",
    "labelOrientation": 0,
    "name": "string",
    "offset": 0,
    "position": "string",
    "type": "string"
  },
  "yAxis": {
    "dataField": [
      "string"
    ],
    "interval": 0,
    "max": 0,
    "min": 0,
    "position": "string",
    "scale": "string",
    "splitLine": true,
    "type": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|applyThreshold|boolean|false|none|none|
|axis|object|false|none|none|
|» xAxis|string|false|none|none|
|» yAxis|[string]|false|none|none|
|» yAxisExcluded|[string]|false|none|none|
|color|string|false|none|none|
|colorPalette|number|true|none|none|
|colorPaletteReversed|boolean|false|none|none|
|colorThresholds|object|false|none|none|
|» thresholds|[object]|true|none|none|
|»» color|string|true|none|none|
|»» threshold|number|true|none|none|
|customData|object|false|none|none|
|» connectNulls|string|false|none|none|
|» dataFields|[string]|false|none|none|
|» isPointColor|boolean|false|none|none|
|» limitToTopN|number|false|none|none|
|» lines|boolean|false|none|none|
|» nameField|string|false|none|none|
|» pointColorPalette|number|false|none|none|
|» pointColorPaletteReversed|boolean|false|none|none|
|» pointScale|any|false|none|none|

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
|» pointScaleDataField|string|false|none|none|
|» seriesCount|number|false|none|none|
|» splitBy|string|false|none|none|
|» stack|boolean|false|none|none|
|» summarizeOthers|boolean|false|none|none|
|» trellis|boolean|false|none|none|
|decimals|number|false|none|none|
|label|string|false|none|none|
|legend|object|false|none|none|
|» position|string|false|none|none|
|» selected|object|false|none|none|
|»» **additionalProperties**|boolean|false|none|none|
|» truncate|boolean|false|none|none|
|mapDetails|object|false|none|none|
|» latitudeField|string|false|none|none|
|» longitudeField|string|false|none|none|
|» mapSourceID|string|false|none|none|
|» mapType|string|false|none|none|
|» nameField|string|false|none|none|
|» pointScale|any|false|none|none|

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
|» valueField|string|false|none|none|
|onClickAction|object|false|none|none|
|» search|string|false|none|none|
|» selectedDashboardId|string|false|none|none|
|» selectedInputId|string|false|none|none|
|» selectedLinkId|string|false|none|none|
|» selectedTimerangeInputId|string|false|none|none|
|» type|string|false|none|none|
|prefix|string|false|none|none|
|separator|boolean|false|none|none|
|series|[[ChartSeries](#schemachartseries)]|false|none|none|
|seriesInfo|object|false|none|none|
|» **additionalProperties**|[ChartType](#schemacharttype)|false|none|none|
|shouldApplyUserChartSettings|boolean|false|none|none|
|style|boolean|false|none|none|
|suffix|string|false|none|none|
|type|string|true|none|none|
|xAxis|object|false|none|none|
|» dataField|string|false|none|none|
|» inverse|boolean|false|none|none|
|» labelInterval|string|false|none|none|
|» labelOrientation|number|false|none|none|
|» name|string|false|none|none|
|» offset|number|false|none|none|
|» position|string|false|none|none|
|» type|string|false|none|none|
|yAxis|object|false|none|none|
|» dataField|[string]|false|none|none|
|» interval|number|false|none|none|
|» max|number|false|none|none|
|» min|number|false|none|none|
|» position|string|false|none|none|
|» scale|string|false|none|none|
|» splitLine|boolean|false|none|none|
|» type|string|false|none|none|

<h2 id="tocS_CriblLib">CriblLib</h2>
<!-- backwards compatibility -->
<a id="schemacribllib"></a>
<a id="schema_CriblLib"></a>
<a id="tocScribllib"></a>
<a id="tocscribllib"></a>

```json
"cribl"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|cribl|
|*anonymous*|cribl-custom|
|*anonymous*|custom|

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

<h2 id="tocS_ChartSeries">ChartSeries</h2>
<!-- backwards compatibility -->
<a id="schemachartseries"></a>
<a id="schema_ChartSeries"></a>
<a id="tocSchartseries"></a>
<a id="tocschartseries"></a>

```json
{
  "areaStyle": {
    "opacity": 0,
    "shadowBlur": 0,
    "shadowColor": "string",
    "shadowOffsetX": 0,
    "shadowOffsetY": 0
  },
  "color": "string",
  "data": [
    {}
  ],
  "map": "string",
  "name": "string",
  "type": "area",
  "yAxisField": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|areaStyle|[AreaStyleOption](#schemaareastyleoption)|false|none|none|
|color|string|false|none|none|
|data|[ChartData](#schemachartdata)|false|none|none|
|map|string|false|none|none|
|name|string|true|none|none|
|type|[ChartType](#schemacharttype)|false|none|none|
|yAxisField|string|false|none|none|

<h2 id="tocS_ChartType">ChartType</h2>
<!-- backwards compatibility -->
<a id="schemacharttype"></a>
<a id="schema_ChartType"></a>
<a id="tocScharttype"></a>
<a id="tocscharttype"></a>

```json
"area"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|area|
|*anonymous*|column|
|*anonymous*|events|
|*anonymous*|funnel|
|*anonymous*|gauge|
|*anonymous*|horizontalBar|
|*anonymous*|line|
|*anonymous*|map|
|*anonymous*|pie|
|*anonymous*|scatter|
|*anonymous*|single|
|*anonymous*|table|

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

<h2 id="tocS_AreaStyleOption">AreaStyleOption</h2>
<!-- backwards compatibility -->
<a id="schemaareastyleoption"></a>
<a id="schema_AreaStyleOption"></a>
<a id="tocSareastyleoption"></a>
<a id="tocsareastyleoption"></a>

```json
{
  "opacity": 0,
  "shadowBlur": 0,
  "shadowColor": "string",
  "shadowOffsetX": 0,
  "shadowOffsetY": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|opacity|number|false|none|none|
|shadowBlur|number|false|none|none|
|shadowColor|string|false|none|none|
|shadowOffsetX|number|false|none|none|
|shadowOffsetY|number|false|none|none|

<h2 id="tocS_ChartData">ChartData</h2>
<!-- backwards compatibility -->
<a id="schemachartdata"></a>
<a id="schema_ChartData"></a>
<a id="tocSchartdata"></a>
<a id="tocschartdata"></a>

```json
[
  {}
]

```

### Properties

*None*

