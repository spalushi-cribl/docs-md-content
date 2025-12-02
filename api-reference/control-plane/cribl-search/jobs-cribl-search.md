
<h1 id="cribl-search-api-jobs-cribl-search-">Cribl Search API - Jobs (Cribl Search) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-search-api-jobs-cribl-search--jobs-cribl-search-">Jobs (Cribl Search)</h1>

## Get a list of SearchJob objects

<a id="opIdlistSearchJob"></a>

> Code samples

`GET /search/jobs`

Get a list of SearchJob objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "accelerated": true,
      "aliasOfOriginalJobId": "string",
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
      "compatibilityChecks": {
        "datatypes": true,
        "stageIds": [
          "string"
        ]
      },
      "completionInfo": "string",
      "context": "string",
      "correlationId": "string",
      "cpuMetrics": {
        "billableCPUSeconds": 0,
        "executorsCPUSeconds": {
          "property1": 0,
          "property2": 0
        },
        "totalCPUSeconds": 0,
        "totalExecCPUSeconds": 0
      },
      "datatypeOverrides": {
        "breakerRulesets": [
          {
            "id": "string",
            "lib": "custom",
            "description": "string",
            "tags": "string",
            "minRawLength": 256,
            "rules": [
              {
                "name": "string",
                "condition": "true",
                "type": "regex",
                "timestampAnchorRegex": "/^/",
                "timestamp": {
                  "type": "auto",
                  "length": 150,
                  "format": "string"
                },
                "timestampTimezone": "local",
                "timestampEarliest": "-420weeks",
                "timestampLatest": "+1week",
                "maxEventBytes": 51200,
                "fields": [
                  {
                    "name": "string",
                    "value": "string"
                  }
                ],
                "disabled": false,
                "parserEnabled": false,
                "shouldUseDataRaw": false
              }
            ]
          }
        ],
        "disableBreakers": true
      },
      "disableNotifications": true,
      "displayUsername": "string",
      "earliest": "string",
      "earliestEpoch": 0,
      "errorStateConfig": {
        "coordinated": true,
        "errorMessages": [
          "string"
        ]
      },
      "group": "string",
      "id": "string",
      "isPrivate": true,
      "latest": "string",
      "latestEpoch": 0,
      "metadata": {
        "arguments": {
          "property1": [
            {
              "property1": "string",
              "property2": "string"
            }
          ],
          "property2": [
            {
              "property1": "string",
              "property2": "string"
            }
          ]
        },
        "computeTypes": {
          "v1": 0,
          "v2": 0,
          "lakehouse": 0
        },
        "datasets": {
          "property1": 0,
          "property2": 0
        },
        "functions": {
          "property1": 0,
          "property2": 0
        },
        "operators": {
          "property1": 0,
          "property2": 0
        },
        "providerTypes": {
          "property1": 0,
          "property2": 0
        },
        "providers": {
          "property1": 0,
          "property2": 0
        }
      },
      "notebookId": "string",
      "numEventsAfter": 0,
      "numEventsBefore": 0,
      "query": "string",
      "queryWithMacrosResolved": "string",
      "sampleRate": 0,
      "savedQueryName": "string",
      "searchParameterDeclarations": [
        {
          "defaultValue": "string",
          "name": "string",
          "type": "string"
        }
      ],
      "searchParameterValues": {
        "property1": "string",
        "property2": "string"
      },
      "setOptions": {
        "property1": "string",
        "property2": "string"
      },
      "stages": [
        {
          "cacheStatusByDatasetId": {
            "property1": {
              "usedCache": true
            },
            "property2": {
              "usedCache": true
            }
          },
          "dependencies": [
            {
              "dependentFields": [
                "string"
              ],
              "id": "string",
              "type": "stage"
            }
          ],
          "earliest": "string",
          "executionWarnings": [
            {
              "text": "string",
              "type": "string"
            }
          ],
          "filter": "string",
          "id": "string",
          "latest": "string",
          "resolvedDatasetIds": [
            "string"
          ],
          "searchConfig": {
            "canComputeMetadataDistributively": true,
            "datasets": [
              "string"
            ],
            "hasSendOperator": true,
            "logicalPlans": {
              "Combined": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              },
              "Coordinated": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              },
              "Federated": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              }
            },
            "orderedFieldNames": [
              "string"
            ],
            "pipelines": {
              "Combined": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              },
              "Coordinated": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              },
              "Federated": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              }
            },
            "referencedColumnPaths": [
              [
                "string"
              ]
            ],
            "searchTerms": [
              {
                "isCaseSensitive": true,
                "term": "string"
              }
            ],
            "sortFields": [
              {
                "direction": "ascending",
                "fieldName": "string",
                "nullPosition": "nullsFirst"
              }
            ],
            "useFormattedVisualization": true
          },
          "status": "failed",
          "subQueryText": "string"
        }
      ],
      "status": "failed",
      "tableConfig": {
        "columnFilterSettings": {
          "contains": {}
        },
        "columnFormatSettings": {
          "palette": {},
          "precision": {},
          "prefix": {},
          "suffix": {}
        },
        "columnOrderSettings": {
          "order": {}
        },
        "columnSortSettings": {
          "sort": {}
        },
        "eventDetailsPanel": true,
        "eventTableFields": [
          "string"
        ],
        "rowNumberColumnWidth": 0,
        "showColumnTotals": true,
        "showColumnTotalsPinned": true,
        "showRowNumbers": true,
        "showRowTotals": true,
        "showRowTotalsPinned": true,
        "wrapCells": true
      },
      "targetEventTime": 0,
      "timeCompleted": 0,
      "timeCreated": 0,
      "timeStarted": 0,
      "timeToFirstByte": 0,
      "totalBytesScanned": 0,
      "totalEventCount": 0,
      "type": "command",
      "usageGroupId": "string",
      "usageMetrics": {
        "bytesIn": 0,
        "bytesOut": 0,
        "eventsIn": 0,
        "eventsOut": 0,
        "objects": {
          "discovered": 0,
          "scanned": 0,
          "skipped": 0
        },
        "tasks": {
          "largeFileTaskCount": 0,
          "standardTaskCount": 0
        },
        "time": {
          "queuedSec": 0,
          "runningSec": 0,
          "taskCompletionTotalSec": 0,
          "taskReceivingTotalSec": 0
        }
      },
      "user": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-searchjob-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchJob objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-searchjob-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchJob](#schemasearchjob)]|false|none|none|
|»» accelerated|boolean|false|none|none|
|»» aliasOfOriginalJobId|string|false|none|none|
|»» chartConfig|object|false|none|none|
|»»» applyThreshold|boolean|false|none|none|
|»»» axis|object|false|none|none|
|»»»» xAxis|string|false|none|none|
|»»»» yAxis|[string]|false|none|none|
|»»»» yAxisExcluded|[string]|false|none|none|
|»»» color|string|false|none|none|
|»»» colorPalette|number|false|none|none|
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
|»»» type|string|false|none|none|
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
|»» compatibilityChecks|object|false|none|none|
|»»» datatypes|boolean|false|none|none|
|»»» stageIds|[string]|false|none|none|
|»» completionInfo|string|false|none|none|
|»» context|string|false|none|none|
|»» correlationId|string|false|none|none|
|»» cpuMetrics|[CPUTimeMetric](#schemacputimemetric)|false|none|none|
|»»» billableCPUSeconds|number|false|none|none|
|»»» executorsCPUSeconds|object|true|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» totalCPUSeconds|number|true|none|none|
|»»» totalExecCPUSeconds|number|true|none|none|
|»» datatypeOverrides|[DatatypeOverrides](#schemadatatypeoverrides)|false|none|none|
|»»» breakerRulesets|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|none|
|»»»» id|string|true|none|none|
|»»»» lib|string|false|none|none|
|»»»» description|string|false|none|none|
|»»»» tags|string|false|none|none|
|»»»» minRawLength|number|false|none|The  minimum number of characters in _raw to determine which rule to use|
|»»»» rules|[object]|false|none|A list of rules that will be applied, in order, to the input data stream|
|»»»»» name|string|true|none|none|
|»»»»» condition|string|true|none|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»»»»» type|string|true|none|none|
|»»»»» timestampAnchorRegex|string|true|none|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»»»»» timestamp|object|true|none|Auto, manual format (strptime), or current time|
|»»»»»» type|string|true|none|none|
|»»»»»» length|number|false|none|none|
|»»»»»» format|string|false|none|none|
|»»»»» timestampTimezone|string|false|none|Timezone to assign to timestamps without timezone info|
|»»»»» timestampEarliest|string|false|none|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»»»»» timestampLatest|string|false|none|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»»»»» maxEventBytes|number|false|none|The maximum number of bytes in an event before it is flushed to the pipelines|
|»»»»» fields|[object]|false|none|Key-value pairs to be added to each event|
|»»»»»» name|string|false|none|none|
|»»»»»» value|string|true|none|The JavaScript expression used to compute the field's value (can be constant)|
|»»»»» disabled|boolean|false|none|Disable this breaker rule (enabled by default)|
|»»»»» parserEnabled|boolean|false|none|none|
|»»»»» shouldUseDataRaw|boolean|false|none|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|
|»»» disableBreakers|boolean|true|none|none|
|»» disableNotifications|boolean|false|none|none|
|»» displayUsername|string|true|none|none|
|»» earliest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» earliestEpoch|number|false|none|none|
|»» errorStateConfig|[SearchJobErrorStateConfig](#schemasearchjoberrorstateconfig)|false|none|none|
|»»» coordinated|boolean|true|none|none|
|»»» errorMessages|[string]|true|none|none|
|»» group|string|true|none|none|
|»» id|string|true|none|none|
|»» isPrivate|boolean|false|none|none|
|»» latest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» latestEpoch|number|false|none|none|
|»» metadata|[SearchJobMetadata](#schemasearchjobmetadata)|false|none|none|
|»»» arguments|object|false|none|none|
|»»»» **additionalProperties**|[object]|false|none|none|
|»»»»» **additionalProperties**|string|false|none|none|
|»»» computeTypes|object|false|none|none|
|»»»» v1|number|false|none|none|
|»»»» v2|number|false|none|none|
|»»»» lakehouse|number|false|none|none|
|»»» datasets|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» functions|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» operators|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» providerTypes|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» providers|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»» notebookId|string|false|none|none|
|»» numEventsAfter|number|false|none|none|
|»» numEventsBefore|number|false|none|none|
|»» query|string|true|none|none|
|»» queryWithMacrosResolved|string|false|none|none|
|»» sampleRate|number|false|none|none|
|»» savedQueryName|string|false|none|none|
|»» searchParameterDeclarations|[[SearchParameter](#schemasearchparameter)]|false|none|none|
|»»» defaultValue|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» name|string|true|none|none|
|»»» type|[SearchParameterType](#schemasearchparametertype)|true|none|none|
|»» searchParameterValues|[SearchParameters](#schemasearchparameters)|false|none|none|
|»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» setOptions|object|false|none|none|
|»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» stages|[[SearchJobStageConfig](#schemasearchjobstageconfig)]|false|none|none|
|»»» cacheStatusByDatasetId|[CacheStatusByDatasetId](#schemacachestatusbydatasetid)|false|none|none|
|»»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» usedCache|boolean|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» reason|string|true|none|none|
|»»»»»» usedCache|boolean|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» dependencies|[[StageDependency](#schemastagedependency)]|true|none|none|
|»»»» dependentFields|[string]|false|none|none|
|»»»» id|[StageId](#schemastageid)|true|none|none|
|»»»» type|[StageDependencyType](#schemastagedependencytype)|true|none|none|
|»»» earliest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» executionWarnings|[[JobExecutionWarning](#schemajobexecutionwarning)]|false|none|none|
|»»»» text|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» filter|string|true|none|none|
|»»» id|string|true|none|none|
|»»» latest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» resolvedDatasetIds|[string]|true|none|none|
|»»» searchConfig|[SearchConfig](#schemasearchconfig)|true|none|none|
|»»»» canComputeMetadataDistributively|boolean|false|none|none|
|»»»» datasets|[string]|true|none|none|
|»»»» hasSendOperator|boolean|true|none|none|
|»»»» logicalPlans|object|true|none|none|
|»»»»» Combined|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»»»» **additionalProperties**|[object]|false|none|none|
|»»»»»»» isPreviewableOperation|boolean|false|none|none|
|»»»»»»» type|[LogicalPlanNodeType](#schemalogicalplannodetype)|true|none|none|
|»»»»» Coordinated|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»»» Federated|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»» orderedFieldNames|[string]|true|none|none|
|»»»» pipelines|object|true|none|none|
|»»»»» Combined|[Pipeline](#schemapipeline)|false|none|none|
|»»»»»» id|string|true|none|none|
|»»»»»» conf|object|true|none|none|
|»»»»»»» asyncFuncTimeout|integer|false|none|Time (in ms) to wait for an async function to complete processing of a data item|
|»»»»»»» output|string|false|none|The output destination for events processed by this Pipeline|
|»»»»»»» description|string|false|none|none|
|»»»»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»»»»» functions|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|none|List of Functions to pass data through|
|»»»»»»»» filter|string|false|none|Filter that selects data to be fed through this Function|
|»»»»»»»» id|string|true|none|Function ID|
|»»»»»»»» description|string|false|none|Simple description of this step|
|»»»»»»»» disabled|boolean|false|none|If true, data will not be pushed through this function|
|»»»»»»»» final|boolean|false|none|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»»»»»»» conf|object|true|none|none|
|»»»»»»»» groupId|string|false|none|Group ID|
|»»»»»»» groups|object|false|none|none|
|»»»»»»»» **additionalProperties**|object|false|none|none|
|»»»»»»»»» name|string|true|none|none|
|»»»»»»»»» description|string|false|none|Short description of this group|
|»»»»»»»»» disabled|boolean|false|none|Whether this group is disabled|
|»»»»» Coordinated|[Pipeline](#schemapipeline)|false|none|none|
|»»»»» Federated|[Pipeline](#schemapipeline)|false|none|none|
|»»»» referencedColumnPaths|[[ColumnPath](#schemacolumnpath)]|false|none|none|
|»»»» searchTerms|[[SearchTerm](#schemasearchterm)]|true|none|none|
|»»»»» isCaseSensitive|boolean|true|none|none|
|»»»»» term|string|true|none|none|
|»»»» sortFields|[[SortByField](#schemasortbyfield)]|false|none|none|
|»»»»» direction|string|true|none|none|
|»»»»» fieldName|string|true|none|none|
|»»»»» nullPosition|string|true|none|none|
|»»»» useFormattedVisualization|boolean|true|none|none|
|»»» status|string|true|none|none|
|»»» subQueryText|string|true|none|none|
|»» status|string|true|none|none|
|»» tableConfig|[TableViewSettings](#schematableviewsettings)|false|none|none|
|»»» columnFilterSettings|[ColumnFilterSettings](#schemacolumnfiltersettings)|false|none|none|
|»»»» contains|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnFormatSettings|[ColumnFormatSettings](#schemacolumnformatsettings)|false|none|none|
|»»»» palette|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» precision|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» prefix|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» suffix|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnOrderSettings|[ColumnOrderSettings](#schemacolumnordersettings)|false|none|none|
|»»»» order|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnSortSettings|[ColumnSortSettings](#schemacolumnsortsettings)|false|none|none|
|»»»» sort|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» eventDetailsPanel|boolean|false|none|none|
|»»» eventTableFields|[string]|false|none|none|
|»»» rowNumberColumnWidth|number|false|none|none|
|»»» showColumnTotals|boolean|true|none|none|
|»»» showColumnTotalsPinned|boolean|true|none|none|
|»»» showRowNumbers|boolean|true|none|none|
|»»» showRowTotals|boolean|true|none|none|
|»»» showRowTotalsPinned|boolean|true|none|none|
|»»» wrapCells|boolean|true|none|none|
|»» targetEventTime|number|false|none|none|
|»» timeCompleted|number|false|none|none|
|»» timeCreated|number|true|none|none|
|»» timeStarted|number|false|none|none|
|»» timeToFirstByte|number|false|none|none|
|»» totalBytesScanned|number|false|none|none|
|»» totalEventCount|number|false|none|none|
|»» type|string|false|none|none|
|»» usageGroupId|string|false|none|none|
|»» usageMetrics|[SearchAuditMetrics](#schemasearchauditmetrics)|false|none|none|
|»»» bytesIn|number|true|none|none|
|»»» bytesOut|number|true|none|none|
|»»» eventsIn|number|true|none|none|
|»»» eventsOut|number|true|none|none|
|»»» objects|object|true|none|none|
|»»»» discovered|number|true|none|none|
|»»»» scanned|number|true|none|none|
|»»»» skipped|number|true|none|none|
|»»» tasks|object|false|none|none|
|»»»» largeFileTaskCount|number|true|none|none|
|»»»» standardTaskCount|number|true|none|none|
|»»» time|object|true|none|none|
|»»»» queuedSec|number|true|none|none|
|»»»» runningSec|number|true|none|none|
|»»»» taskCompletionTotalSec|number|true|none|none|
|»»»» taskReceivingTotalSec|number|true|none|none|
|»» user|string|true|none|none|

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
|lib|custom|
|lib|cribl-custom|
|type|regex|
|type|json|
|type|json_array|
|type|header|
|type|timestamp|
|type|csv|
|type|aws_cloudtrail|
|type|aws_vpcflow|
|type|auto|
|type|format|
|type|current|
|type|string|
|type|number|
|type|boolean|
|usedCache|true|
|usedCache|false|
|type|stage|
|type|stage-scalar|
|type|aggregate|
|type|dedup|
|type|distinct|
|type|extract|
|type|filter|
|type|limit|
|type|mv-expand|
|type|mv-pull|
|type|noop|
|type|pivot|
|type|project|
|type|sort|
|direction|ascending|
|direction|descending|
|nullPosition|nullsFirst|
|nullPosition|nullsLast|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|
|type|command|
|type|standard|
|type|scheduled|
|type|datatypePreview|
|type|dashboard|
|type|notebook|
|type|systemInsights|

<aside class="success">
This operation does not require authentication
</aside>

## Create SearchJob

<a id="opIdcreateSearchJob"></a>

> Code samples

`POST /search/jobs`

Create SearchJob

> Body parameter

```json
{
  "accelerated": true,
  "aliasOfOriginalJobId": "string",
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
  "compatibilityChecks": {
    "datatypes": true,
    "stageIds": [
      "string"
    ]
  },
  "completionInfo": "string",
  "context": "string",
  "correlationId": "string",
  "cpuMetrics": {
    "billableCPUSeconds": 0,
    "executorsCPUSeconds": {
      "property1": 0,
      "property2": 0
    },
    "totalCPUSeconds": 0,
    "totalExecCPUSeconds": 0
  },
  "datatypeOverrides": {
    "breakerRulesets": [
      {
        "id": "string",
        "lib": "custom",
        "description": "string",
        "tags": "string",
        "minRawLength": 256,
        "rules": [
          {
            "name": "string",
            "condition": "true",
            "type": "regex",
            "timestampAnchorRegex": "/^/",
            "timestamp": {
              "type": "auto",
              "length": 150,
              "format": "string"
            },
            "timestampTimezone": "local",
            "timestampEarliest": "-420weeks",
            "timestampLatest": "+1week",
            "maxEventBytes": 51200,
            "fields": [
              {
                "name": "string",
                "value": "string"
              }
            ],
            "disabled": false,
            "parserEnabled": false,
            "shouldUseDataRaw": false
          }
        ]
      }
    ],
    "disableBreakers": true
  },
  "disableNotifications": true,
  "displayUsername": "string",
  "earliest": "string",
  "earliestEpoch": 0,
  "errorStateConfig": {
    "coordinated": true,
    "errorMessages": [
      "string"
    ]
  },
  "group": "string",
  "id": "string",
  "isPrivate": true,
  "latest": "string",
  "latestEpoch": 0,
  "metadata": {
    "arguments": {
      "property1": [
        {
          "property1": "string",
          "property2": "string"
        }
      ],
      "property2": [
        {
          "property1": "string",
          "property2": "string"
        }
      ]
    },
    "computeTypes": {
      "v1": 0,
      "v2": 0,
      "lakehouse": 0
    },
    "datasets": {
      "property1": 0,
      "property2": 0
    },
    "functions": {
      "property1": 0,
      "property2": 0
    },
    "operators": {
      "property1": 0,
      "property2": 0
    },
    "providerTypes": {
      "property1": 0,
      "property2": 0
    },
    "providers": {
      "property1": 0,
      "property2": 0
    }
  },
  "notebookId": "string",
  "numEventsAfter": 0,
  "numEventsBefore": 0,
  "query": "string",
  "queryWithMacrosResolved": "string",
  "sampleRate": 0,
  "savedQueryName": "string",
  "searchParameterDeclarations": [
    {
      "defaultValue": "string",
      "name": "string",
      "type": "string"
    }
  ],
  "searchParameterValues": {
    "property1": "string",
    "property2": "string"
  },
  "setOptions": {
    "property1": "string",
    "property2": "string"
  },
  "stages": [
    {
      "cacheStatusByDatasetId": {
        "property1": {
          "usedCache": true
        },
        "property2": {
          "usedCache": true
        }
      },
      "dependencies": [
        {
          "dependentFields": [
            "string"
          ],
          "id": "string",
          "type": "stage"
        }
      ],
      "earliest": "string",
      "executionWarnings": [
        {
          "text": "string",
          "type": "string"
        }
      ],
      "filter": "string",
      "id": "string",
      "latest": "string",
      "resolvedDatasetIds": [
        "string"
      ],
      "searchConfig": {
        "canComputeMetadataDistributively": true,
        "datasets": [
          "string"
        ],
        "hasSendOperator": true,
        "logicalPlans": {
          "Combined": {
            "property1": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ],
            "property2": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ]
          },
          "Coordinated": {
            "property1": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ],
            "property2": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ]
          },
          "Federated": {
            "property1": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ],
            "property2": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ]
          }
        },
        "orderedFieldNames": [
          "string"
        ],
        "pipelines": {
          "Combined": {
            "id": "string",
            "conf": {
              "asyncFuncTimeout": 10000,
              "output": "default",
              "description": "string",
              "streamtags": [],
              "functions": [
                {
                  "filter": "true",
                  "id": "string",
                  "description": "string",
                  "disabled": true,
                  "final": true,
                  "conf": {},
                  "groupId": "string"
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
              }
            }
          },
          "Coordinated": {
            "id": "string",
            "conf": {
              "asyncFuncTimeout": 10000,
              "output": "default",
              "description": "string",
              "streamtags": [],
              "functions": [
                {
                  "filter": "true",
                  "id": "string",
                  "description": "string",
                  "disabled": true,
                  "final": true,
                  "conf": {},
                  "groupId": "string"
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
              }
            }
          },
          "Federated": {
            "id": "string",
            "conf": {
              "asyncFuncTimeout": 10000,
              "output": "default",
              "description": "string",
              "streamtags": [],
              "functions": [
                {
                  "filter": "true",
                  "id": "string",
                  "description": "string",
                  "disabled": true,
                  "final": true,
                  "conf": {},
                  "groupId": "string"
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
              }
            }
          }
        },
        "referencedColumnPaths": [
          [
            "string"
          ]
        ],
        "searchTerms": [
          {
            "isCaseSensitive": true,
            "term": "string"
          }
        ],
        "sortFields": [
          {
            "direction": "ascending",
            "fieldName": "string",
            "nullPosition": "nullsFirst"
          }
        ],
        "useFormattedVisualization": true
      },
      "status": "failed",
      "subQueryText": "string"
    }
  ],
  "status": "failed",
  "tableConfig": {
    "columnFilterSettings": {
      "contains": {}
    },
    "columnFormatSettings": {
      "palette": {},
      "precision": {},
      "prefix": {},
      "suffix": {}
    },
    "columnOrderSettings": {
      "order": {}
    },
    "columnSortSettings": {
      "sort": {}
    },
    "eventDetailsPanel": true,
    "eventTableFields": [
      "string"
    ],
    "rowNumberColumnWidth": 0,
    "showColumnTotals": true,
    "showColumnTotalsPinned": true,
    "showRowNumbers": true,
    "showRowTotals": true,
    "showRowTotalsPinned": true,
    "wrapCells": true
  },
  "targetEventTime": 0,
  "timeCompleted": 0,
  "timeCreated": 0,
  "timeStarted": 0,
  "timeToFirstByte": 0,
  "totalBytesScanned": 0,
  "totalEventCount": 0,
  "type": "command",
  "usageGroupId": "string",
  "usageMetrics": {
    "bytesIn": 0,
    "bytesOut": 0,
    "eventsIn": 0,
    "eventsOut": 0,
    "objects": {
      "discovered": 0,
      "scanned": 0,
      "skipped": 0
    },
    "tasks": {
      "largeFileTaskCount": 0,
      "standardTaskCount": 0
    },
    "time": {
      "queuedSec": 0,
      "runningSec": 0,
      "taskCompletionTotalSec": 0,
      "taskReceivingTotalSec": 0
    }
  },
  "user": "string"
}
```

<h3 id="create-searchjob-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[SearchJob](#schemasearchjob)|true|New SearchJob object|
|» accelerated|body|boolean|false|none|
|» aliasOfOriginalJobId|body|string|false|none|
|» chartConfig|body|object|false|none|
|»» applyThreshold|body|boolean|false|none|
|»» axis|body|object|false|none|
|»»» xAxis|body|string|false|none|
|»»» yAxis|body|[string]|false|none|
|»»» yAxisExcluded|body|[string]|false|none|
|»» color|body|string|false|none|
|»» colorPalette|body|number|false|none|
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
|»» type|body|string|false|none|
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
|» compatibilityChecks|body|object|false|none|
|»» datatypes|body|boolean|false|none|
|»» stageIds|body|[string]|false|none|
|» completionInfo|body|string|false|none|
|» context|body|string|false|none|
|» correlationId|body|string|false|none|
|» cpuMetrics|body|[CPUTimeMetric](#schemacputimemetric)|false|none|
|»» billableCPUSeconds|body|number|false|none|
|»» executorsCPUSeconds|body|object|true|none|
|»»» **additionalProperties**|body|number|false|none|
|»» totalCPUSeconds|body|number|true|none|
|»» totalExecCPUSeconds|body|number|true|none|
|» datatypeOverrides|body|[DatatypeOverrides](#schemadatatypeoverrides)|false|none|
|»» breakerRulesets|body|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|
|»»» id|body|string|true|none|
|»»» lib|body|string|false|none|
|»»» description|body|string|false|none|
|»»» tags|body|string|false|none|
|»»» minRawLength|body|number|false|The  minimum number of characters in _raw to determine which rule to use|
|»»» rules|body|[object]|false|A list of rules that will be applied, in order, to the input data stream|
|»»»» name|body|string|true|none|
|»»»» condition|body|string|true|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»»»» type|body|string|true|none|
|»»»» timestampAnchorRegex|body|string|true|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»»»» timestamp|body|object|true|Auto, manual format (strptime), or current time|
|»»»»» type|body|string|true|none|
|»»»»» length|body|number|false|none|
|»»»»» format|body|string|false|none|
|»»»» timestampTimezone|body|string|false|Timezone to assign to timestamps without timezone info|
|»»»» timestampEarliest|body|string|false|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»»»» timestampLatest|body|string|false|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»»»» maxEventBytes|body|number|false|The maximum number of bytes in an event before it is flushed to the pipelines|
|»»»» fields|body|[object]|false|Key-value pairs to be added to each event|
|»»»»» name|body|string|false|none|
|»»»»» value|body|string|true|The JavaScript expression used to compute the field's value (can be constant)|
|»»»» disabled|body|boolean|false|Disable this breaker rule (enabled by default)|
|»»»» parserEnabled|body|boolean|false|none|
|»»»» shouldUseDataRaw|body|boolean|false|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|
|»» disableBreakers|body|boolean|true|none|
|» disableNotifications|body|boolean|false|none|
|» displayUsername|body|string|true|none|
|» earliest|body|any|false|none|
|»» *anonymous*|body|string|false|none|
|»» *anonymous*|body|number|false|none|
|» earliestEpoch|body|number|false|none|
|» errorStateConfig|body|[SearchJobErrorStateConfig](#schemasearchjoberrorstateconfig)|false|none|
|»» coordinated|body|boolean|true|none|
|»» errorMessages|body|[string]|true|none|
|» group|body|string|true|none|
|» id|body|string|true|none|
|» isPrivate|body|boolean|false|none|
|» latest|body|any|false|none|
|»» *anonymous*|body|string|false|none|
|»» *anonymous*|body|number|false|none|
|» latestEpoch|body|number|false|none|
|» metadata|body|[SearchJobMetadata](#schemasearchjobmetadata)|false|none|
|»» arguments|body|object|false|none|
|»»» **additionalProperties**|body|[object]|false|none|
|»»»» **additionalProperties**|body|string|false|none|
|»» computeTypes|body|object|false|none|
|»»» v1|body|number|false|none|
|»»» v2|body|number|false|none|
|»»» lakehouse|body|number|false|none|
|»» datasets|body|object|false|none|
|»»» **additionalProperties**|body|number|false|none|
|»» functions|body|object|false|none|
|»»» **additionalProperties**|body|number|false|none|
|»» operators|body|object|false|none|
|»»» **additionalProperties**|body|number|false|none|
|»» providerTypes|body|object|false|none|
|»»» **additionalProperties**|body|number|false|none|
|»» providers|body|object|false|none|
|»»» **additionalProperties**|body|number|false|none|
|» notebookId|body|string|false|none|
|» numEventsAfter|body|number|false|none|
|» numEventsBefore|body|number|false|none|
|» query|body|string|true|none|
|» queryWithMacrosResolved|body|string|false|none|
|» sampleRate|body|number|false|none|
|» savedQueryName|body|string|false|none|
|» searchParameterDeclarations|body|[[SearchParameter](#schemasearchparameter)]|false|none|
|»» defaultValue|body|any|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|number|false|none|
|»»» *anonymous*|body|boolean|false|none|
|»» name|body|string|true|none|
|»» type|body|[SearchParameterType](#schemasearchparametertype)|true|none|
|» searchParameterValues|body|[SearchParameters](#schemasearchparameters)|false|none|
|»» **additionalProperties**|body|any|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|number|false|none|
|»»» *anonymous*|body|boolean|false|none|
|» setOptions|body|object|false|none|
|»» **additionalProperties**|body|any|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|number|false|none|
|» stages|body|[[SearchJobStageConfig](#schemasearchjobstageconfig)]|false|none|
|»» cacheStatusByDatasetId|body|[CacheStatusByDatasetId](#schemacachestatusbydatasetid)|false|none|
|»»» **additionalProperties**|body|any|false|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» usedCache|body|boolean|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» reason|body|string|true|none|
|»»»»» usedCache|body|boolean|true|none|
|»» dependencies|body|[[StageDependency](#schemastagedependency)]|true|none|
|»»» dependentFields|body|[string]|false|none|
|»»» id|body|[StageId](#schemastageid)|true|none|
|»»» type|body|[StageDependencyType](#schemastagedependencytype)|true|none|
|»» earliest|body|any|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|number|false|none|
|»» executionWarnings|body|[[JobExecutionWarning](#schemajobexecutionwarning)]|false|none|
|»»» text|body|string|false|none|
|»»» type|body|string|true|none|
|»» filter|body|string|true|none|
|»» id|body|string|true|none|
|»» latest|body|any|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|number|false|none|
|»» resolvedDatasetIds|body|[string]|true|none|
|»» searchConfig|body|[SearchConfig](#schemasearchconfig)|true|none|
|»»» canComputeMetadataDistributively|body|boolean|false|none|
|»»» datasets|body|[string]|true|none|
|»»» hasSendOperator|body|boolean|true|none|
|»»» logicalPlans|body|object|true|none|
|»»»» Combined|body|[LogicalPlan](#schemalogicalplan)|false|none|
|»»»»» **additionalProperties**|body|[object]|false|none|
|»»»»»» isPreviewableOperation|body|boolean|false|none|
|»»»»»» type|body|[LogicalPlanNodeType](#schemalogicalplannodetype)|true|none|
|»»»» Coordinated|body|[LogicalPlan](#schemalogicalplan)|false|none|
|»»»» Federated|body|[LogicalPlan](#schemalogicalplan)|false|none|
|»»» orderedFieldNames|body|[string]|true|none|
|»»» pipelines|body|object|true|none|
|»»»» Combined|body|[Pipeline](#schemapipeline)|false|none|
|»»»»» id|body|string|true|none|
|»»»»» conf|body|object|true|none|
|»»»»»» asyncFuncTimeout|body|integer|false|Time (in ms) to wait for an async function to complete processing of a data item|
|»»»»»» output|body|string|false|The output destination for events processed by this Pipeline|
|»»»»»» description|body|string|false|none|
|»»»»»» streamtags|body|[string]|false|Tags for filtering and grouping in @{product}|
|»»»»»» functions|body|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|List of Functions to pass data through|
|»»»»»»» filter|body|string|false|Filter that selects data to be fed through this Function|
|»»»»»»» id|body|string|true|Function ID|
|»»»»»»» description|body|string|false|Simple description of this step|
|»»»»»»» disabled|body|boolean|false|If true, data will not be pushed through this function|
|»»»»»»» final|body|boolean|false|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»»»»»» conf|body|object|true|none|
|»»»»»»» groupId|body|string|false|Group ID|
|»»»»»» groups|body|object|false|none|
|»»»»»»» **additionalProperties**|body|object|false|none|
|»»»»»»»» name|body|string|true|none|
|»»»»»»»» description|body|string|false|Short description of this group|
|»»»»»»»» disabled|body|boolean|false|Whether this group is disabled|
|»»»» Coordinated|body|[Pipeline](#schemapipeline)|false|none|
|»»»» Federated|body|[Pipeline](#schemapipeline)|false|none|
|»»» referencedColumnPaths|body|[[ColumnPath](#schemacolumnpath)]|false|none|
|»»» searchTerms|body|[[SearchTerm](#schemasearchterm)]|true|none|
|»»»» isCaseSensitive|body|boolean|true|none|
|»»»» term|body|string|true|none|
|»»» sortFields|body|[[SortByField](#schemasortbyfield)]|false|none|
|»»»» direction|body|string|true|none|
|»»»» fieldName|body|string|true|none|
|»»»» nullPosition|body|string|true|none|
|»»» useFormattedVisualization|body|boolean|true|none|
|»» status|body|string|true|none|
|»» subQueryText|body|string|true|none|
|» status|body|string|true|none|
|» tableConfig|body|[TableViewSettings](#schematableviewsettings)|false|none|
|»» columnFilterSettings|body|[ColumnFilterSettings](#schemacolumnfiltersettings)|false|none|
|»»» contains|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»» columnFormatSettings|body|[ColumnFormatSettings](#schemacolumnformatsettings)|false|none|
|»»» palette|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»»» precision|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»»» prefix|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»»» suffix|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»» columnOrderSettings|body|[ColumnOrderSettings](#schemacolumnordersettings)|false|none|
|»»» order|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»» columnSortSettings|body|[ColumnSortSettings](#schemacolumnsortsettings)|false|none|
|»»» sort|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»» eventDetailsPanel|body|boolean|false|none|
|»» eventTableFields|body|[string]|false|none|
|»» rowNumberColumnWidth|body|number|false|none|
|»» showColumnTotals|body|boolean|true|none|
|»» showColumnTotalsPinned|body|boolean|true|none|
|»» showRowNumbers|body|boolean|true|none|
|»» showRowTotals|body|boolean|true|none|
|»» showRowTotalsPinned|body|boolean|true|none|
|»» wrapCells|body|boolean|true|none|
|» targetEventTime|body|number|false|none|
|» timeCompleted|body|number|false|none|
|» timeCreated|body|number|true|none|
|» timeStarted|body|number|false|none|
|» timeToFirstByte|body|number|false|none|
|» totalBytesScanned|body|number|false|none|
|» totalEventCount|body|number|false|none|
|» type|body|string|false|none|
|» usageGroupId|body|string|false|none|
|» usageMetrics|body|[SearchAuditMetrics](#schemasearchauditmetrics)|false|none|
|»» bytesIn|body|number|true|none|
|»» bytesOut|body|number|true|none|
|»» eventsIn|body|number|true|none|
|»» eventsOut|body|number|true|none|
|»» objects|body|object|true|none|
|»»» discovered|body|number|true|none|
|»»» scanned|body|number|true|none|
|»»» skipped|body|number|true|none|
|»» tasks|body|object|false|none|
|»»» largeFileTaskCount|body|number|true|none|
|»»» standardTaskCount|body|number|true|none|
|»» time|body|object|true|none|
|»»» queuedSec|body|number|true|none|
|»»» runningSec|body|number|true|none|
|»»» taskCompletionTotalSec|body|number|true|none|
|»»» taskReceivingTotalSec|body|number|true|none|
|» user|body|string|true|none|

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
|»»» lib|custom|
|»»» lib|cribl-custom|
|»»»» type|regex|
|»»»» type|json|
|»»»» type|json_array|
|»»»» type|header|
|»»»» type|timestamp|
|»»»» type|csv|
|»»»» type|aws_cloudtrail|
|»»»» type|aws_vpcflow|
|»»»»» type|auto|
|»»»»» type|format|
|»»»»» type|current|
|»» type|string|
|»» type|number|
|»» type|boolean|
|»»»»» usedCache|true|
|»»»»» usedCache|false|
|»»» type|stage|
|»»» type|stage-scalar|
|»»»»»» type|aggregate|
|»»»»»» type|dedup|
|»»»»»» type|distinct|
|»»»»»» type|extract|
|»»»»»» type|filter|
|»»»»»» type|limit|
|»»»»»» type|mv-expand|
|»»»»»» type|mv-pull|
|»»»»»» type|noop|
|»»»»»» type|pivot|
|»»»»»» type|project|
|»»»»»» type|sort|
|»»»» direction|ascending|
|»»»» direction|descending|
|»»»» nullPosition|nullsFirst|
|»»»» nullPosition|nullsLast|
|»» status|failed|
|»» status|new|
|»» status|running|
|»» status|completed|
|»» status|canceled|
|»» status|queued|
|»» status|finalizing|
|» status|failed|
|» status|new|
|» status|running|
|» status|completed|
|» status|canceled|
|» status|queued|
|» status|finalizing|
|» type|command|
|» type|standard|
|» type|scheduled|
|» type|datatypePreview|
|» type|dashboard|
|» type|notebook|
|» type|systemInsights|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "accelerated": true,
      "aliasOfOriginalJobId": "string",
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
      "compatibilityChecks": {
        "datatypes": true,
        "stageIds": [
          "string"
        ]
      },
      "completionInfo": "string",
      "context": "string",
      "correlationId": "string",
      "cpuMetrics": {
        "billableCPUSeconds": 0,
        "executorsCPUSeconds": {
          "property1": 0,
          "property2": 0
        },
        "totalCPUSeconds": 0,
        "totalExecCPUSeconds": 0
      },
      "datatypeOverrides": {
        "breakerRulesets": [
          {
            "id": "string",
            "lib": "custom",
            "description": "string",
            "tags": "string",
            "minRawLength": 256,
            "rules": [
              {
                "name": "string",
                "condition": "true",
                "type": "regex",
                "timestampAnchorRegex": "/^/",
                "timestamp": {
                  "type": "auto",
                  "length": 150,
                  "format": "string"
                },
                "timestampTimezone": "local",
                "timestampEarliest": "-420weeks",
                "timestampLatest": "+1week",
                "maxEventBytes": 51200,
                "fields": [
                  {
                    "name": "string",
                    "value": "string"
                  }
                ],
                "disabled": false,
                "parserEnabled": false,
                "shouldUseDataRaw": false
              }
            ]
          }
        ],
        "disableBreakers": true
      },
      "disableNotifications": true,
      "displayUsername": "string",
      "earliest": "string",
      "earliestEpoch": 0,
      "errorStateConfig": {
        "coordinated": true,
        "errorMessages": [
          "string"
        ]
      },
      "group": "string",
      "id": "string",
      "isPrivate": true,
      "latest": "string",
      "latestEpoch": 0,
      "metadata": {
        "arguments": {
          "property1": [
            {
              "property1": "string",
              "property2": "string"
            }
          ],
          "property2": [
            {
              "property1": "string",
              "property2": "string"
            }
          ]
        },
        "computeTypes": {
          "v1": 0,
          "v2": 0,
          "lakehouse": 0
        },
        "datasets": {
          "property1": 0,
          "property2": 0
        },
        "functions": {
          "property1": 0,
          "property2": 0
        },
        "operators": {
          "property1": 0,
          "property2": 0
        },
        "providerTypes": {
          "property1": 0,
          "property2": 0
        },
        "providers": {
          "property1": 0,
          "property2": 0
        }
      },
      "notebookId": "string",
      "numEventsAfter": 0,
      "numEventsBefore": 0,
      "query": "string",
      "queryWithMacrosResolved": "string",
      "sampleRate": 0,
      "savedQueryName": "string",
      "searchParameterDeclarations": [
        {
          "defaultValue": "string",
          "name": "string",
          "type": "string"
        }
      ],
      "searchParameterValues": {
        "property1": "string",
        "property2": "string"
      },
      "setOptions": {
        "property1": "string",
        "property2": "string"
      },
      "stages": [
        {
          "cacheStatusByDatasetId": {
            "property1": {
              "usedCache": true
            },
            "property2": {
              "usedCache": true
            }
          },
          "dependencies": [
            {
              "dependentFields": [
                "string"
              ],
              "id": "string",
              "type": "stage"
            }
          ],
          "earliest": "string",
          "executionWarnings": [
            {
              "text": "string",
              "type": "string"
            }
          ],
          "filter": "string",
          "id": "string",
          "latest": "string",
          "resolvedDatasetIds": [
            "string"
          ],
          "searchConfig": {
            "canComputeMetadataDistributively": true,
            "datasets": [
              "string"
            ],
            "hasSendOperator": true,
            "logicalPlans": {
              "Combined": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              },
              "Coordinated": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              },
              "Federated": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              }
            },
            "orderedFieldNames": [
              "string"
            ],
            "pipelines": {
              "Combined": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              },
              "Coordinated": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              },
              "Federated": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              }
            },
            "referencedColumnPaths": [
              [
                "string"
              ]
            ],
            "searchTerms": [
              {
                "isCaseSensitive": true,
                "term": "string"
              }
            ],
            "sortFields": [
              {
                "direction": "ascending",
                "fieldName": "string",
                "nullPosition": "nullsFirst"
              }
            ],
            "useFormattedVisualization": true
          },
          "status": "failed",
          "subQueryText": "string"
        }
      ],
      "status": "failed",
      "tableConfig": {
        "columnFilterSettings": {
          "contains": {}
        },
        "columnFormatSettings": {
          "palette": {},
          "precision": {},
          "prefix": {},
          "suffix": {}
        },
        "columnOrderSettings": {
          "order": {}
        },
        "columnSortSettings": {
          "sort": {}
        },
        "eventDetailsPanel": true,
        "eventTableFields": [
          "string"
        ],
        "rowNumberColumnWidth": 0,
        "showColumnTotals": true,
        "showColumnTotalsPinned": true,
        "showRowNumbers": true,
        "showRowTotals": true,
        "showRowTotalsPinned": true,
        "wrapCells": true
      },
      "targetEventTime": 0,
      "timeCompleted": 0,
      "timeCreated": 0,
      "timeStarted": 0,
      "timeToFirstByte": 0,
      "totalBytesScanned": 0,
      "totalEventCount": 0,
      "type": "command",
      "usageGroupId": "string",
      "usageMetrics": {
        "bytesIn": 0,
        "bytesOut": 0,
        "eventsIn": 0,
        "eventsOut": 0,
        "objects": {
          "discovered": 0,
          "scanned": 0,
          "skipped": 0
        },
        "tasks": {
          "largeFileTaskCount": 0,
          "standardTaskCount": 0
        },
        "time": {
          "queuedSec": 0,
          "runningSec": 0,
          "taskCompletionTotalSec": 0,
          "taskReceivingTotalSec": 0
        }
      },
      "user": "string"
    }
  ]
}
```

<h3 id="create-searchjob-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchJob objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-searchjob-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchJob](#schemasearchjob)]|false|none|none|
|»» accelerated|boolean|false|none|none|
|»» aliasOfOriginalJobId|string|false|none|none|
|»» chartConfig|object|false|none|none|
|»»» applyThreshold|boolean|false|none|none|
|»»» axis|object|false|none|none|
|»»»» xAxis|string|false|none|none|
|»»»» yAxis|[string]|false|none|none|
|»»»» yAxisExcluded|[string]|false|none|none|
|»»» color|string|false|none|none|
|»»» colorPalette|number|false|none|none|
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
|»»» type|string|false|none|none|
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
|»» compatibilityChecks|object|false|none|none|
|»»» datatypes|boolean|false|none|none|
|»»» stageIds|[string]|false|none|none|
|»» completionInfo|string|false|none|none|
|»» context|string|false|none|none|
|»» correlationId|string|false|none|none|
|»» cpuMetrics|[CPUTimeMetric](#schemacputimemetric)|false|none|none|
|»»» billableCPUSeconds|number|false|none|none|
|»»» executorsCPUSeconds|object|true|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» totalCPUSeconds|number|true|none|none|
|»»» totalExecCPUSeconds|number|true|none|none|
|»» datatypeOverrides|[DatatypeOverrides](#schemadatatypeoverrides)|false|none|none|
|»»» breakerRulesets|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|none|
|»»»» id|string|true|none|none|
|»»»» lib|string|false|none|none|
|»»»» description|string|false|none|none|
|»»»» tags|string|false|none|none|
|»»»» minRawLength|number|false|none|The  minimum number of characters in _raw to determine which rule to use|
|»»»» rules|[object]|false|none|A list of rules that will be applied, in order, to the input data stream|
|»»»»» name|string|true|none|none|
|»»»»» condition|string|true|none|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»»»»» type|string|true|none|none|
|»»»»» timestampAnchorRegex|string|true|none|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»»»»» timestamp|object|true|none|Auto, manual format (strptime), or current time|
|»»»»»» type|string|true|none|none|
|»»»»»» length|number|false|none|none|
|»»»»»» format|string|false|none|none|
|»»»»» timestampTimezone|string|false|none|Timezone to assign to timestamps without timezone info|
|»»»»» timestampEarliest|string|false|none|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»»»»» timestampLatest|string|false|none|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»»»»» maxEventBytes|number|false|none|The maximum number of bytes in an event before it is flushed to the pipelines|
|»»»»» fields|[object]|false|none|Key-value pairs to be added to each event|
|»»»»»» name|string|false|none|none|
|»»»»»» value|string|true|none|The JavaScript expression used to compute the field's value (can be constant)|
|»»»»» disabled|boolean|false|none|Disable this breaker rule (enabled by default)|
|»»»»» parserEnabled|boolean|false|none|none|
|»»»»» shouldUseDataRaw|boolean|false|none|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|
|»»» disableBreakers|boolean|true|none|none|
|»» disableNotifications|boolean|false|none|none|
|»» displayUsername|string|true|none|none|
|»» earliest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» earliestEpoch|number|false|none|none|
|»» errorStateConfig|[SearchJobErrorStateConfig](#schemasearchjoberrorstateconfig)|false|none|none|
|»»» coordinated|boolean|true|none|none|
|»»» errorMessages|[string]|true|none|none|
|»» group|string|true|none|none|
|»» id|string|true|none|none|
|»» isPrivate|boolean|false|none|none|
|»» latest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» latestEpoch|number|false|none|none|
|»» metadata|[SearchJobMetadata](#schemasearchjobmetadata)|false|none|none|
|»»» arguments|object|false|none|none|
|»»»» **additionalProperties**|[object]|false|none|none|
|»»»»» **additionalProperties**|string|false|none|none|
|»»» computeTypes|object|false|none|none|
|»»»» v1|number|false|none|none|
|»»»» v2|number|false|none|none|
|»»»» lakehouse|number|false|none|none|
|»»» datasets|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» functions|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» operators|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» providerTypes|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» providers|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»» notebookId|string|false|none|none|
|»» numEventsAfter|number|false|none|none|
|»» numEventsBefore|number|false|none|none|
|»» query|string|true|none|none|
|»» queryWithMacrosResolved|string|false|none|none|
|»» sampleRate|number|false|none|none|
|»» savedQueryName|string|false|none|none|
|»» searchParameterDeclarations|[[SearchParameter](#schemasearchparameter)]|false|none|none|
|»»» defaultValue|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» name|string|true|none|none|
|»»» type|[SearchParameterType](#schemasearchparametertype)|true|none|none|
|»» searchParameterValues|[SearchParameters](#schemasearchparameters)|false|none|none|
|»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» setOptions|object|false|none|none|
|»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» stages|[[SearchJobStageConfig](#schemasearchjobstageconfig)]|false|none|none|
|»»» cacheStatusByDatasetId|[CacheStatusByDatasetId](#schemacachestatusbydatasetid)|false|none|none|
|»»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» usedCache|boolean|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» reason|string|true|none|none|
|»»»»»» usedCache|boolean|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» dependencies|[[StageDependency](#schemastagedependency)]|true|none|none|
|»»»» dependentFields|[string]|false|none|none|
|»»»» id|[StageId](#schemastageid)|true|none|none|
|»»»» type|[StageDependencyType](#schemastagedependencytype)|true|none|none|
|»»» earliest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» executionWarnings|[[JobExecutionWarning](#schemajobexecutionwarning)]|false|none|none|
|»»»» text|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» filter|string|true|none|none|
|»»» id|string|true|none|none|
|»»» latest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» resolvedDatasetIds|[string]|true|none|none|
|»»» searchConfig|[SearchConfig](#schemasearchconfig)|true|none|none|
|»»»» canComputeMetadataDistributively|boolean|false|none|none|
|»»»» datasets|[string]|true|none|none|
|»»»» hasSendOperator|boolean|true|none|none|
|»»»» logicalPlans|object|true|none|none|
|»»»»» Combined|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»»»» **additionalProperties**|[object]|false|none|none|
|»»»»»»» isPreviewableOperation|boolean|false|none|none|
|»»»»»»» type|[LogicalPlanNodeType](#schemalogicalplannodetype)|true|none|none|
|»»»»» Coordinated|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»»» Federated|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»» orderedFieldNames|[string]|true|none|none|
|»»»» pipelines|object|true|none|none|
|»»»»» Combined|[Pipeline](#schemapipeline)|false|none|none|
|»»»»»» id|string|true|none|none|
|»»»»»» conf|object|true|none|none|
|»»»»»»» asyncFuncTimeout|integer|false|none|Time (in ms) to wait for an async function to complete processing of a data item|
|»»»»»»» output|string|false|none|The output destination for events processed by this Pipeline|
|»»»»»»» description|string|false|none|none|
|»»»»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»»»»» functions|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|none|List of Functions to pass data through|
|»»»»»»»» filter|string|false|none|Filter that selects data to be fed through this Function|
|»»»»»»»» id|string|true|none|Function ID|
|»»»»»»»» description|string|false|none|Simple description of this step|
|»»»»»»»» disabled|boolean|false|none|If true, data will not be pushed through this function|
|»»»»»»»» final|boolean|false|none|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»»»»»»» conf|object|true|none|none|
|»»»»»»»» groupId|string|false|none|Group ID|
|»»»»»»» groups|object|false|none|none|
|»»»»»»»» **additionalProperties**|object|false|none|none|
|»»»»»»»»» name|string|true|none|none|
|»»»»»»»»» description|string|false|none|Short description of this group|
|»»»»»»»»» disabled|boolean|false|none|Whether this group is disabled|
|»»»»» Coordinated|[Pipeline](#schemapipeline)|false|none|none|
|»»»»» Federated|[Pipeline](#schemapipeline)|false|none|none|
|»»»» referencedColumnPaths|[[ColumnPath](#schemacolumnpath)]|false|none|none|
|»»»» searchTerms|[[SearchTerm](#schemasearchterm)]|true|none|none|
|»»»»» isCaseSensitive|boolean|true|none|none|
|»»»»» term|string|true|none|none|
|»»»» sortFields|[[SortByField](#schemasortbyfield)]|false|none|none|
|»»»»» direction|string|true|none|none|
|»»»»» fieldName|string|true|none|none|
|»»»»» nullPosition|string|true|none|none|
|»»»» useFormattedVisualization|boolean|true|none|none|
|»»» status|string|true|none|none|
|»»» subQueryText|string|true|none|none|
|»» status|string|true|none|none|
|»» tableConfig|[TableViewSettings](#schematableviewsettings)|false|none|none|
|»»» columnFilterSettings|[ColumnFilterSettings](#schemacolumnfiltersettings)|false|none|none|
|»»»» contains|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnFormatSettings|[ColumnFormatSettings](#schemacolumnformatsettings)|false|none|none|
|»»»» palette|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» precision|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» prefix|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» suffix|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnOrderSettings|[ColumnOrderSettings](#schemacolumnordersettings)|false|none|none|
|»»»» order|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnSortSettings|[ColumnSortSettings](#schemacolumnsortsettings)|false|none|none|
|»»»» sort|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» eventDetailsPanel|boolean|false|none|none|
|»»» eventTableFields|[string]|false|none|none|
|»»» rowNumberColumnWidth|number|false|none|none|
|»»» showColumnTotals|boolean|true|none|none|
|»»» showColumnTotalsPinned|boolean|true|none|none|
|»»» showRowNumbers|boolean|true|none|none|
|»»» showRowTotals|boolean|true|none|none|
|»»» showRowTotalsPinned|boolean|true|none|none|
|»»» wrapCells|boolean|true|none|none|
|»» targetEventTime|number|false|none|none|
|»» timeCompleted|number|false|none|none|
|»» timeCreated|number|true|none|none|
|»» timeStarted|number|false|none|none|
|»» timeToFirstByte|number|false|none|none|
|»» totalBytesScanned|number|false|none|none|
|»» totalEventCount|number|false|none|none|
|»» type|string|false|none|none|
|»» usageGroupId|string|false|none|none|
|»» usageMetrics|[SearchAuditMetrics](#schemasearchauditmetrics)|false|none|none|
|»»» bytesIn|number|true|none|none|
|»»» bytesOut|number|true|none|none|
|»»» eventsIn|number|true|none|none|
|»»» eventsOut|number|true|none|none|
|»»» objects|object|true|none|none|
|»»»» discovered|number|true|none|none|
|»»»» scanned|number|true|none|none|
|»»»» skipped|number|true|none|none|
|»»» tasks|object|false|none|none|
|»»»» largeFileTaskCount|number|true|none|none|
|»»»» standardTaskCount|number|true|none|none|
|»»» time|object|true|none|none|
|»»»» queuedSec|number|true|none|none|
|»»»» runningSec|number|true|none|none|
|»»»» taskCompletionTotalSec|number|true|none|none|
|»»»» taskReceivingTotalSec|number|true|none|none|
|»» user|string|true|none|none|

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
|lib|custom|
|lib|cribl-custom|
|type|regex|
|type|json|
|type|json_array|
|type|header|
|type|timestamp|
|type|csv|
|type|aws_cloudtrail|
|type|aws_vpcflow|
|type|auto|
|type|format|
|type|current|
|type|string|
|type|number|
|type|boolean|
|usedCache|true|
|usedCache|false|
|type|stage|
|type|stage-scalar|
|type|aggregate|
|type|dedup|
|type|distinct|
|type|extract|
|type|filter|
|type|limit|
|type|mv-expand|
|type|mv-pull|
|type|noop|
|type|pivot|
|type|project|
|type|sort|
|direction|ascending|
|direction|descending|
|nullPosition|nullsFirst|
|nullPosition|nullsLast|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|
|type|command|
|type|standard|
|type|scheduled|
|type|datatypePreview|
|type|dashboard|
|type|notebook|
|type|systemInsights|

<aside class="success">
This operation does not require authentication
</aside>

## Delete SearchJob

<a id="opIddeleteSearchJobById"></a>

> Code samples

`DELETE /search/jobs/{id}`

Delete SearchJob

<h3 id="delete-searchjob-parameters">Parameters</h3>

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
      "accelerated": true,
      "aliasOfOriginalJobId": "string",
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
      "compatibilityChecks": {
        "datatypes": true,
        "stageIds": [
          "string"
        ]
      },
      "completionInfo": "string",
      "context": "string",
      "correlationId": "string",
      "cpuMetrics": {
        "billableCPUSeconds": 0,
        "executorsCPUSeconds": {
          "property1": 0,
          "property2": 0
        },
        "totalCPUSeconds": 0,
        "totalExecCPUSeconds": 0
      },
      "datatypeOverrides": {
        "breakerRulesets": [
          {
            "id": "string",
            "lib": "custom",
            "description": "string",
            "tags": "string",
            "minRawLength": 256,
            "rules": [
              {
                "name": "string",
                "condition": "true",
                "type": "regex",
                "timestampAnchorRegex": "/^/",
                "timestamp": {
                  "type": "auto",
                  "length": 150,
                  "format": "string"
                },
                "timestampTimezone": "local",
                "timestampEarliest": "-420weeks",
                "timestampLatest": "+1week",
                "maxEventBytes": 51200,
                "fields": [
                  {
                    "name": "string",
                    "value": "string"
                  }
                ],
                "disabled": false,
                "parserEnabled": false,
                "shouldUseDataRaw": false
              }
            ]
          }
        ],
        "disableBreakers": true
      },
      "disableNotifications": true,
      "displayUsername": "string",
      "earliest": "string",
      "earliestEpoch": 0,
      "errorStateConfig": {
        "coordinated": true,
        "errorMessages": [
          "string"
        ]
      },
      "group": "string",
      "id": "string",
      "isPrivate": true,
      "latest": "string",
      "latestEpoch": 0,
      "metadata": {
        "arguments": {
          "property1": [
            {
              "property1": "string",
              "property2": "string"
            }
          ],
          "property2": [
            {
              "property1": "string",
              "property2": "string"
            }
          ]
        },
        "computeTypes": {
          "v1": 0,
          "v2": 0,
          "lakehouse": 0
        },
        "datasets": {
          "property1": 0,
          "property2": 0
        },
        "functions": {
          "property1": 0,
          "property2": 0
        },
        "operators": {
          "property1": 0,
          "property2": 0
        },
        "providerTypes": {
          "property1": 0,
          "property2": 0
        },
        "providers": {
          "property1": 0,
          "property2": 0
        }
      },
      "notebookId": "string",
      "numEventsAfter": 0,
      "numEventsBefore": 0,
      "query": "string",
      "queryWithMacrosResolved": "string",
      "sampleRate": 0,
      "savedQueryName": "string",
      "searchParameterDeclarations": [
        {
          "defaultValue": "string",
          "name": "string",
          "type": "string"
        }
      ],
      "searchParameterValues": {
        "property1": "string",
        "property2": "string"
      },
      "setOptions": {
        "property1": "string",
        "property2": "string"
      },
      "stages": [
        {
          "cacheStatusByDatasetId": {
            "property1": {
              "usedCache": true
            },
            "property2": {
              "usedCache": true
            }
          },
          "dependencies": [
            {
              "dependentFields": [
                "string"
              ],
              "id": "string",
              "type": "stage"
            }
          ],
          "earliest": "string",
          "executionWarnings": [
            {
              "text": "string",
              "type": "string"
            }
          ],
          "filter": "string",
          "id": "string",
          "latest": "string",
          "resolvedDatasetIds": [
            "string"
          ],
          "searchConfig": {
            "canComputeMetadataDistributively": true,
            "datasets": [
              "string"
            ],
            "hasSendOperator": true,
            "logicalPlans": {
              "Combined": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              },
              "Coordinated": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              },
              "Federated": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              }
            },
            "orderedFieldNames": [
              "string"
            ],
            "pipelines": {
              "Combined": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              },
              "Coordinated": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              },
              "Federated": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              }
            },
            "referencedColumnPaths": [
              [
                "string"
              ]
            ],
            "searchTerms": [
              {
                "isCaseSensitive": true,
                "term": "string"
              }
            ],
            "sortFields": [
              {
                "direction": "ascending",
                "fieldName": "string",
                "nullPosition": "nullsFirst"
              }
            ],
            "useFormattedVisualization": true
          },
          "status": "failed",
          "subQueryText": "string"
        }
      ],
      "status": "failed",
      "tableConfig": {
        "columnFilterSettings": {
          "contains": {}
        },
        "columnFormatSettings": {
          "palette": {},
          "precision": {},
          "prefix": {},
          "suffix": {}
        },
        "columnOrderSettings": {
          "order": {}
        },
        "columnSortSettings": {
          "sort": {}
        },
        "eventDetailsPanel": true,
        "eventTableFields": [
          "string"
        ],
        "rowNumberColumnWidth": 0,
        "showColumnTotals": true,
        "showColumnTotalsPinned": true,
        "showRowNumbers": true,
        "showRowTotals": true,
        "showRowTotalsPinned": true,
        "wrapCells": true
      },
      "targetEventTime": 0,
      "timeCompleted": 0,
      "timeCreated": 0,
      "timeStarted": 0,
      "timeToFirstByte": 0,
      "totalBytesScanned": 0,
      "totalEventCount": 0,
      "type": "command",
      "usageGroupId": "string",
      "usageMetrics": {
        "bytesIn": 0,
        "bytesOut": 0,
        "eventsIn": 0,
        "eventsOut": 0,
        "objects": {
          "discovered": 0,
          "scanned": 0,
          "skipped": 0
        },
        "tasks": {
          "largeFileTaskCount": 0,
          "standardTaskCount": 0
        },
        "time": {
          "queuedSec": 0,
          "runningSec": 0,
          "taskCompletionTotalSec": 0,
          "taskReceivingTotalSec": 0
        }
      },
      "user": "string"
    }
  ]
}
```

<h3 id="delete-searchjob-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchJob objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-searchjob-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchJob](#schemasearchjob)]|false|none|none|
|»» accelerated|boolean|false|none|none|
|»» aliasOfOriginalJobId|string|false|none|none|
|»» chartConfig|object|false|none|none|
|»»» applyThreshold|boolean|false|none|none|
|»»» axis|object|false|none|none|
|»»»» xAxis|string|false|none|none|
|»»»» yAxis|[string]|false|none|none|
|»»»» yAxisExcluded|[string]|false|none|none|
|»»» color|string|false|none|none|
|»»» colorPalette|number|false|none|none|
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
|»»» type|string|false|none|none|
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
|»» compatibilityChecks|object|false|none|none|
|»»» datatypes|boolean|false|none|none|
|»»» stageIds|[string]|false|none|none|
|»» completionInfo|string|false|none|none|
|»» context|string|false|none|none|
|»» correlationId|string|false|none|none|
|»» cpuMetrics|[CPUTimeMetric](#schemacputimemetric)|false|none|none|
|»»» billableCPUSeconds|number|false|none|none|
|»»» executorsCPUSeconds|object|true|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» totalCPUSeconds|number|true|none|none|
|»»» totalExecCPUSeconds|number|true|none|none|
|»» datatypeOverrides|[DatatypeOverrides](#schemadatatypeoverrides)|false|none|none|
|»»» breakerRulesets|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|none|
|»»»» id|string|true|none|none|
|»»»» lib|string|false|none|none|
|»»»» description|string|false|none|none|
|»»»» tags|string|false|none|none|
|»»»» minRawLength|number|false|none|The  minimum number of characters in _raw to determine which rule to use|
|»»»» rules|[object]|false|none|A list of rules that will be applied, in order, to the input data stream|
|»»»»» name|string|true|none|none|
|»»»»» condition|string|true|none|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»»»»» type|string|true|none|none|
|»»»»» timestampAnchorRegex|string|true|none|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»»»»» timestamp|object|true|none|Auto, manual format (strptime), or current time|
|»»»»»» type|string|true|none|none|
|»»»»»» length|number|false|none|none|
|»»»»»» format|string|false|none|none|
|»»»»» timestampTimezone|string|false|none|Timezone to assign to timestamps without timezone info|
|»»»»» timestampEarliest|string|false|none|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»»»»» timestampLatest|string|false|none|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»»»»» maxEventBytes|number|false|none|The maximum number of bytes in an event before it is flushed to the pipelines|
|»»»»» fields|[object]|false|none|Key-value pairs to be added to each event|
|»»»»»» name|string|false|none|none|
|»»»»»» value|string|true|none|The JavaScript expression used to compute the field's value (can be constant)|
|»»»»» disabled|boolean|false|none|Disable this breaker rule (enabled by default)|
|»»»»» parserEnabled|boolean|false|none|none|
|»»»»» shouldUseDataRaw|boolean|false|none|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|
|»»» disableBreakers|boolean|true|none|none|
|»» disableNotifications|boolean|false|none|none|
|»» displayUsername|string|true|none|none|
|»» earliest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» earliestEpoch|number|false|none|none|
|»» errorStateConfig|[SearchJobErrorStateConfig](#schemasearchjoberrorstateconfig)|false|none|none|
|»»» coordinated|boolean|true|none|none|
|»»» errorMessages|[string]|true|none|none|
|»» group|string|true|none|none|
|»» id|string|true|none|none|
|»» isPrivate|boolean|false|none|none|
|»» latest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» latestEpoch|number|false|none|none|
|»» metadata|[SearchJobMetadata](#schemasearchjobmetadata)|false|none|none|
|»»» arguments|object|false|none|none|
|»»»» **additionalProperties**|[object]|false|none|none|
|»»»»» **additionalProperties**|string|false|none|none|
|»»» computeTypes|object|false|none|none|
|»»»» v1|number|false|none|none|
|»»»» v2|number|false|none|none|
|»»»» lakehouse|number|false|none|none|
|»»» datasets|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» functions|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» operators|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» providerTypes|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» providers|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»» notebookId|string|false|none|none|
|»» numEventsAfter|number|false|none|none|
|»» numEventsBefore|number|false|none|none|
|»» query|string|true|none|none|
|»» queryWithMacrosResolved|string|false|none|none|
|»» sampleRate|number|false|none|none|
|»» savedQueryName|string|false|none|none|
|»» searchParameterDeclarations|[[SearchParameter](#schemasearchparameter)]|false|none|none|
|»»» defaultValue|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» name|string|true|none|none|
|»»» type|[SearchParameterType](#schemasearchparametertype)|true|none|none|
|»» searchParameterValues|[SearchParameters](#schemasearchparameters)|false|none|none|
|»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» setOptions|object|false|none|none|
|»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» stages|[[SearchJobStageConfig](#schemasearchjobstageconfig)]|false|none|none|
|»»» cacheStatusByDatasetId|[CacheStatusByDatasetId](#schemacachestatusbydatasetid)|false|none|none|
|»»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» usedCache|boolean|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» reason|string|true|none|none|
|»»»»»» usedCache|boolean|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» dependencies|[[StageDependency](#schemastagedependency)]|true|none|none|
|»»»» dependentFields|[string]|false|none|none|
|»»»» id|[StageId](#schemastageid)|true|none|none|
|»»»» type|[StageDependencyType](#schemastagedependencytype)|true|none|none|
|»»» earliest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» executionWarnings|[[JobExecutionWarning](#schemajobexecutionwarning)]|false|none|none|
|»»»» text|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» filter|string|true|none|none|
|»»» id|string|true|none|none|
|»»» latest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» resolvedDatasetIds|[string]|true|none|none|
|»»» searchConfig|[SearchConfig](#schemasearchconfig)|true|none|none|
|»»»» canComputeMetadataDistributively|boolean|false|none|none|
|»»»» datasets|[string]|true|none|none|
|»»»» hasSendOperator|boolean|true|none|none|
|»»»» logicalPlans|object|true|none|none|
|»»»»» Combined|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»»»» **additionalProperties**|[object]|false|none|none|
|»»»»»»» isPreviewableOperation|boolean|false|none|none|
|»»»»»»» type|[LogicalPlanNodeType](#schemalogicalplannodetype)|true|none|none|
|»»»»» Coordinated|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»»» Federated|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»» orderedFieldNames|[string]|true|none|none|
|»»»» pipelines|object|true|none|none|
|»»»»» Combined|[Pipeline](#schemapipeline)|false|none|none|
|»»»»»» id|string|true|none|none|
|»»»»»» conf|object|true|none|none|
|»»»»»»» asyncFuncTimeout|integer|false|none|Time (in ms) to wait for an async function to complete processing of a data item|
|»»»»»»» output|string|false|none|The output destination for events processed by this Pipeline|
|»»»»»»» description|string|false|none|none|
|»»»»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»»»»» functions|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|none|List of Functions to pass data through|
|»»»»»»»» filter|string|false|none|Filter that selects data to be fed through this Function|
|»»»»»»»» id|string|true|none|Function ID|
|»»»»»»»» description|string|false|none|Simple description of this step|
|»»»»»»»» disabled|boolean|false|none|If true, data will not be pushed through this function|
|»»»»»»»» final|boolean|false|none|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»»»»»»» conf|object|true|none|none|
|»»»»»»»» groupId|string|false|none|Group ID|
|»»»»»»» groups|object|false|none|none|
|»»»»»»»» **additionalProperties**|object|false|none|none|
|»»»»»»»»» name|string|true|none|none|
|»»»»»»»»» description|string|false|none|Short description of this group|
|»»»»»»»»» disabled|boolean|false|none|Whether this group is disabled|
|»»»»» Coordinated|[Pipeline](#schemapipeline)|false|none|none|
|»»»»» Federated|[Pipeline](#schemapipeline)|false|none|none|
|»»»» referencedColumnPaths|[[ColumnPath](#schemacolumnpath)]|false|none|none|
|»»»» searchTerms|[[SearchTerm](#schemasearchterm)]|true|none|none|
|»»»»» isCaseSensitive|boolean|true|none|none|
|»»»»» term|string|true|none|none|
|»»»» sortFields|[[SortByField](#schemasortbyfield)]|false|none|none|
|»»»»» direction|string|true|none|none|
|»»»»» fieldName|string|true|none|none|
|»»»»» nullPosition|string|true|none|none|
|»»»» useFormattedVisualization|boolean|true|none|none|
|»»» status|string|true|none|none|
|»»» subQueryText|string|true|none|none|
|»» status|string|true|none|none|
|»» tableConfig|[TableViewSettings](#schematableviewsettings)|false|none|none|
|»»» columnFilterSettings|[ColumnFilterSettings](#schemacolumnfiltersettings)|false|none|none|
|»»»» contains|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnFormatSettings|[ColumnFormatSettings](#schemacolumnformatsettings)|false|none|none|
|»»»» palette|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» precision|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» prefix|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» suffix|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnOrderSettings|[ColumnOrderSettings](#schemacolumnordersettings)|false|none|none|
|»»»» order|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnSortSettings|[ColumnSortSettings](#schemacolumnsortsettings)|false|none|none|
|»»»» sort|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» eventDetailsPanel|boolean|false|none|none|
|»»» eventTableFields|[string]|false|none|none|
|»»» rowNumberColumnWidth|number|false|none|none|
|»»» showColumnTotals|boolean|true|none|none|
|»»» showColumnTotalsPinned|boolean|true|none|none|
|»»» showRowNumbers|boolean|true|none|none|
|»»» showRowTotals|boolean|true|none|none|
|»»» showRowTotalsPinned|boolean|true|none|none|
|»»» wrapCells|boolean|true|none|none|
|»» targetEventTime|number|false|none|none|
|»» timeCompleted|number|false|none|none|
|»» timeCreated|number|true|none|none|
|»» timeStarted|number|false|none|none|
|»» timeToFirstByte|number|false|none|none|
|»» totalBytesScanned|number|false|none|none|
|»» totalEventCount|number|false|none|none|
|»» type|string|false|none|none|
|»» usageGroupId|string|false|none|none|
|»» usageMetrics|[SearchAuditMetrics](#schemasearchauditmetrics)|false|none|none|
|»»» bytesIn|number|true|none|none|
|»»» bytesOut|number|true|none|none|
|»»» eventsIn|number|true|none|none|
|»»» eventsOut|number|true|none|none|
|»»» objects|object|true|none|none|
|»»»» discovered|number|true|none|none|
|»»»» scanned|number|true|none|none|
|»»»» skipped|number|true|none|none|
|»»» tasks|object|false|none|none|
|»»»» largeFileTaskCount|number|true|none|none|
|»»»» standardTaskCount|number|true|none|none|
|»»» time|object|true|none|none|
|»»»» queuedSec|number|true|none|none|
|»»»» runningSec|number|true|none|none|
|»»»» taskCompletionTotalSec|number|true|none|none|
|»»»» taskReceivingTotalSec|number|true|none|none|
|»» user|string|true|none|none|

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
|lib|custom|
|lib|cribl-custom|
|type|regex|
|type|json|
|type|json_array|
|type|header|
|type|timestamp|
|type|csv|
|type|aws_cloudtrail|
|type|aws_vpcflow|
|type|auto|
|type|format|
|type|current|
|type|string|
|type|number|
|type|boolean|
|usedCache|true|
|usedCache|false|
|type|stage|
|type|stage-scalar|
|type|aggregate|
|type|dedup|
|type|distinct|
|type|extract|
|type|filter|
|type|limit|
|type|mv-expand|
|type|mv-pull|
|type|noop|
|type|pivot|
|type|project|
|type|sort|
|direction|ascending|
|direction|descending|
|nullPosition|nullsFirst|
|nullPosition|nullsLast|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|
|type|command|
|type|standard|
|type|scheduled|
|type|datatypePreview|
|type|dashboard|
|type|notebook|
|type|systemInsights|

<aside class="success">
This operation does not require authentication
</aside>

## Get SearchJob by ID

<a id="opIdgetSearchJobById"></a>

> Code samples

`GET /search/jobs/{id}`

Get SearchJob by ID

<h3 id="get-searchjob-by-id-parameters">Parameters</h3>

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
      "accelerated": true,
      "aliasOfOriginalJobId": "string",
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
      "compatibilityChecks": {
        "datatypes": true,
        "stageIds": [
          "string"
        ]
      },
      "completionInfo": "string",
      "context": "string",
      "correlationId": "string",
      "cpuMetrics": {
        "billableCPUSeconds": 0,
        "executorsCPUSeconds": {
          "property1": 0,
          "property2": 0
        },
        "totalCPUSeconds": 0,
        "totalExecCPUSeconds": 0
      },
      "datatypeOverrides": {
        "breakerRulesets": [
          {
            "id": "string",
            "lib": "custom",
            "description": "string",
            "tags": "string",
            "minRawLength": 256,
            "rules": [
              {
                "name": "string",
                "condition": "true",
                "type": "regex",
                "timestampAnchorRegex": "/^/",
                "timestamp": {
                  "type": "auto",
                  "length": 150,
                  "format": "string"
                },
                "timestampTimezone": "local",
                "timestampEarliest": "-420weeks",
                "timestampLatest": "+1week",
                "maxEventBytes": 51200,
                "fields": [
                  {
                    "name": "string",
                    "value": "string"
                  }
                ],
                "disabled": false,
                "parserEnabled": false,
                "shouldUseDataRaw": false
              }
            ]
          }
        ],
        "disableBreakers": true
      },
      "disableNotifications": true,
      "displayUsername": "string",
      "earliest": "string",
      "earliestEpoch": 0,
      "errorStateConfig": {
        "coordinated": true,
        "errorMessages": [
          "string"
        ]
      },
      "group": "string",
      "id": "string",
      "isPrivate": true,
      "latest": "string",
      "latestEpoch": 0,
      "metadata": {
        "arguments": {
          "property1": [
            {
              "property1": "string",
              "property2": "string"
            }
          ],
          "property2": [
            {
              "property1": "string",
              "property2": "string"
            }
          ]
        },
        "computeTypes": {
          "v1": 0,
          "v2": 0,
          "lakehouse": 0
        },
        "datasets": {
          "property1": 0,
          "property2": 0
        },
        "functions": {
          "property1": 0,
          "property2": 0
        },
        "operators": {
          "property1": 0,
          "property2": 0
        },
        "providerTypes": {
          "property1": 0,
          "property2": 0
        },
        "providers": {
          "property1": 0,
          "property2": 0
        }
      },
      "notebookId": "string",
      "numEventsAfter": 0,
      "numEventsBefore": 0,
      "query": "string",
      "queryWithMacrosResolved": "string",
      "sampleRate": 0,
      "savedQueryName": "string",
      "searchParameterDeclarations": [
        {
          "defaultValue": "string",
          "name": "string",
          "type": "string"
        }
      ],
      "searchParameterValues": {
        "property1": "string",
        "property2": "string"
      },
      "setOptions": {
        "property1": "string",
        "property2": "string"
      },
      "stages": [
        {
          "cacheStatusByDatasetId": {
            "property1": {
              "usedCache": true
            },
            "property2": {
              "usedCache": true
            }
          },
          "dependencies": [
            {
              "dependentFields": [
                "string"
              ],
              "id": "string",
              "type": "stage"
            }
          ],
          "earliest": "string",
          "executionWarnings": [
            {
              "text": "string",
              "type": "string"
            }
          ],
          "filter": "string",
          "id": "string",
          "latest": "string",
          "resolvedDatasetIds": [
            "string"
          ],
          "searchConfig": {
            "canComputeMetadataDistributively": true,
            "datasets": [
              "string"
            ],
            "hasSendOperator": true,
            "logicalPlans": {
              "Combined": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              },
              "Coordinated": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              },
              "Federated": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              }
            },
            "orderedFieldNames": [
              "string"
            ],
            "pipelines": {
              "Combined": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              },
              "Coordinated": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              },
              "Federated": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              }
            },
            "referencedColumnPaths": [
              [
                "string"
              ]
            ],
            "searchTerms": [
              {
                "isCaseSensitive": true,
                "term": "string"
              }
            ],
            "sortFields": [
              {
                "direction": "ascending",
                "fieldName": "string",
                "nullPosition": "nullsFirst"
              }
            ],
            "useFormattedVisualization": true
          },
          "status": "failed",
          "subQueryText": "string"
        }
      ],
      "status": "failed",
      "tableConfig": {
        "columnFilterSettings": {
          "contains": {}
        },
        "columnFormatSettings": {
          "palette": {},
          "precision": {},
          "prefix": {},
          "suffix": {}
        },
        "columnOrderSettings": {
          "order": {}
        },
        "columnSortSettings": {
          "sort": {}
        },
        "eventDetailsPanel": true,
        "eventTableFields": [
          "string"
        ],
        "rowNumberColumnWidth": 0,
        "showColumnTotals": true,
        "showColumnTotalsPinned": true,
        "showRowNumbers": true,
        "showRowTotals": true,
        "showRowTotalsPinned": true,
        "wrapCells": true
      },
      "targetEventTime": 0,
      "timeCompleted": 0,
      "timeCreated": 0,
      "timeStarted": 0,
      "timeToFirstByte": 0,
      "totalBytesScanned": 0,
      "totalEventCount": 0,
      "type": "command",
      "usageGroupId": "string",
      "usageMetrics": {
        "bytesIn": 0,
        "bytesOut": 0,
        "eventsIn": 0,
        "eventsOut": 0,
        "objects": {
          "discovered": 0,
          "scanned": 0,
          "skipped": 0
        },
        "tasks": {
          "largeFileTaskCount": 0,
          "standardTaskCount": 0
        },
        "time": {
          "queuedSec": 0,
          "runningSec": 0,
          "taskCompletionTotalSec": 0,
          "taskReceivingTotalSec": 0
        }
      },
      "user": "string"
    }
  ]
}
```

<h3 id="get-searchjob-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchJob objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-searchjob-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchJob](#schemasearchjob)]|false|none|none|
|»» accelerated|boolean|false|none|none|
|»» aliasOfOriginalJobId|string|false|none|none|
|»» chartConfig|object|false|none|none|
|»»» applyThreshold|boolean|false|none|none|
|»»» axis|object|false|none|none|
|»»»» xAxis|string|false|none|none|
|»»»» yAxis|[string]|false|none|none|
|»»»» yAxisExcluded|[string]|false|none|none|
|»»» color|string|false|none|none|
|»»» colorPalette|number|false|none|none|
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
|»»» type|string|false|none|none|
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
|»» compatibilityChecks|object|false|none|none|
|»»» datatypes|boolean|false|none|none|
|»»» stageIds|[string]|false|none|none|
|»» completionInfo|string|false|none|none|
|»» context|string|false|none|none|
|»» correlationId|string|false|none|none|
|»» cpuMetrics|[CPUTimeMetric](#schemacputimemetric)|false|none|none|
|»»» billableCPUSeconds|number|false|none|none|
|»»» executorsCPUSeconds|object|true|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» totalCPUSeconds|number|true|none|none|
|»»» totalExecCPUSeconds|number|true|none|none|
|»» datatypeOverrides|[DatatypeOverrides](#schemadatatypeoverrides)|false|none|none|
|»»» breakerRulesets|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|none|
|»»»» id|string|true|none|none|
|»»»» lib|string|false|none|none|
|»»»» description|string|false|none|none|
|»»»» tags|string|false|none|none|
|»»»» minRawLength|number|false|none|The  minimum number of characters in _raw to determine which rule to use|
|»»»» rules|[object]|false|none|A list of rules that will be applied, in order, to the input data stream|
|»»»»» name|string|true|none|none|
|»»»»» condition|string|true|none|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»»»»» type|string|true|none|none|
|»»»»» timestampAnchorRegex|string|true|none|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»»»»» timestamp|object|true|none|Auto, manual format (strptime), or current time|
|»»»»»» type|string|true|none|none|
|»»»»»» length|number|false|none|none|
|»»»»»» format|string|false|none|none|
|»»»»» timestampTimezone|string|false|none|Timezone to assign to timestamps without timezone info|
|»»»»» timestampEarliest|string|false|none|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»»»»» timestampLatest|string|false|none|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»»»»» maxEventBytes|number|false|none|The maximum number of bytes in an event before it is flushed to the pipelines|
|»»»»» fields|[object]|false|none|Key-value pairs to be added to each event|
|»»»»»» name|string|false|none|none|
|»»»»»» value|string|true|none|The JavaScript expression used to compute the field's value (can be constant)|
|»»»»» disabled|boolean|false|none|Disable this breaker rule (enabled by default)|
|»»»»» parserEnabled|boolean|false|none|none|
|»»»»» shouldUseDataRaw|boolean|false|none|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|
|»»» disableBreakers|boolean|true|none|none|
|»» disableNotifications|boolean|false|none|none|
|»» displayUsername|string|true|none|none|
|»» earliest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» earliestEpoch|number|false|none|none|
|»» errorStateConfig|[SearchJobErrorStateConfig](#schemasearchjoberrorstateconfig)|false|none|none|
|»»» coordinated|boolean|true|none|none|
|»»» errorMessages|[string]|true|none|none|
|»» group|string|true|none|none|
|»» id|string|true|none|none|
|»» isPrivate|boolean|false|none|none|
|»» latest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» latestEpoch|number|false|none|none|
|»» metadata|[SearchJobMetadata](#schemasearchjobmetadata)|false|none|none|
|»»» arguments|object|false|none|none|
|»»»» **additionalProperties**|[object]|false|none|none|
|»»»»» **additionalProperties**|string|false|none|none|
|»»» computeTypes|object|false|none|none|
|»»»» v1|number|false|none|none|
|»»»» v2|number|false|none|none|
|»»»» lakehouse|number|false|none|none|
|»»» datasets|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» functions|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» operators|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» providerTypes|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» providers|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»» notebookId|string|false|none|none|
|»» numEventsAfter|number|false|none|none|
|»» numEventsBefore|number|false|none|none|
|»» query|string|true|none|none|
|»» queryWithMacrosResolved|string|false|none|none|
|»» sampleRate|number|false|none|none|
|»» savedQueryName|string|false|none|none|
|»» searchParameterDeclarations|[[SearchParameter](#schemasearchparameter)]|false|none|none|
|»»» defaultValue|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» name|string|true|none|none|
|»»» type|[SearchParameterType](#schemasearchparametertype)|true|none|none|
|»» searchParameterValues|[SearchParameters](#schemasearchparameters)|false|none|none|
|»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» setOptions|object|false|none|none|
|»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» stages|[[SearchJobStageConfig](#schemasearchjobstageconfig)]|false|none|none|
|»»» cacheStatusByDatasetId|[CacheStatusByDatasetId](#schemacachestatusbydatasetid)|false|none|none|
|»»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» usedCache|boolean|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» reason|string|true|none|none|
|»»»»»» usedCache|boolean|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» dependencies|[[StageDependency](#schemastagedependency)]|true|none|none|
|»»»» dependentFields|[string]|false|none|none|
|»»»» id|[StageId](#schemastageid)|true|none|none|
|»»»» type|[StageDependencyType](#schemastagedependencytype)|true|none|none|
|»»» earliest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» executionWarnings|[[JobExecutionWarning](#schemajobexecutionwarning)]|false|none|none|
|»»»» text|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» filter|string|true|none|none|
|»»» id|string|true|none|none|
|»»» latest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» resolvedDatasetIds|[string]|true|none|none|
|»»» searchConfig|[SearchConfig](#schemasearchconfig)|true|none|none|
|»»»» canComputeMetadataDistributively|boolean|false|none|none|
|»»»» datasets|[string]|true|none|none|
|»»»» hasSendOperator|boolean|true|none|none|
|»»»» logicalPlans|object|true|none|none|
|»»»»» Combined|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»»»» **additionalProperties**|[object]|false|none|none|
|»»»»»»» isPreviewableOperation|boolean|false|none|none|
|»»»»»»» type|[LogicalPlanNodeType](#schemalogicalplannodetype)|true|none|none|
|»»»»» Coordinated|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»»» Federated|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»» orderedFieldNames|[string]|true|none|none|
|»»»» pipelines|object|true|none|none|
|»»»»» Combined|[Pipeline](#schemapipeline)|false|none|none|
|»»»»»» id|string|true|none|none|
|»»»»»» conf|object|true|none|none|
|»»»»»»» asyncFuncTimeout|integer|false|none|Time (in ms) to wait for an async function to complete processing of a data item|
|»»»»»»» output|string|false|none|The output destination for events processed by this Pipeline|
|»»»»»»» description|string|false|none|none|
|»»»»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»»»»» functions|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|none|List of Functions to pass data through|
|»»»»»»»» filter|string|false|none|Filter that selects data to be fed through this Function|
|»»»»»»»» id|string|true|none|Function ID|
|»»»»»»»» description|string|false|none|Simple description of this step|
|»»»»»»»» disabled|boolean|false|none|If true, data will not be pushed through this function|
|»»»»»»»» final|boolean|false|none|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»»»»»»» conf|object|true|none|none|
|»»»»»»»» groupId|string|false|none|Group ID|
|»»»»»»» groups|object|false|none|none|
|»»»»»»»» **additionalProperties**|object|false|none|none|
|»»»»»»»»» name|string|true|none|none|
|»»»»»»»»» description|string|false|none|Short description of this group|
|»»»»»»»»» disabled|boolean|false|none|Whether this group is disabled|
|»»»»» Coordinated|[Pipeline](#schemapipeline)|false|none|none|
|»»»»» Federated|[Pipeline](#schemapipeline)|false|none|none|
|»»»» referencedColumnPaths|[[ColumnPath](#schemacolumnpath)]|false|none|none|
|»»»» searchTerms|[[SearchTerm](#schemasearchterm)]|true|none|none|
|»»»»» isCaseSensitive|boolean|true|none|none|
|»»»»» term|string|true|none|none|
|»»»» sortFields|[[SortByField](#schemasortbyfield)]|false|none|none|
|»»»»» direction|string|true|none|none|
|»»»»» fieldName|string|true|none|none|
|»»»»» nullPosition|string|true|none|none|
|»»»» useFormattedVisualization|boolean|true|none|none|
|»»» status|string|true|none|none|
|»»» subQueryText|string|true|none|none|
|»» status|string|true|none|none|
|»» tableConfig|[TableViewSettings](#schematableviewsettings)|false|none|none|
|»»» columnFilterSettings|[ColumnFilterSettings](#schemacolumnfiltersettings)|false|none|none|
|»»»» contains|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnFormatSettings|[ColumnFormatSettings](#schemacolumnformatsettings)|false|none|none|
|»»»» palette|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» precision|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» prefix|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» suffix|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnOrderSettings|[ColumnOrderSettings](#schemacolumnordersettings)|false|none|none|
|»»»» order|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnSortSettings|[ColumnSortSettings](#schemacolumnsortsettings)|false|none|none|
|»»»» sort|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» eventDetailsPanel|boolean|false|none|none|
|»»» eventTableFields|[string]|false|none|none|
|»»» rowNumberColumnWidth|number|false|none|none|
|»»» showColumnTotals|boolean|true|none|none|
|»»» showColumnTotalsPinned|boolean|true|none|none|
|»»» showRowNumbers|boolean|true|none|none|
|»»» showRowTotals|boolean|true|none|none|
|»»» showRowTotalsPinned|boolean|true|none|none|
|»»» wrapCells|boolean|true|none|none|
|»» targetEventTime|number|false|none|none|
|»» timeCompleted|number|false|none|none|
|»» timeCreated|number|true|none|none|
|»» timeStarted|number|false|none|none|
|»» timeToFirstByte|number|false|none|none|
|»» totalBytesScanned|number|false|none|none|
|»» totalEventCount|number|false|none|none|
|»» type|string|false|none|none|
|»» usageGroupId|string|false|none|none|
|»» usageMetrics|[SearchAuditMetrics](#schemasearchauditmetrics)|false|none|none|
|»»» bytesIn|number|true|none|none|
|»»» bytesOut|number|true|none|none|
|»»» eventsIn|number|true|none|none|
|»»» eventsOut|number|true|none|none|
|»»» objects|object|true|none|none|
|»»»» discovered|number|true|none|none|
|»»»» scanned|number|true|none|none|
|»»»» skipped|number|true|none|none|
|»»» tasks|object|false|none|none|
|»»»» largeFileTaskCount|number|true|none|none|
|»»»» standardTaskCount|number|true|none|none|
|»»» time|object|true|none|none|
|»»»» queuedSec|number|true|none|none|
|»»»» runningSec|number|true|none|none|
|»»»» taskCompletionTotalSec|number|true|none|none|
|»»»» taskReceivingTotalSec|number|true|none|none|
|»» user|string|true|none|none|

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
|lib|custom|
|lib|cribl-custom|
|type|regex|
|type|json|
|type|json_array|
|type|header|
|type|timestamp|
|type|csv|
|type|aws_cloudtrail|
|type|aws_vpcflow|
|type|auto|
|type|format|
|type|current|
|type|string|
|type|number|
|type|boolean|
|usedCache|true|
|usedCache|false|
|type|stage|
|type|stage-scalar|
|type|aggregate|
|type|dedup|
|type|distinct|
|type|extract|
|type|filter|
|type|limit|
|type|mv-expand|
|type|mv-pull|
|type|noop|
|type|pivot|
|type|project|
|type|sort|
|direction|ascending|
|direction|descending|
|nullPosition|nullsFirst|
|nullPosition|nullsLast|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|
|type|command|
|type|standard|
|type|scheduled|
|type|datatypePreview|
|type|dashboard|
|type|notebook|
|type|systemInsights|

<aside class="success">
This operation does not require authentication
</aside>

## Update SearchJob

<a id="opIdupdateSearchJobById"></a>

> Code samples

`PATCH /search/jobs/{id}`

Update SearchJob

> Body parameter

```json
{
  "accelerated": true,
  "aliasOfOriginalJobId": "string",
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
  "compatibilityChecks": {
    "datatypes": true,
    "stageIds": [
      "string"
    ]
  },
  "completionInfo": "string",
  "context": "string",
  "correlationId": "string",
  "cpuMetrics": {
    "billableCPUSeconds": 0,
    "executorsCPUSeconds": {
      "property1": 0,
      "property2": 0
    },
    "totalCPUSeconds": 0,
    "totalExecCPUSeconds": 0
  },
  "datatypeOverrides": {
    "breakerRulesets": [
      {
        "id": "string",
        "lib": "custom",
        "description": "string",
        "tags": "string",
        "minRawLength": 256,
        "rules": [
          {
            "name": "string",
            "condition": "true",
            "type": "regex",
            "timestampAnchorRegex": "/^/",
            "timestamp": {
              "type": "auto",
              "length": 150,
              "format": "string"
            },
            "timestampTimezone": "local",
            "timestampEarliest": "-420weeks",
            "timestampLatest": "+1week",
            "maxEventBytes": 51200,
            "fields": [
              {
                "name": "string",
                "value": "string"
              }
            ],
            "disabled": false,
            "parserEnabled": false,
            "shouldUseDataRaw": false
          }
        ]
      }
    ],
    "disableBreakers": true
  },
  "disableNotifications": true,
  "displayUsername": "string",
  "earliest": "string",
  "earliestEpoch": 0,
  "errorStateConfig": {
    "coordinated": true,
    "errorMessages": [
      "string"
    ]
  },
  "group": "string",
  "id": "string",
  "isPrivate": true,
  "latest": "string",
  "latestEpoch": 0,
  "metadata": {
    "arguments": {
      "property1": [
        {
          "property1": "string",
          "property2": "string"
        }
      ],
      "property2": [
        {
          "property1": "string",
          "property2": "string"
        }
      ]
    },
    "computeTypes": {
      "v1": 0,
      "v2": 0,
      "lakehouse": 0
    },
    "datasets": {
      "property1": 0,
      "property2": 0
    },
    "functions": {
      "property1": 0,
      "property2": 0
    },
    "operators": {
      "property1": 0,
      "property2": 0
    },
    "providerTypes": {
      "property1": 0,
      "property2": 0
    },
    "providers": {
      "property1": 0,
      "property2": 0
    }
  },
  "notebookId": "string",
  "numEventsAfter": 0,
  "numEventsBefore": 0,
  "query": "string",
  "queryWithMacrosResolved": "string",
  "sampleRate": 0,
  "savedQueryName": "string",
  "searchParameterDeclarations": [
    {
      "defaultValue": "string",
      "name": "string",
      "type": "string"
    }
  ],
  "searchParameterValues": {
    "property1": "string",
    "property2": "string"
  },
  "setOptions": {
    "property1": "string",
    "property2": "string"
  },
  "stages": [
    {
      "cacheStatusByDatasetId": {
        "property1": {
          "usedCache": true
        },
        "property2": {
          "usedCache": true
        }
      },
      "dependencies": [
        {
          "dependentFields": [
            "string"
          ],
          "id": "string",
          "type": "stage"
        }
      ],
      "earliest": "string",
      "executionWarnings": [
        {
          "text": "string",
          "type": "string"
        }
      ],
      "filter": "string",
      "id": "string",
      "latest": "string",
      "resolvedDatasetIds": [
        "string"
      ],
      "searchConfig": {
        "canComputeMetadataDistributively": true,
        "datasets": [
          "string"
        ],
        "hasSendOperator": true,
        "logicalPlans": {
          "Combined": {
            "property1": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ],
            "property2": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ]
          },
          "Coordinated": {
            "property1": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ],
            "property2": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ]
          },
          "Federated": {
            "property1": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ],
            "property2": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ]
          }
        },
        "orderedFieldNames": [
          "string"
        ],
        "pipelines": {
          "Combined": {
            "id": "string",
            "conf": {
              "asyncFuncTimeout": 10000,
              "output": "default",
              "description": "string",
              "streamtags": [],
              "functions": [
                {
                  "filter": "true",
                  "id": "string",
                  "description": "string",
                  "disabled": true,
                  "final": true,
                  "conf": {},
                  "groupId": "string"
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
              }
            }
          },
          "Coordinated": {
            "id": "string",
            "conf": {
              "asyncFuncTimeout": 10000,
              "output": "default",
              "description": "string",
              "streamtags": [],
              "functions": [
                {
                  "filter": "true",
                  "id": "string",
                  "description": "string",
                  "disabled": true,
                  "final": true,
                  "conf": {},
                  "groupId": "string"
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
              }
            }
          },
          "Federated": {
            "id": "string",
            "conf": {
              "asyncFuncTimeout": 10000,
              "output": "default",
              "description": "string",
              "streamtags": [],
              "functions": [
                {
                  "filter": "true",
                  "id": "string",
                  "description": "string",
                  "disabled": true,
                  "final": true,
                  "conf": {},
                  "groupId": "string"
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
              }
            }
          }
        },
        "referencedColumnPaths": [
          [
            "string"
          ]
        ],
        "searchTerms": [
          {
            "isCaseSensitive": true,
            "term": "string"
          }
        ],
        "sortFields": [
          {
            "direction": "ascending",
            "fieldName": "string",
            "nullPosition": "nullsFirst"
          }
        ],
        "useFormattedVisualization": true
      },
      "status": "failed",
      "subQueryText": "string"
    }
  ],
  "status": "failed",
  "tableConfig": {
    "columnFilterSettings": {
      "contains": {}
    },
    "columnFormatSettings": {
      "palette": {},
      "precision": {},
      "prefix": {},
      "suffix": {}
    },
    "columnOrderSettings": {
      "order": {}
    },
    "columnSortSettings": {
      "sort": {}
    },
    "eventDetailsPanel": true,
    "eventTableFields": [
      "string"
    ],
    "rowNumberColumnWidth": 0,
    "showColumnTotals": true,
    "showColumnTotalsPinned": true,
    "showRowNumbers": true,
    "showRowTotals": true,
    "showRowTotalsPinned": true,
    "wrapCells": true
  },
  "targetEventTime": 0,
  "timeCompleted": 0,
  "timeCreated": 0,
  "timeStarted": 0,
  "timeToFirstByte": 0,
  "totalBytesScanned": 0,
  "totalEventCount": 0,
  "type": "command",
  "usageGroupId": "string",
  "usageMetrics": {
    "bytesIn": 0,
    "bytesOut": 0,
    "eventsIn": 0,
    "eventsOut": 0,
    "objects": {
      "discovered": 0,
      "scanned": 0,
      "skipped": 0
    },
    "tasks": {
      "largeFileTaskCount": 0,
      "standardTaskCount": 0
    },
    "time": {
      "queuedSec": 0,
      "runningSec": 0,
      "taskCompletionTotalSec": 0,
      "taskReceivingTotalSec": 0
    }
  },
  "user": "string"
}
```

<h3 id="update-searchjob-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[SearchJob](#schemasearchjob)|true|SearchJob object to be updated|
|» accelerated|body|boolean|false|none|
|» aliasOfOriginalJobId|body|string|false|none|
|» chartConfig|body|object|false|none|
|»» applyThreshold|body|boolean|false|none|
|»» axis|body|object|false|none|
|»»» xAxis|body|string|false|none|
|»»» yAxis|body|[string]|false|none|
|»»» yAxisExcluded|body|[string]|false|none|
|»» color|body|string|false|none|
|»» colorPalette|body|number|false|none|
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
|»» type|body|string|false|none|
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
|» compatibilityChecks|body|object|false|none|
|»» datatypes|body|boolean|false|none|
|»» stageIds|body|[string]|false|none|
|» completionInfo|body|string|false|none|
|» context|body|string|false|none|
|» correlationId|body|string|false|none|
|» cpuMetrics|body|[CPUTimeMetric](#schemacputimemetric)|false|none|
|»» billableCPUSeconds|body|number|false|none|
|»» executorsCPUSeconds|body|object|true|none|
|»»» **additionalProperties**|body|number|false|none|
|»» totalCPUSeconds|body|number|true|none|
|»» totalExecCPUSeconds|body|number|true|none|
|» datatypeOverrides|body|[DatatypeOverrides](#schemadatatypeoverrides)|false|none|
|»» breakerRulesets|body|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|
|»»» id|body|string|true|none|
|»»» lib|body|string|false|none|
|»»» description|body|string|false|none|
|»»» tags|body|string|false|none|
|»»» minRawLength|body|number|false|The  minimum number of characters in _raw to determine which rule to use|
|»»» rules|body|[object]|false|A list of rules that will be applied, in order, to the input data stream|
|»»»» name|body|string|true|none|
|»»»» condition|body|string|true|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»»»» type|body|string|true|none|
|»»»» timestampAnchorRegex|body|string|true|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»»»» timestamp|body|object|true|Auto, manual format (strptime), or current time|
|»»»»» type|body|string|true|none|
|»»»»» length|body|number|false|none|
|»»»»» format|body|string|false|none|
|»»»» timestampTimezone|body|string|false|Timezone to assign to timestamps without timezone info|
|»»»» timestampEarliest|body|string|false|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»»»» timestampLatest|body|string|false|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»»»» maxEventBytes|body|number|false|The maximum number of bytes in an event before it is flushed to the pipelines|
|»»»» fields|body|[object]|false|Key-value pairs to be added to each event|
|»»»»» name|body|string|false|none|
|»»»»» value|body|string|true|The JavaScript expression used to compute the field's value (can be constant)|
|»»»» disabled|body|boolean|false|Disable this breaker rule (enabled by default)|
|»»»» parserEnabled|body|boolean|false|none|
|»»»» shouldUseDataRaw|body|boolean|false|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|
|»» disableBreakers|body|boolean|true|none|
|» disableNotifications|body|boolean|false|none|
|» displayUsername|body|string|true|none|
|» earliest|body|any|false|none|
|»» *anonymous*|body|string|false|none|
|»» *anonymous*|body|number|false|none|
|» earliestEpoch|body|number|false|none|
|» errorStateConfig|body|[SearchJobErrorStateConfig](#schemasearchjoberrorstateconfig)|false|none|
|»» coordinated|body|boolean|true|none|
|»» errorMessages|body|[string]|true|none|
|» group|body|string|true|none|
|» id|body|string|true|none|
|» isPrivate|body|boolean|false|none|
|» latest|body|any|false|none|
|»» *anonymous*|body|string|false|none|
|»» *anonymous*|body|number|false|none|
|» latestEpoch|body|number|false|none|
|» metadata|body|[SearchJobMetadata](#schemasearchjobmetadata)|false|none|
|»» arguments|body|object|false|none|
|»»» **additionalProperties**|body|[object]|false|none|
|»»»» **additionalProperties**|body|string|false|none|
|»» computeTypes|body|object|false|none|
|»»» v1|body|number|false|none|
|»»» v2|body|number|false|none|
|»»» lakehouse|body|number|false|none|
|»» datasets|body|object|false|none|
|»»» **additionalProperties**|body|number|false|none|
|»» functions|body|object|false|none|
|»»» **additionalProperties**|body|number|false|none|
|»» operators|body|object|false|none|
|»»» **additionalProperties**|body|number|false|none|
|»» providerTypes|body|object|false|none|
|»»» **additionalProperties**|body|number|false|none|
|»» providers|body|object|false|none|
|»»» **additionalProperties**|body|number|false|none|
|» notebookId|body|string|false|none|
|» numEventsAfter|body|number|false|none|
|» numEventsBefore|body|number|false|none|
|» query|body|string|true|none|
|» queryWithMacrosResolved|body|string|false|none|
|» sampleRate|body|number|false|none|
|» savedQueryName|body|string|false|none|
|» searchParameterDeclarations|body|[[SearchParameter](#schemasearchparameter)]|false|none|
|»» defaultValue|body|any|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|number|false|none|
|»»» *anonymous*|body|boolean|false|none|
|»» name|body|string|true|none|
|»» type|body|[SearchParameterType](#schemasearchparametertype)|true|none|
|» searchParameterValues|body|[SearchParameters](#schemasearchparameters)|false|none|
|»» **additionalProperties**|body|any|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|number|false|none|
|»»» *anonymous*|body|boolean|false|none|
|» setOptions|body|object|false|none|
|»» **additionalProperties**|body|any|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|number|false|none|
|» stages|body|[[SearchJobStageConfig](#schemasearchjobstageconfig)]|false|none|
|»» cacheStatusByDatasetId|body|[CacheStatusByDatasetId](#schemacachestatusbydatasetid)|false|none|
|»»» **additionalProperties**|body|any|false|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» usedCache|body|boolean|true|none|
|»»»» *anonymous*|body|object|false|none|
|»»»»» reason|body|string|true|none|
|»»»»» usedCache|body|boolean|true|none|
|»» dependencies|body|[[StageDependency](#schemastagedependency)]|true|none|
|»»» dependentFields|body|[string]|false|none|
|»»» id|body|[StageId](#schemastageid)|true|none|
|»»» type|body|[StageDependencyType](#schemastagedependencytype)|true|none|
|»» earliest|body|any|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|number|false|none|
|»» executionWarnings|body|[[JobExecutionWarning](#schemajobexecutionwarning)]|false|none|
|»»» text|body|string|false|none|
|»»» type|body|string|true|none|
|»» filter|body|string|true|none|
|»» id|body|string|true|none|
|»» latest|body|any|false|none|
|»»» *anonymous*|body|string|false|none|
|»»» *anonymous*|body|number|false|none|
|»» resolvedDatasetIds|body|[string]|true|none|
|»» searchConfig|body|[SearchConfig](#schemasearchconfig)|true|none|
|»»» canComputeMetadataDistributively|body|boolean|false|none|
|»»» datasets|body|[string]|true|none|
|»»» hasSendOperator|body|boolean|true|none|
|»»» logicalPlans|body|object|true|none|
|»»»» Combined|body|[LogicalPlan](#schemalogicalplan)|false|none|
|»»»»» **additionalProperties**|body|[object]|false|none|
|»»»»»» isPreviewableOperation|body|boolean|false|none|
|»»»»»» type|body|[LogicalPlanNodeType](#schemalogicalplannodetype)|true|none|
|»»»» Coordinated|body|[LogicalPlan](#schemalogicalplan)|false|none|
|»»»» Federated|body|[LogicalPlan](#schemalogicalplan)|false|none|
|»»» orderedFieldNames|body|[string]|true|none|
|»»» pipelines|body|object|true|none|
|»»»» Combined|body|[Pipeline](#schemapipeline)|false|none|
|»»»»» id|body|string|true|none|
|»»»»» conf|body|object|true|none|
|»»»»»» asyncFuncTimeout|body|integer|false|Time (in ms) to wait for an async function to complete processing of a data item|
|»»»»»» output|body|string|false|The output destination for events processed by this Pipeline|
|»»»»»» description|body|string|false|none|
|»»»»»» streamtags|body|[string]|false|Tags for filtering and grouping in @{product}|
|»»»»»» functions|body|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|List of Functions to pass data through|
|»»»»»»» filter|body|string|false|Filter that selects data to be fed through this Function|
|»»»»»»» id|body|string|true|Function ID|
|»»»»»»» description|body|string|false|Simple description of this step|
|»»»»»»» disabled|body|boolean|false|If true, data will not be pushed through this function|
|»»»»»»» final|body|boolean|false|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»»»»»» conf|body|object|true|none|
|»»»»»»» groupId|body|string|false|Group ID|
|»»»»»» groups|body|object|false|none|
|»»»»»»» **additionalProperties**|body|object|false|none|
|»»»»»»»» name|body|string|true|none|
|»»»»»»»» description|body|string|false|Short description of this group|
|»»»»»»»» disabled|body|boolean|false|Whether this group is disabled|
|»»»» Coordinated|body|[Pipeline](#schemapipeline)|false|none|
|»»»» Federated|body|[Pipeline](#schemapipeline)|false|none|
|»»» referencedColumnPaths|body|[[ColumnPath](#schemacolumnpath)]|false|none|
|»»» searchTerms|body|[[SearchTerm](#schemasearchterm)]|true|none|
|»»»» isCaseSensitive|body|boolean|true|none|
|»»»» term|body|string|true|none|
|»»» sortFields|body|[[SortByField](#schemasortbyfield)]|false|none|
|»»»» direction|body|string|true|none|
|»»»» fieldName|body|string|true|none|
|»»»» nullPosition|body|string|true|none|
|»»» useFormattedVisualization|body|boolean|true|none|
|»» status|body|string|true|none|
|»» subQueryText|body|string|true|none|
|» status|body|string|true|none|
|» tableConfig|body|[TableViewSettings](#schematableviewsettings)|false|none|
|»» columnFilterSettings|body|[ColumnFilterSettings](#schemacolumnfiltersettings)|false|none|
|»»» contains|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»» columnFormatSettings|body|[ColumnFormatSettings](#schemacolumnformatsettings)|false|none|
|»»» palette|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»»» precision|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»»» prefix|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»»» suffix|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»» columnOrderSettings|body|[ColumnOrderSettings](#schemacolumnordersettings)|false|none|
|»»» order|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»» columnSortSettings|body|[ColumnSortSettings](#schemacolumnsortsettings)|false|none|
|»»» sort|body|[ColumnSetting](#schemacolumnsetting)|true|none|
|»» eventDetailsPanel|body|boolean|false|none|
|»» eventTableFields|body|[string]|false|none|
|»» rowNumberColumnWidth|body|number|false|none|
|»» showColumnTotals|body|boolean|true|none|
|»» showColumnTotalsPinned|body|boolean|true|none|
|»» showRowNumbers|body|boolean|true|none|
|»» showRowTotals|body|boolean|true|none|
|»» showRowTotalsPinned|body|boolean|true|none|
|»» wrapCells|body|boolean|true|none|
|» targetEventTime|body|number|false|none|
|» timeCompleted|body|number|false|none|
|» timeCreated|body|number|true|none|
|» timeStarted|body|number|false|none|
|» timeToFirstByte|body|number|false|none|
|» totalBytesScanned|body|number|false|none|
|» totalEventCount|body|number|false|none|
|» type|body|string|false|none|
|» usageGroupId|body|string|false|none|
|» usageMetrics|body|[SearchAuditMetrics](#schemasearchauditmetrics)|false|none|
|»» bytesIn|body|number|true|none|
|»» bytesOut|body|number|true|none|
|»» eventsIn|body|number|true|none|
|»» eventsOut|body|number|true|none|
|»» objects|body|object|true|none|
|»»» discovered|body|number|true|none|
|»»» scanned|body|number|true|none|
|»»» skipped|body|number|true|none|
|»» tasks|body|object|false|none|
|»»» largeFileTaskCount|body|number|true|none|
|»»» standardTaskCount|body|number|true|none|
|»» time|body|object|true|none|
|»»» queuedSec|body|number|true|none|
|»»» runningSec|body|number|true|none|
|»»» taskCompletionTotalSec|body|number|true|none|
|»»» taskReceivingTotalSec|body|number|true|none|
|» user|body|string|true|none|

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
|»»» lib|custom|
|»»» lib|cribl-custom|
|»»»» type|regex|
|»»»» type|json|
|»»»» type|json_array|
|»»»» type|header|
|»»»» type|timestamp|
|»»»» type|csv|
|»»»» type|aws_cloudtrail|
|»»»» type|aws_vpcflow|
|»»»»» type|auto|
|»»»»» type|format|
|»»»»» type|current|
|»» type|string|
|»» type|number|
|»» type|boolean|
|»»»»» usedCache|true|
|»»»»» usedCache|false|
|»»» type|stage|
|»»» type|stage-scalar|
|»»»»»» type|aggregate|
|»»»»»» type|dedup|
|»»»»»» type|distinct|
|»»»»»» type|extract|
|»»»»»» type|filter|
|»»»»»» type|limit|
|»»»»»» type|mv-expand|
|»»»»»» type|mv-pull|
|»»»»»» type|noop|
|»»»»»» type|pivot|
|»»»»»» type|project|
|»»»»»» type|sort|
|»»»» direction|ascending|
|»»»» direction|descending|
|»»»» nullPosition|nullsFirst|
|»»»» nullPosition|nullsLast|
|»» status|failed|
|»» status|new|
|»» status|running|
|»» status|completed|
|»» status|canceled|
|»» status|queued|
|»» status|finalizing|
|» status|failed|
|» status|new|
|» status|running|
|» status|completed|
|» status|canceled|
|» status|queued|
|» status|finalizing|
|» type|command|
|» type|standard|
|» type|scheduled|
|» type|datatypePreview|
|» type|dashboard|
|» type|notebook|
|» type|systemInsights|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "accelerated": true,
      "aliasOfOriginalJobId": "string",
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
      "compatibilityChecks": {
        "datatypes": true,
        "stageIds": [
          "string"
        ]
      },
      "completionInfo": "string",
      "context": "string",
      "correlationId": "string",
      "cpuMetrics": {
        "billableCPUSeconds": 0,
        "executorsCPUSeconds": {
          "property1": 0,
          "property2": 0
        },
        "totalCPUSeconds": 0,
        "totalExecCPUSeconds": 0
      },
      "datatypeOverrides": {
        "breakerRulesets": [
          {
            "id": "string",
            "lib": "custom",
            "description": "string",
            "tags": "string",
            "minRawLength": 256,
            "rules": [
              {
                "name": "string",
                "condition": "true",
                "type": "regex",
                "timestampAnchorRegex": "/^/",
                "timestamp": {
                  "type": "auto",
                  "length": 150,
                  "format": "string"
                },
                "timestampTimezone": "local",
                "timestampEarliest": "-420weeks",
                "timestampLatest": "+1week",
                "maxEventBytes": 51200,
                "fields": [
                  {
                    "name": "string",
                    "value": "string"
                  }
                ],
                "disabled": false,
                "parserEnabled": false,
                "shouldUseDataRaw": false
              }
            ]
          }
        ],
        "disableBreakers": true
      },
      "disableNotifications": true,
      "displayUsername": "string",
      "earliest": "string",
      "earliestEpoch": 0,
      "errorStateConfig": {
        "coordinated": true,
        "errorMessages": [
          "string"
        ]
      },
      "group": "string",
      "id": "string",
      "isPrivate": true,
      "latest": "string",
      "latestEpoch": 0,
      "metadata": {
        "arguments": {
          "property1": [
            {
              "property1": "string",
              "property2": "string"
            }
          ],
          "property2": [
            {
              "property1": "string",
              "property2": "string"
            }
          ]
        },
        "computeTypes": {
          "v1": 0,
          "v2": 0,
          "lakehouse": 0
        },
        "datasets": {
          "property1": 0,
          "property2": 0
        },
        "functions": {
          "property1": 0,
          "property2": 0
        },
        "operators": {
          "property1": 0,
          "property2": 0
        },
        "providerTypes": {
          "property1": 0,
          "property2": 0
        },
        "providers": {
          "property1": 0,
          "property2": 0
        }
      },
      "notebookId": "string",
      "numEventsAfter": 0,
      "numEventsBefore": 0,
      "query": "string",
      "queryWithMacrosResolved": "string",
      "sampleRate": 0,
      "savedQueryName": "string",
      "searchParameterDeclarations": [
        {
          "defaultValue": "string",
          "name": "string",
          "type": "string"
        }
      ],
      "searchParameterValues": {
        "property1": "string",
        "property2": "string"
      },
      "setOptions": {
        "property1": "string",
        "property2": "string"
      },
      "stages": [
        {
          "cacheStatusByDatasetId": {
            "property1": {
              "usedCache": true
            },
            "property2": {
              "usedCache": true
            }
          },
          "dependencies": [
            {
              "dependentFields": [
                "string"
              ],
              "id": "string",
              "type": "stage"
            }
          ],
          "earliest": "string",
          "executionWarnings": [
            {
              "text": "string",
              "type": "string"
            }
          ],
          "filter": "string",
          "id": "string",
          "latest": "string",
          "resolvedDatasetIds": [
            "string"
          ],
          "searchConfig": {
            "canComputeMetadataDistributively": true,
            "datasets": [
              "string"
            ],
            "hasSendOperator": true,
            "logicalPlans": {
              "Combined": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              },
              "Coordinated": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              },
              "Federated": {
                "property1": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ],
                "property2": [
                  {
                    "isPreviewableOperation": true,
                    "type": "aggregate"
                  }
                ]
              }
            },
            "orderedFieldNames": [
              "string"
            ],
            "pipelines": {
              "Combined": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              },
              "Coordinated": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              },
              "Federated": {
                "id": "string",
                "conf": {
                  "asyncFuncTimeout": 10000,
                  "output": "default",
                  "description": "string",
                  "streamtags": [],
                  "functions": [
                    {
                      "filter": "true",
                      "id": "string",
                      "description": "string",
                      "disabled": true,
                      "final": true,
                      "conf": {},
                      "groupId": "string"
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
                  }
                }
              }
            },
            "referencedColumnPaths": [
              [
                "string"
              ]
            ],
            "searchTerms": [
              {
                "isCaseSensitive": true,
                "term": "string"
              }
            ],
            "sortFields": [
              {
                "direction": "ascending",
                "fieldName": "string",
                "nullPosition": "nullsFirst"
              }
            ],
            "useFormattedVisualization": true
          },
          "status": "failed",
          "subQueryText": "string"
        }
      ],
      "status": "failed",
      "tableConfig": {
        "columnFilterSettings": {
          "contains": {}
        },
        "columnFormatSettings": {
          "palette": {},
          "precision": {},
          "prefix": {},
          "suffix": {}
        },
        "columnOrderSettings": {
          "order": {}
        },
        "columnSortSettings": {
          "sort": {}
        },
        "eventDetailsPanel": true,
        "eventTableFields": [
          "string"
        ],
        "rowNumberColumnWidth": 0,
        "showColumnTotals": true,
        "showColumnTotalsPinned": true,
        "showRowNumbers": true,
        "showRowTotals": true,
        "showRowTotalsPinned": true,
        "wrapCells": true
      },
      "targetEventTime": 0,
      "timeCompleted": 0,
      "timeCreated": 0,
      "timeStarted": 0,
      "timeToFirstByte": 0,
      "totalBytesScanned": 0,
      "totalEventCount": 0,
      "type": "command",
      "usageGroupId": "string",
      "usageMetrics": {
        "bytesIn": 0,
        "bytesOut": 0,
        "eventsIn": 0,
        "eventsOut": 0,
        "objects": {
          "discovered": 0,
          "scanned": 0,
          "skipped": 0
        },
        "tasks": {
          "largeFileTaskCount": 0,
          "standardTaskCount": 0
        },
        "time": {
          "queuedSec": 0,
          "runningSec": 0,
          "taskCompletionTotalSec": 0,
          "taskReceivingTotalSec": 0
        }
      },
      "user": "string"
    }
  ]
}
```

<h3 id="update-searchjob-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchJob objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-searchjob-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchJob](#schemasearchjob)]|false|none|none|
|»» accelerated|boolean|false|none|none|
|»» aliasOfOriginalJobId|string|false|none|none|
|»» chartConfig|object|false|none|none|
|»»» applyThreshold|boolean|false|none|none|
|»»» axis|object|false|none|none|
|»»»» xAxis|string|false|none|none|
|»»»» yAxis|[string]|false|none|none|
|»»»» yAxisExcluded|[string]|false|none|none|
|»»» color|string|false|none|none|
|»»» colorPalette|number|false|none|none|
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
|»»» type|string|false|none|none|
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
|»» compatibilityChecks|object|false|none|none|
|»»» datatypes|boolean|false|none|none|
|»»» stageIds|[string]|false|none|none|
|»» completionInfo|string|false|none|none|
|»» context|string|false|none|none|
|»» correlationId|string|false|none|none|
|»» cpuMetrics|[CPUTimeMetric](#schemacputimemetric)|false|none|none|
|»»» billableCPUSeconds|number|false|none|none|
|»»» executorsCPUSeconds|object|true|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» totalCPUSeconds|number|true|none|none|
|»»» totalExecCPUSeconds|number|true|none|none|
|»» datatypeOverrides|[DatatypeOverrides](#schemadatatypeoverrides)|false|none|none|
|»»» breakerRulesets|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|none|
|»»»» id|string|true|none|none|
|»»»» lib|string|false|none|none|
|»»»» description|string|false|none|none|
|»»»» tags|string|false|none|none|
|»»»» minRawLength|number|false|none|The  minimum number of characters in _raw to determine which rule to use|
|»»»» rules|[object]|false|none|A list of rules that will be applied, in order, to the input data stream|
|»»»»» name|string|true|none|none|
|»»»»» condition|string|true|none|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|»»»»» type|string|true|none|none|
|»»»»» timestampAnchorRegex|string|true|none|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|»»»»» timestamp|object|true|none|Auto, manual format (strptime), or current time|
|»»»»»» type|string|true|none|none|
|»»»»»» length|number|false|none|none|
|»»»»»» format|string|false|none|none|
|»»»»» timestampTimezone|string|false|none|Timezone to assign to timestamps without timezone info|
|»»»»» timestampEarliest|string|false|none|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|»»»»» timestampLatest|string|false|none|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|»»»»» maxEventBytes|number|false|none|The maximum number of bytes in an event before it is flushed to the pipelines|
|»»»»» fields|[object]|false|none|Key-value pairs to be added to each event|
|»»»»»» name|string|false|none|none|
|»»»»»» value|string|true|none|The JavaScript expression used to compute the field's value (can be constant)|
|»»»»» disabled|boolean|false|none|Disable this breaker rule (enabled by default)|
|»»»»» parserEnabled|boolean|false|none|none|
|»»»»» shouldUseDataRaw|boolean|false|none|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|
|»»» disableBreakers|boolean|true|none|none|
|»» disableNotifications|boolean|false|none|none|
|»» displayUsername|string|true|none|none|
|»» earliest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» earliestEpoch|number|false|none|none|
|»» errorStateConfig|[SearchJobErrorStateConfig](#schemasearchjoberrorstateconfig)|false|none|none|
|»»» coordinated|boolean|true|none|none|
|»»» errorMessages|[string]|true|none|none|
|»» group|string|true|none|none|
|»» id|string|true|none|none|
|»» isPrivate|boolean|false|none|none|
|»» latest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» latestEpoch|number|false|none|none|
|»» metadata|[SearchJobMetadata](#schemasearchjobmetadata)|false|none|none|
|»»» arguments|object|false|none|none|
|»»»» **additionalProperties**|[object]|false|none|none|
|»»»»» **additionalProperties**|string|false|none|none|
|»»» computeTypes|object|false|none|none|
|»»»» v1|number|false|none|none|
|»»»» v2|number|false|none|none|
|»»»» lakehouse|number|false|none|none|
|»»» datasets|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» functions|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» operators|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» providerTypes|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»»» providers|object|false|none|none|
|»»»» **additionalProperties**|number|false|none|none|
|»» notebookId|string|false|none|none|
|»» numEventsAfter|number|false|none|none|
|»» numEventsBefore|number|false|none|none|
|»» query|string|true|none|none|
|»» queryWithMacrosResolved|string|false|none|none|
|»» sampleRate|number|false|none|none|
|»» savedQueryName|string|false|none|none|
|»» searchParameterDeclarations|[[SearchParameter](#schemasearchparameter)]|false|none|none|
|»»» defaultValue|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» name|string|true|none|none|
|»»» type|[SearchParameterType](#schemasearchparametertype)|true|none|none|
|»» searchParameterValues|[SearchParameters](#schemasearchparameters)|false|none|none|
|»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» setOptions|object|false|none|none|
|»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» stages|[[SearchJobStageConfig](#schemasearchjobstageconfig)]|false|none|none|
|»»» cacheStatusByDatasetId|[CacheStatusByDatasetId](#schemacachestatusbydatasetid)|false|none|none|
|»»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» usedCache|boolean|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» reason|string|true|none|none|
|»»»»»» usedCache|boolean|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» dependencies|[[StageDependency](#schemastagedependency)]|true|none|none|
|»»»» dependentFields|[string]|false|none|none|
|»»»» id|[StageId](#schemastageid)|true|none|none|
|»»»» type|[StageDependencyType](#schemastagedependencytype)|true|none|none|
|»»» earliest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» executionWarnings|[[JobExecutionWarning](#schemajobexecutionwarning)]|false|none|none|
|»»»» text|string|false|none|none|
|»»»» type|string|true|none|none|
|»»» filter|string|true|none|none|
|»»» id|string|true|none|none|
|»»» latest|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» resolvedDatasetIds|[string]|true|none|none|
|»»» searchConfig|[SearchConfig](#schemasearchconfig)|true|none|none|
|»»»» canComputeMetadataDistributively|boolean|false|none|none|
|»»»» datasets|[string]|true|none|none|
|»»»» hasSendOperator|boolean|true|none|none|
|»»»» logicalPlans|object|true|none|none|
|»»»»» Combined|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»»»» **additionalProperties**|[object]|false|none|none|
|»»»»»»» isPreviewableOperation|boolean|false|none|none|
|»»»»»»» type|[LogicalPlanNodeType](#schemalogicalplannodetype)|true|none|none|
|»»»»» Coordinated|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»»» Federated|[LogicalPlan](#schemalogicalplan)|false|none|none|
|»»»» orderedFieldNames|[string]|true|none|none|
|»»»» pipelines|object|true|none|none|
|»»»»» Combined|[Pipeline](#schemapipeline)|false|none|none|
|»»»»»» id|string|true|none|none|
|»»»»»» conf|object|true|none|none|
|»»»»»»» asyncFuncTimeout|integer|false|none|Time (in ms) to wait for an async function to complete processing of a data item|
|»»»»»»» output|string|false|none|The output destination for events processed by this Pipeline|
|»»»»»»» description|string|false|none|none|
|»»»»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»»»»» functions|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|none|List of Functions to pass data through|
|»»»»»»»» filter|string|false|none|Filter that selects data to be fed through this Function|
|»»»»»»»» id|string|true|none|Function ID|
|»»»»»»»» description|string|false|none|Simple description of this step|
|»»»»»»»» disabled|boolean|false|none|If true, data will not be pushed through this function|
|»»»»»»»» final|boolean|false|none|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»»»»»»» conf|object|true|none|none|
|»»»»»»»» groupId|string|false|none|Group ID|
|»»»»»»» groups|object|false|none|none|
|»»»»»»»» **additionalProperties**|object|false|none|none|
|»»»»»»»»» name|string|true|none|none|
|»»»»»»»»» description|string|false|none|Short description of this group|
|»»»»»»»»» disabled|boolean|false|none|Whether this group is disabled|
|»»»»» Coordinated|[Pipeline](#schemapipeline)|false|none|none|
|»»»»» Federated|[Pipeline](#schemapipeline)|false|none|none|
|»»»» referencedColumnPaths|[[ColumnPath](#schemacolumnpath)]|false|none|none|
|»»»» searchTerms|[[SearchTerm](#schemasearchterm)]|true|none|none|
|»»»»» isCaseSensitive|boolean|true|none|none|
|»»»»» term|string|true|none|none|
|»»»» sortFields|[[SortByField](#schemasortbyfield)]|false|none|none|
|»»»»» direction|string|true|none|none|
|»»»»» fieldName|string|true|none|none|
|»»»»» nullPosition|string|true|none|none|
|»»»» useFormattedVisualization|boolean|true|none|none|
|»»» status|string|true|none|none|
|»»» subQueryText|string|true|none|none|
|»» status|string|true|none|none|
|»» tableConfig|[TableViewSettings](#schematableviewsettings)|false|none|none|
|»»» columnFilterSettings|[ColumnFilterSettings](#schemacolumnfiltersettings)|false|none|none|
|»»»» contains|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnFormatSettings|[ColumnFormatSettings](#schemacolumnformatsettings)|false|none|none|
|»»»» palette|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» precision|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» prefix|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»»» suffix|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnOrderSettings|[ColumnOrderSettings](#schemacolumnordersettings)|false|none|none|
|»»»» order|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» columnSortSettings|[ColumnSortSettings](#schemacolumnsortsettings)|false|none|none|
|»»»» sort|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|»»» eventDetailsPanel|boolean|false|none|none|
|»»» eventTableFields|[string]|false|none|none|
|»»» rowNumberColumnWidth|number|false|none|none|
|»»» showColumnTotals|boolean|true|none|none|
|»»» showColumnTotalsPinned|boolean|true|none|none|
|»»» showRowNumbers|boolean|true|none|none|
|»»» showRowTotals|boolean|true|none|none|
|»»» showRowTotalsPinned|boolean|true|none|none|
|»»» wrapCells|boolean|true|none|none|
|»» targetEventTime|number|false|none|none|
|»» timeCompleted|number|false|none|none|
|»» timeCreated|number|true|none|none|
|»» timeStarted|number|false|none|none|
|»» timeToFirstByte|number|false|none|none|
|»» totalBytesScanned|number|false|none|none|
|»» totalEventCount|number|false|none|none|
|»» type|string|false|none|none|
|»» usageGroupId|string|false|none|none|
|»» usageMetrics|[SearchAuditMetrics](#schemasearchauditmetrics)|false|none|none|
|»»» bytesIn|number|true|none|none|
|»»» bytesOut|number|true|none|none|
|»»» eventsIn|number|true|none|none|
|»»» eventsOut|number|true|none|none|
|»»» objects|object|true|none|none|
|»»»» discovered|number|true|none|none|
|»»»» scanned|number|true|none|none|
|»»»» skipped|number|true|none|none|
|»»» tasks|object|false|none|none|
|»»»» largeFileTaskCount|number|true|none|none|
|»»»» standardTaskCount|number|true|none|none|
|»»» time|object|true|none|none|
|»»»» queuedSec|number|true|none|none|
|»»»» runningSec|number|true|none|none|
|»»»» taskCompletionTotalSec|number|true|none|none|
|»»»» taskReceivingTotalSec|number|true|none|none|
|»» user|string|true|none|none|

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
|lib|custom|
|lib|cribl-custom|
|type|regex|
|type|json|
|type|json_array|
|type|header|
|type|timestamp|
|type|csv|
|type|aws_cloudtrail|
|type|aws_vpcflow|
|type|auto|
|type|format|
|type|current|
|type|string|
|type|number|
|type|boolean|
|usedCache|true|
|usedCache|false|
|type|stage|
|type|stage-scalar|
|type|aggregate|
|type|dedup|
|type|distinct|
|type|extract|
|type|filter|
|type|limit|
|type|mv-expand|
|type|mv-pull|
|type|noop|
|type|pivot|
|type|project|
|type|sort|
|direction|ascending|
|direction|descending|
|nullPosition|nullsFirst|
|nullPosition|nullsLast|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|
|type|command|
|type|standard|
|type|scheduled|
|type|datatypePreview|
|type|dashboard|
|type|notebook|
|type|systemInsights|

<aside class="success">
This operation does not require authentication
</aside>

## internal endpoint, dispatch search *id* to worker nodes filtered by worker node filter using RPC

<a id="opIdcreateSearchJobDispatchExecutorsById"></a>

> Code samples

`POST /search/jobs/{id}/dispatch-executors`

internal endpoint, dispatch search *id* to worker nodes filtered by worker node filter using RPC

<h3 id="internal-endpoint,-dispatch-search-*id*-to-worker-nodes-filtered-by-worker-node-filter-using-rpc-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job id|

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

<h3 id="internal-endpoint,-dispatch-search-*id*-to-worker-nodes-filtered-by-worker-node-filter-using-rpc-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="internal-endpoint,-dispatch-search-*id*-to-worker-nodes-filtered-by-worker-node-filter-using-rpc-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## List search results, when lower/upper bound is provided, offset is relative to the time range.

<a id="opIdgetSearchJobsResultsById"></a>

> Code samples

`GET /search/jobs/{id}/results`

List search results, when lower/upper bound is provided, offset is relative to the time range.

<h3 id="list-search-results,-when-lower/upper-bound-is-provided,-offset-is-relative-to-the-time-range.-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job id|
|limit|query|number|false|maximum number of events returned|
|offset|query|number|false|starting offset of the events|
|lowerBound|query|number|false|lower bound of the time range, inclusive|
|upperBound|query|number|false|upper bound of the time range, exclusive|

> Example responses

> 200 Response

> 500 Response

```json
{
  "message": "string"
}
```

<h3 id="list-search-results,-when-lower/upper-bound-is-provided,-offset-is-relative-to-the-time-range.-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|SearchResultsResults for the Search /results and /results-poll endpoints. object|[SearchJobResults](#schemasearchjobresults)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## List search results

<a id="opIdgetSearchJobsResultsPollById"></a>

> Code samples

`GET /search/jobs/{id}/results-poll`

List search results

<h3 id="list-search-results-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job id|
|limit|query|number|false|maximum number of events returned|
|offset|query|number|false|starting offset of the events|
|lowerBound|query|number|false|lower bound of the time range, inclusive|
|upperBound|query|number|false|upper bound of the time range, exclusive|
|lastJobStatus|query|string|false|last known status of the Search Job. Used to return immediatelyupon status change if the status was queued.|

> Example responses

> 200 Response

> 500 Response

```json
{
  "message": "string"
}
```

<h3 id="list-search-results-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|SearchResultsResults for the Search /results and /results-poll endpoints. object|[SearchJobResults](#schemasearchjobresults)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## List search results for a given stage. Note that this cannot be the root stage!

<a id="opIdgetSearchJobsStagesResultsByIdAndStageId"></a>

> Code samples

`GET /search/jobs/{id}/stages/{stageId}/results`

List search results for a given stage. Note that this cannot be the root stage!

<h3 id="list-search-results-for-a-given-stage.-note-that-this-cannot-be-the-root-stage!-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job ID|
|stageId|path|string|true|id of the search job stage|

> Example responses

> 200 Response

```json
"string"
```

<h3 id="list-search-results-for-a-given-stage.-note-that-this-cannot-be-the-root-stage!-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|string object|string|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Runs the query and returns the results

<a id="opIdgetSearchQuery"></a>

> Code samples

`GET /search/query`

Runs the query and returns the results

<h3 id="runs-the-query-and-returns-the-results-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|queryId|query|string|false|Saved query ID|
|jobId|query|string|false|Job ID|
|query|query|string|false|Search query string|
|earliest|query|any|false|Beginning of query time range, inclusive, in a relative time format or seconds|
|latest|query|any|false|End of query time range, exclusive, in a relative time format or seconds|
|sampleRate|query|number|false|Number between 0-1 to sample events during search|
|force|query|boolean|false|When true, forces to run the scheduled query|
|offset|query|number|false|Pagination offset|
|limit|query|number|false|Pagination limit - maximum number of events to return|

> Example responses

> 200 Response

> 500 Response

```json
{
  "message": "string"
}
```

<h3 id="runs-the-query-and-returns-the-results-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|SearchResultsResults for the Search /results and /results-poll endpoints. object|[SearchJobResults](#schemasearchjobresults)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get search job settings

<a id="opIdgetSearchJobSettingsById"></a>

> Code samples

`GET /search/jobs/{id}/settings`

Get search job settings

<h3 id="get-search-job-settings-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job id|

> Example responses

> 200 Response

```json
{
  "reaperKeepUntil": 0
}
```

<h3 id="get-search-job-settings-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|SearchJobSettings object|[SearchJobSettings](#schemasearchjobsettings)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Save search job settings

<a id="opIdcreateSearchJobSettingsById"></a>

> Code samples

`POST /search/jobs/{id}/settings`

Save search job settings

<h3 id="save-search-job-settings-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job id|

> Example responses

> 200 Response

```json
{
  "reaperKeepUntil": 0
}
```

<h3 id="save-search-job-settings-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|SearchJobSettings object|[SearchJobSettings](#schemasearchjobsettings)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get job status

<a id="opIdgetSearchJobStatusById"></a>

> Code samples

`GET /search/jobs/{id}/status`

Get job status

<h3 id="get-job-status-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job id|

> Example responses

> 200 Response

```json
{
  "bytesIn": 0,
  "bytesSkipped": 0,
  "cacheStatusesByStageId": {
    "property1": {
      "property1": {
        "usedCache": true
      },
      "property2": {
        "usedCache": true
      }
    },
    "property2": {
      "property1": {
        "usedCache": true
      },
      "property2": {
        "usedCache": true
      }
    }
  },
  "eventsFound": 0,
  "eventsIn": 0,
  "eventsSkipped": 0,
  "objectsFound": 0,
  "objectsSearched": 0,
  "objectsSkipped": 0,
  "stageDetails": [
    {
      "cacheStatusByDatasetId": {
        "property1": {
          "usedCache": true
        },
        "property2": {
          "usedCache": true
        }
      },
      "stageId": "string",
      "status": "failed"
    }
  ],
  "status": "failed",
  "timeCompleted": 0,
  "timeCreated": 0,
  "timeNow": 0,
  "timeStarted": 0
}
```

<h3 id="get-job-status-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|StatusResponse object|[StatusResponse](#schemastatusresponse)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get search timeline

<a id="opIdgetSearchJobTimelineById"></a>

> Code samples

`GET /search/jobs/{id}/timeline`

Get search timeline

<h3 id="get-search-timeline-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job id|

> Example responses

> 200 Response

```json
{
  "buckets": [
    {
      "duration": 0,
      "earliest": 0,
      "eventCount": 0
    }
  ],
  "totalEventCount": 0
}
```

<h3 id="get-search-timeline-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|SearchTimeline object|[SearchTimeline](#schemasearchtimeline)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## List field summaries

<a id="opIdgetSearchJobFieldSummariesById"></a>

> Code samples

`GET /search/jobs/{id}/field-summaries`

List field summaries

<h3 id="list-field-summaries-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|search job id|

> Example responses

> 200 Response

```json
{
  "fields": [
    {
      "buckets": [
        {
          "duration": 0,
          "earliest": 0,
          "eventCount": 0
        }
      ],
      "count": 0,
      "countDistinct": 0,
      "countNull": 0,
      "max": "string",
      "mean": 0,
      "min": "string",
      "name": "string",
      "stdev": 0,
      "topValues": [
        {
          "count": 0,
          "value": "string"
        }
      ],
      "type": "array"
    }
  ],
  "partial": true,
  "totalEventCount": 0
}
```

<h3 id="list-field-summaries-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|FieldSummaries object|[FieldSummaries](#schemafieldsummaries)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_SearchJob">SearchJob</h2>
<!-- backwards compatibility -->
<a id="schemasearchjob"></a>
<a id="schema_SearchJob"></a>
<a id="tocSsearchjob"></a>
<a id="tocssearchjob"></a>

```json
{
  "accelerated": true,
  "aliasOfOriginalJobId": "string",
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
  "compatibilityChecks": {
    "datatypes": true,
    "stageIds": [
      "string"
    ]
  },
  "completionInfo": "string",
  "context": "string",
  "correlationId": "string",
  "cpuMetrics": {
    "billableCPUSeconds": 0,
    "executorsCPUSeconds": {
      "property1": 0,
      "property2": 0
    },
    "totalCPUSeconds": 0,
    "totalExecCPUSeconds": 0
  },
  "datatypeOverrides": {
    "breakerRulesets": [
      {
        "id": "string",
        "lib": "custom",
        "description": "string",
        "tags": "string",
        "minRawLength": 256,
        "rules": [
          {
            "name": "string",
            "condition": "true",
            "type": "regex",
            "timestampAnchorRegex": "/^/",
            "timestamp": {
              "type": "auto",
              "length": 150,
              "format": "string"
            },
            "timestampTimezone": "local",
            "timestampEarliest": "-420weeks",
            "timestampLatest": "+1week",
            "maxEventBytes": 51200,
            "fields": [
              {
                "name": "string",
                "value": "string"
              }
            ],
            "disabled": false,
            "parserEnabled": false,
            "shouldUseDataRaw": false
          }
        ]
      }
    ],
    "disableBreakers": true
  },
  "disableNotifications": true,
  "displayUsername": "string",
  "earliest": "string",
  "earliestEpoch": 0,
  "errorStateConfig": {
    "coordinated": true,
    "errorMessages": [
      "string"
    ]
  },
  "group": "string",
  "id": "string",
  "isPrivate": true,
  "latest": "string",
  "latestEpoch": 0,
  "metadata": {
    "arguments": {
      "property1": [
        {
          "property1": "string",
          "property2": "string"
        }
      ],
      "property2": [
        {
          "property1": "string",
          "property2": "string"
        }
      ]
    },
    "computeTypes": {
      "v1": 0,
      "v2": 0,
      "lakehouse": 0
    },
    "datasets": {
      "property1": 0,
      "property2": 0
    },
    "functions": {
      "property1": 0,
      "property2": 0
    },
    "operators": {
      "property1": 0,
      "property2": 0
    },
    "providerTypes": {
      "property1": 0,
      "property2": 0
    },
    "providers": {
      "property1": 0,
      "property2": 0
    }
  },
  "notebookId": "string",
  "numEventsAfter": 0,
  "numEventsBefore": 0,
  "query": "string",
  "queryWithMacrosResolved": "string",
  "sampleRate": 0,
  "savedQueryName": "string",
  "searchParameterDeclarations": [
    {
      "defaultValue": "string",
      "name": "string",
      "type": "string"
    }
  ],
  "searchParameterValues": {
    "property1": "string",
    "property2": "string"
  },
  "setOptions": {
    "property1": "string",
    "property2": "string"
  },
  "stages": [
    {
      "cacheStatusByDatasetId": {
        "property1": {
          "usedCache": true
        },
        "property2": {
          "usedCache": true
        }
      },
      "dependencies": [
        {
          "dependentFields": [
            "string"
          ],
          "id": "string",
          "type": "stage"
        }
      ],
      "earliest": "string",
      "executionWarnings": [
        {
          "text": "string",
          "type": "string"
        }
      ],
      "filter": "string",
      "id": "string",
      "latest": "string",
      "resolvedDatasetIds": [
        "string"
      ],
      "searchConfig": {
        "canComputeMetadataDistributively": true,
        "datasets": [
          "string"
        ],
        "hasSendOperator": true,
        "logicalPlans": {
          "Combined": {
            "property1": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ],
            "property2": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ]
          },
          "Coordinated": {
            "property1": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ],
            "property2": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ]
          },
          "Federated": {
            "property1": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ],
            "property2": [
              {
                "isPreviewableOperation": true,
                "type": "aggregate"
              }
            ]
          }
        },
        "orderedFieldNames": [
          "string"
        ],
        "pipelines": {
          "Combined": {
            "id": "string",
            "conf": {
              "asyncFuncTimeout": 10000,
              "output": "default",
              "description": "string",
              "streamtags": [],
              "functions": [
                {
                  "filter": "true",
                  "id": "string",
                  "description": "string",
                  "disabled": true,
                  "final": true,
                  "conf": {},
                  "groupId": "string"
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
              }
            }
          },
          "Coordinated": {
            "id": "string",
            "conf": {
              "asyncFuncTimeout": 10000,
              "output": "default",
              "description": "string",
              "streamtags": [],
              "functions": [
                {
                  "filter": "true",
                  "id": "string",
                  "description": "string",
                  "disabled": true,
                  "final": true,
                  "conf": {},
                  "groupId": "string"
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
              }
            }
          },
          "Federated": {
            "id": "string",
            "conf": {
              "asyncFuncTimeout": 10000,
              "output": "default",
              "description": "string",
              "streamtags": [],
              "functions": [
                {
                  "filter": "true",
                  "id": "string",
                  "description": "string",
                  "disabled": true,
                  "final": true,
                  "conf": {},
                  "groupId": "string"
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
              }
            }
          }
        },
        "referencedColumnPaths": [
          [
            "string"
          ]
        ],
        "searchTerms": [
          {
            "isCaseSensitive": true,
            "term": "string"
          }
        ],
        "sortFields": [
          {
            "direction": "ascending",
            "fieldName": "string",
            "nullPosition": "nullsFirst"
          }
        ],
        "useFormattedVisualization": true
      },
      "status": "failed",
      "subQueryText": "string"
    }
  ],
  "status": "failed",
  "tableConfig": {
    "columnFilterSettings": {
      "contains": {}
    },
    "columnFormatSettings": {
      "palette": {},
      "precision": {},
      "prefix": {},
      "suffix": {}
    },
    "columnOrderSettings": {
      "order": {}
    },
    "columnSortSettings": {
      "sort": {}
    },
    "eventDetailsPanel": true,
    "eventTableFields": [
      "string"
    ],
    "rowNumberColumnWidth": 0,
    "showColumnTotals": true,
    "showColumnTotalsPinned": true,
    "showRowNumbers": true,
    "showRowTotals": true,
    "showRowTotalsPinned": true,
    "wrapCells": true
  },
  "targetEventTime": 0,
  "timeCompleted": 0,
  "timeCreated": 0,
  "timeStarted": 0,
  "timeToFirstByte": 0,
  "totalBytesScanned": 0,
  "totalEventCount": 0,
  "type": "command",
  "usageGroupId": "string",
  "usageMetrics": {
    "bytesIn": 0,
    "bytesOut": 0,
    "eventsIn": 0,
    "eventsOut": 0,
    "objects": {
      "discovered": 0,
      "scanned": 0,
      "skipped": 0
    },
    "tasks": {
      "largeFileTaskCount": 0,
      "standardTaskCount": 0
    },
    "time": {
      "queuedSec": 0,
      "runningSec": 0,
      "taskCompletionTotalSec": 0,
      "taskReceivingTotalSec": 0
    }
  },
  "user": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|accelerated|boolean|false|none|none|
|aliasOfOriginalJobId|string|false|none|none|
|chartConfig|object|false|none|none|
|» applyThreshold|boolean|false|none|none|
|» axis|object|false|none|none|
|»» xAxis|string|false|none|none|
|»» yAxis|[string]|false|none|none|
|»» yAxisExcluded|[string]|false|none|none|
|» color|string|false|none|none|
|» colorPalette|number|false|none|none|
|» colorPaletteReversed|boolean|false|none|none|
|» colorThresholds|object|false|none|none|
|»» thresholds|[object]|true|none|none|
|»»» color|string|true|none|none|
|»»» threshold|number|true|none|none|
|» customData|object|false|none|none|
|»» connectNulls|string|false|none|none|
|»» dataFields|[string]|false|none|none|
|»» isPointColor|boolean|false|none|none|
|»» limitToTopN|number|false|none|none|
|»» lines|boolean|false|none|none|
|»» nameField|string|false|none|none|
|»» pointColorPalette|number|false|none|none|
|»» pointColorPaletteReversed|boolean|false|none|none|
|»» pointScale|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» pointScaleDataField|string|false|none|none|
|»» seriesCount|number|false|none|none|
|»» splitBy|string|false|none|none|
|»» stack|boolean|false|none|none|
|»» summarizeOthers|boolean|false|none|none|
|»» trellis|boolean|false|none|none|
|» decimals|number|false|none|none|
|» label|string|false|none|none|
|» legend|object|false|none|none|
|»» position|string|false|none|none|
|»» selected|object|false|none|none|
|»»» **additionalProperties**|boolean|false|none|none|
|»» truncate|boolean|false|none|none|
|» mapDetails|object|false|none|none|
|»» latitudeField|string|false|none|none|
|»» longitudeField|string|false|none|none|
|»» mapSourceID|string|false|none|none|
|»» mapType|string|false|none|none|
|»» nameField|string|false|none|none|
|»» pointScale|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» valueField|string|false|none|none|
|» onClickAction|object|false|none|none|
|»» search|string|false|none|none|
|»» selectedDashboardId|string|false|none|none|
|»» selectedInputId|string|false|none|none|
|»» selectedLinkId|string|false|none|none|
|»» selectedTimerangeInputId|string|false|none|none|
|»» type|string|false|none|none|
|» prefix|string|false|none|none|
|» separator|boolean|false|none|none|
|» series|[[ChartSeries](#schemachartseries)]|false|none|none|
|» seriesInfo|object|false|none|none|
|»» **additionalProperties**|[ChartType](#schemacharttype)|false|none|none|
|» shouldApplyUserChartSettings|boolean|false|none|none|
|» style|boolean|false|none|none|
|» suffix|string|false|none|none|
|» type|string|false|none|none|
|» xAxis|object|false|none|none|
|»» dataField|string|false|none|none|
|»» inverse|boolean|false|none|none|
|»» labelInterval|string|false|none|none|
|»» labelOrientation|number|false|none|none|
|»» name|string|false|none|none|
|»» offset|number|false|none|none|
|»» position|string|false|none|none|
|»» type|string|false|none|none|
|» yAxis|object|false|none|none|
|»» dataField|[string]|false|none|none|
|»» interval|number|false|none|none|
|»» max|number|false|none|none|
|»» min|number|false|none|none|
|»» position|string|false|none|none|
|»» scale|string|false|none|none|
|»» splitLine|boolean|false|none|none|
|»» type|string|false|none|none|
|compatibilityChecks|object|false|none|none|
|» datatypes|boolean|false|none|none|
|» stageIds|[string]|false|none|none|
|completionInfo|string|false|none|none|
|context|string|false|none|none|
|correlationId|string|false|none|none|
|cpuMetrics|[CPUTimeMetric](#schemacputimemetric)|false|none|none|
|datatypeOverrides|[DatatypeOverrides](#schemadatatypeoverrides)|false|none|none|
|disableNotifications|boolean|false|none|none|
|displayUsername|string|true|none|none|
|earliest|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|earliestEpoch|number|false|none|none|
|errorStateConfig|[SearchJobErrorStateConfig](#schemasearchjoberrorstateconfig)|false|none|none|
|group|string|true|none|none|
|id|string|true|none|none|
|isPrivate|boolean|false|none|none|
|latest|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|latestEpoch|number|false|none|none|
|metadata|[SearchJobMetadata](#schemasearchjobmetadata)|false|none|none|
|notebookId|string|false|none|none|
|numEventsAfter|number|false|none|none|
|numEventsBefore|number|false|none|none|
|query|string|true|none|none|
|queryWithMacrosResolved|string|false|none|none|
|sampleRate|number|false|none|none|
|savedQueryName|string|false|none|none|
|searchParameterDeclarations|[[SearchParameter](#schemasearchparameter)]|false|none|none|
|searchParameterValues|[SearchParameters](#schemasearchparameters)|false|none|none|
|setOptions|object|false|none|none|
|» **additionalProperties**|any|false|none|none|

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
|stages|[[SearchJobStageConfig](#schemasearchjobstageconfig)]|false|none|none|
|status|string|true|none|none|
|tableConfig|[TableViewSettings](#schematableviewsettings)|false|none|none|
|targetEventTime|number|false|none|none|
|timeCompleted|number|false|none|none|
|timeCreated|number|true|none|none|
|timeStarted|number|false|none|none|
|timeToFirstByte|number|false|none|none|
|totalBytesScanned|number|false|none|none|
|totalEventCount|number|false|none|none|
|type|string|false|none|none|
|usageGroupId|string|false|none|none|
|usageMetrics|[SearchAuditMetrics](#schemasearchauditmetrics)|false|none|none|
|user|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|
|type|command|
|type|standard|
|type|scheduled|
|type|datatypePreview|
|type|dashboard|
|type|notebook|
|type|systemInsights|

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

<h2 id="tocS_SearchJobResults">SearchJobResults</h2>
<!-- backwards compatibility -->
<a id="schemasearchjobresults"></a>
<a id="schema_SearchJobResults"></a>
<a id="tocSsearchjobresults"></a>
<a id="tocssearchjobresults"></a>

```json
{
  "isFinished": true,
  "job": {
    "id": "string",
    "query": "string",
    "earliest": "string",
    "latest": "string",
    "timeCreated": 0,
    "timeStarted": 0,
    "timeCompleted": 0,
    "status": "failed"
  },
  "limit": 0,
  "offset": 0,
  "persistedEventCount": 0,
  "totalEventCount": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|isFinished|boolean|true|none|none|
|job|[SearchJobResultsJobInfo](#schemasearchjobresultsjobinfo)|true|none|none|
|limit|number|false|none|none|
|offset|number|true|none|none|
|persistedEventCount|number|true|none|none|
|totalEventCount|number|true|none|none|

<h2 id="tocS_SearchJobSettings">SearchJobSettings</h2>
<!-- backwards compatibility -->
<a id="schemasearchjobsettings"></a>
<a id="schema_SearchJobSettings"></a>
<a id="tocSsearchjobsettings"></a>
<a id="tocssearchjobsettings"></a>

```json
{
  "reaperKeepUntil": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|reaperKeepUntil|number|false|none|none|

<h2 id="tocS_StatusResponse">StatusResponse</h2>
<!-- backwards compatibility -->
<a id="schemastatusresponse"></a>
<a id="schema_StatusResponse"></a>
<a id="tocSstatusresponse"></a>
<a id="tocsstatusresponse"></a>

```json
{
  "bytesIn": 0,
  "bytesSkipped": 0,
  "cacheStatusesByStageId": {
    "property1": {
      "property1": {
        "usedCache": true
      },
      "property2": {
        "usedCache": true
      }
    },
    "property2": {
      "property1": {
        "usedCache": true
      },
      "property2": {
        "usedCache": true
      }
    }
  },
  "eventsFound": 0,
  "eventsIn": 0,
  "eventsSkipped": 0,
  "objectsFound": 0,
  "objectsSearched": 0,
  "objectsSkipped": 0,
  "stageDetails": [
    {
      "cacheStatusByDatasetId": {
        "property1": {
          "usedCache": true
        },
        "property2": {
          "usedCache": true
        }
      },
      "stageId": "string",
      "status": "failed"
    }
  ],
  "status": "failed",
  "timeCompleted": 0,
  "timeCreated": 0,
  "timeNow": 0,
  "timeStarted": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|bytesIn|number|false|none|none|
|bytesSkipped|number|false|none|none|
|cacheStatusesByStageId|object|false|none|none|
|» **additionalProperties**|object|false|none|none|
|»» **additionalProperties**|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» usedCache|boolean|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» reason|string|true|none|none|
|»»»» usedCache|boolean|true|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|eventsFound|number|false|none|none|
|eventsIn|number|false|none|none|
|eventsSkipped|number|false|none|none|
|objectsFound|number|false|none|none|
|objectsSearched|number|false|none|none|
|objectsSkipped|number|false|none|none|
|stageDetails|[object]|false|none|none|
|» cacheStatusByDatasetId|[CacheStatusByDatasetId](#schemacachestatusbydatasetid)|true|none|none|
|» stageId|string|true|none|none|
|» status|string|true|none|none|
|status|string|true|none|none|
|timeCompleted|number|false|none|none|
|timeCreated|number|true|none|none|
|timeNow|number|false|none|none|
|timeStarted|number|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|usedCache|true|
|usedCache|false|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|

<h2 id="tocS_SearchTimeline">SearchTimeline</h2>
<!-- backwards compatibility -->
<a id="schemasearchtimeline"></a>
<a id="schema_SearchTimeline"></a>
<a id="tocSsearchtimeline"></a>
<a id="tocssearchtimeline"></a>

```json
{
  "buckets": [
    {
      "duration": 0,
      "earliest": 0,
      "eventCount": 0
    }
  ],
  "totalEventCount": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|buckets|[[Bucket](#schemabucket)]|true|none|none|
|totalEventCount|number|true|none|none|

<h2 id="tocS_FieldSummaries">FieldSummaries</h2>
<!-- backwards compatibility -->
<a id="schemafieldsummaries"></a>
<a id="schema_FieldSummaries"></a>
<a id="tocSfieldsummaries"></a>
<a id="tocsfieldsummaries"></a>

```json
{
  "fields": [
    {
      "buckets": [
        {
          "duration": 0,
          "earliest": 0,
          "eventCount": 0
        }
      ],
      "count": 0,
      "countDistinct": 0,
      "countNull": 0,
      "max": "string",
      "mean": 0,
      "min": "string",
      "name": "string",
      "stdev": 0,
      "topValues": [
        {
          "count": 0,
          "value": "string"
        }
      ],
      "type": "array"
    }
  ],
  "partial": true,
  "totalEventCount": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|fields|[[Field](#schemafield)]|true|none|none|
|partial|boolean|false|none|none|
|totalEventCount|number|true|none|none|

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

<h2 id="tocS_CPUTimeMetric">CPUTimeMetric</h2>
<!-- backwards compatibility -->
<a id="schemacputimemetric"></a>
<a id="schema_CPUTimeMetric"></a>
<a id="tocScputimemetric"></a>
<a id="tocscputimemetric"></a>

```json
{
  "billableCPUSeconds": 0,
  "executorsCPUSeconds": {
    "property1": 0,
    "property2": 0
  },
  "totalCPUSeconds": 0,
  "totalExecCPUSeconds": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|billableCPUSeconds|number|false|none|none|
|executorsCPUSeconds|object|true|none|none|
|» **additionalProperties**|number|false|none|none|
|totalCPUSeconds|number|true|none|none|
|totalExecCPUSeconds|number|true|none|none|

<h2 id="tocS_DatatypeOverrides">DatatypeOverrides</h2>
<!-- backwards compatibility -->
<a id="schemadatatypeoverrides"></a>
<a id="schema_DatatypeOverrides"></a>
<a id="tocSdatatypeoverrides"></a>
<a id="tocsdatatypeoverrides"></a>

```json
{
  "breakerRulesets": [
    {
      "id": "string",
      "lib": "custom",
      "description": "string",
      "tags": "string",
      "minRawLength": 256,
      "rules": [
        {
          "name": "string",
          "condition": "true",
          "type": "regex",
          "timestampAnchorRegex": "/^/",
          "timestamp": {
            "type": "auto",
            "length": 150,
            "format": "string"
          },
          "timestampTimezone": "local",
          "timestampEarliest": "-420weeks",
          "timestampLatest": "+1week",
          "maxEventBytes": 51200,
          "fields": [
            {
              "name": "string",
              "value": "string"
            }
          ],
          "disabled": false,
          "parserEnabled": false,
          "shouldUseDataRaw": false
        }
      ]
    }
  ],
  "disableBreakers": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|breakerRulesets|[[EventBreakerRuleset](#schemaeventbreakerruleset)]|false|none|none|
|disableBreakers|boolean|true|none|none|

<h2 id="tocS_SearchJobErrorStateConfig">SearchJobErrorStateConfig</h2>
<!-- backwards compatibility -->
<a id="schemasearchjoberrorstateconfig"></a>
<a id="schema_SearchJobErrorStateConfig"></a>
<a id="tocSsearchjoberrorstateconfig"></a>
<a id="tocssearchjoberrorstateconfig"></a>

```json
{
  "coordinated": true,
  "errorMessages": [
    "string"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|coordinated|boolean|true|none|none|
|errorMessages|[string]|true|none|none|

<h2 id="tocS_SearchJobMetadata">SearchJobMetadata</h2>
<!-- backwards compatibility -->
<a id="schemasearchjobmetadata"></a>
<a id="schema_SearchJobMetadata"></a>
<a id="tocSsearchjobmetadata"></a>
<a id="tocssearchjobmetadata"></a>

```json
{
  "arguments": {
    "property1": [
      {
        "property1": "string",
        "property2": "string"
      }
    ],
    "property2": [
      {
        "property1": "string",
        "property2": "string"
      }
    ]
  },
  "computeTypes": {
    "v1": 0,
    "v2": 0,
    "lakehouse": 0
  },
  "datasets": {
    "property1": 0,
    "property2": 0
  },
  "functions": {
    "property1": 0,
    "property2": 0
  },
  "operators": {
    "property1": 0,
    "property2": 0
  },
  "providerTypes": {
    "property1": 0,
    "property2": 0
  },
  "providers": {
    "property1": 0,
    "property2": 0
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|arguments|object|false|none|none|
|» **additionalProperties**|[object]|false|none|none|
|»» **additionalProperties**|string|false|none|none|
|computeTypes|object|false|none|none|
|» v1|number|false|none|none|
|» v2|number|false|none|none|
|» lakehouse|number|false|none|none|
|datasets|object|false|none|none|
|» **additionalProperties**|number|false|none|none|
|functions|object|false|none|none|
|» **additionalProperties**|number|false|none|none|
|operators|object|false|none|none|
|» **additionalProperties**|number|false|none|none|
|providerTypes|object|false|none|none|
|» **additionalProperties**|number|false|none|none|
|providers|object|false|none|none|
|» **additionalProperties**|number|false|none|none|

<h2 id="tocS_SearchParameter">SearchParameter</h2>
<!-- backwards compatibility -->
<a id="schemasearchparameter"></a>
<a id="schema_SearchParameter"></a>
<a id="tocSsearchparameter"></a>
<a id="tocssearchparameter"></a>

```json
{
  "defaultValue": "string",
  "name": "string",
  "type": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|defaultValue|[SearchParameterValue](#schemasearchparametervalue)|false|none|none|
|name|string|true|none|none|
|type|[SearchParameterType](#schemasearchparametertype)|true|none|none|

<h2 id="tocS_SearchParameters">SearchParameters</h2>
<!-- backwards compatibility -->
<a id="schemasearchparameters"></a>
<a id="schema_SearchParameters"></a>
<a id="tocSsearchparameters"></a>
<a id="tocssearchparameters"></a>

```json
{
  "property1": "string",
  "property2": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|**additionalProperties**|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|boolean|false|none|none|

<h2 id="tocS_SearchJobStageConfig">SearchJobStageConfig</h2>
<!-- backwards compatibility -->
<a id="schemasearchjobstageconfig"></a>
<a id="schema_SearchJobStageConfig"></a>
<a id="tocSsearchjobstageconfig"></a>
<a id="tocssearchjobstageconfig"></a>

```json
{
  "cacheStatusByDatasetId": {
    "property1": {
      "usedCache": true
    },
    "property2": {
      "usedCache": true
    }
  },
  "dependencies": [
    {
      "dependentFields": [
        "string"
      ],
      "id": "string",
      "type": "stage"
    }
  ],
  "earliest": "string",
  "executionWarnings": [
    {
      "text": "string",
      "type": "string"
    }
  ],
  "filter": "string",
  "id": "string",
  "latest": "string",
  "resolvedDatasetIds": [
    "string"
  ],
  "searchConfig": {
    "canComputeMetadataDistributively": true,
    "datasets": [
      "string"
    ],
    "hasSendOperator": true,
    "logicalPlans": {
      "Combined": {
        "property1": [
          {
            "isPreviewableOperation": true,
            "type": "aggregate"
          }
        ],
        "property2": [
          {
            "isPreviewableOperation": true,
            "type": "aggregate"
          }
        ]
      },
      "Coordinated": {
        "property1": [
          {
            "isPreviewableOperation": true,
            "type": "aggregate"
          }
        ],
        "property2": [
          {
            "isPreviewableOperation": true,
            "type": "aggregate"
          }
        ]
      },
      "Federated": {
        "property1": [
          {
            "isPreviewableOperation": true,
            "type": "aggregate"
          }
        ],
        "property2": [
          {
            "isPreviewableOperation": true,
            "type": "aggregate"
          }
        ]
      }
    },
    "orderedFieldNames": [
      "string"
    ],
    "pipelines": {
      "Combined": {
        "id": "string",
        "conf": {
          "asyncFuncTimeout": 10000,
          "output": "default",
          "description": "string",
          "streamtags": [],
          "functions": [
            {
              "filter": "true",
              "id": "string",
              "description": "string",
              "disabled": true,
              "final": true,
              "conf": {},
              "groupId": "string"
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
          }
        }
      },
      "Coordinated": {
        "id": "string",
        "conf": {
          "asyncFuncTimeout": 10000,
          "output": "default",
          "description": "string",
          "streamtags": [],
          "functions": [
            {
              "filter": "true",
              "id": "string",
              "description": "string",
              "disabled": true,
              "final": true,
              "conf": {},
              "groupId": "string"
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
          }
        }
      },
      "Federated": {
        "id": "string",
        "conf": {
          "asyncFuncTimeout": 10000,
          "output": "default",
          "description": "string",
          "streamtags": [],
          "functions": [
            {
              "filter": "true",
              "id": "string",
              "description": "string",
              "disabled": true,
              "final": true,
              "conf": {},
              "groupId": "string"
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
          }
        }
      }
    },
    "referencedColumnPaths": [
      [
        "string"
      ]
    ],
    "searchTerms": [
      {
        "isCaseSensitive": true,
        "term": "string"
      }
    ],
    "sortFields": [
      {
        "direction": "ascending",
        "fieldName": "string",
        "nullPosition": "nullsFirst"
      }
    ],
    "useFormattedVisualization": true
  },
  "status": "failed",
  "subQueryText": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cacheStatusByDatasetId|[CacheStatusByDatasetId](#schemacachestatusbydatasetid)|false|none|none|
|dependencies|[[StageDependency](#schemastagedependency)]|true|none|none|
|earliest|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|executionWarnings|[[JobExecutionWarning](#schemajobexecutionwarning)]|false|none|none|
|filter|string|true|none|none|
|id|string|true|none|none|
|latest|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|resolvedDatasetIds|[string]|true|none|none|
|searchConfig|[SearchConfig](#schemasearchconfig)|true|none|none|
|status|string|true|none|none|
|subQueryText|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|

<h2 id="tocS_TableViewSettings">TableViewSettings</h2>
<!-- backwards compatibility -->
<a id="schematableviewsettings"></a>
<a id="schema_TableViewSettings"></a>
<a id="tocStableviewsettings"></a>
<a id="tocstableviewsettings"></a>

```json
{
  "columnFilterSettings": {
    "contains": {}
  },
  "columnFormatSettings": {
    "palette": {},
    "precision": {},
    "prefix": {},
    "suffix": {}
  },
  "columnOrderSettings": {
    "order": {}
  },
  "columnSortSettings": {
    "sort": {}
  },
  "eventDetailsPanel": true,
  "eventTableFields": [
    "string"
  ],
  "rowNumberColumnWidth": 0,
  "showColumnTotals": true,
  "showColumnTotalsPinned": true,
  "showRowNumbers": true,
  "showRowTotals": true,
  "showRowTotalsPinned": true,
  "wrapCells": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|columnFilterSettings|[ColumnFilterSettings](#schemacolumnfiltersettings)|false|none|none|
|columnFormatSettings|[ColumnFormatSettings](#schemacolumnformatsettings)|false|none|none|
|columnOrderSettings|[ColumnOrderSettings](#schemacolumnordersettings)|false|none|none|
|columnSortSettings|[ColumnSortSettings](#schemacolumnsortsettings)|false|none|none|
|eventDetailsPanel|boolean|false|none|none|
|eventTableFields|[string]|false|none|none|
|rowNumberColumnWidth|number|false|none|none|
|showColumnTotals|boolean|true|none|none|
|showColumnTotalsPinned|boolean|true|none|none|
|showRowNumbers|boolean|true|none|none|
|showRowTotals|boolean|true|none|none|
|showRowTotalsPinned|boolean|true|none|none|
|wrapCells|boolean|true|none|none|

<h2 id="tocS_SearchAuditMetrics">SearchAuditMetrics</h2>
<!-- backwards compatibility -->
<a id="schemasearchauditmetrics"></a>
<a id="schema_SearchAuditMetrics"></a>
<a id="tocSsearchauditmetrics"></a>
<a id="tocssearchauditmetrics"></a>

```json
{
  "bytesIn": 0,
  "bytesOut": 0,
  "eventsIn": 0,
  "eventsOut": 0,
  "objects": {
    "discovered": 0,
    "scanned": 0,
    "skipped": 0
  },
  "tasks": {
    "largeFileTaskCount": 0,
    "standardTaskCount": 0
  },
  "time": {
    "queuedSec": 0,
    "runningSec": 0,
    "taskCompletionTotalSec": 0,
    "taskReceivingTotalSec": 0
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|bytesIn|number|true|none|none|
|bytesOut|number|true|none|none|
|eventsIn|number|true|none|none|
|eventsOut|number|true|none|none|
|objects|object|true|none|none|
|» discovered|number|true|none|none|
|» scanned|number|true|none|none|
|» skipped|number|true|none|none|
|tasks|object|false|none|none|
|» largeFileTaskCount|number|true|none|none|
|» standardTaskCount|number|true|none|none|
|time|object|true|none|none|
|» queuedSec|number|true|none|none|
|» runningSec|number|true|none|none|
|» taskCompletionTotalSec|number|true|none|none|
|» taskReceivingTotalSec|number|true|none|none|

<h2 id="tocS_SearchJobResultsJobInfo">SearchJobResultsJobInfo</h2>
<!-- backwards compatibility -->
<a id="schemasearchjobresultsjobinfo"></a>
<a id="schema_SearchJobResultsJobInfo"></a>
<a id="tocSsearchjobresultsjobinfo"></a>
<a id="tocssearchjobresultsjobinfo"></a>

```json
{
  "id": "string",
  "query": "string",
  "earliest": "string",
  "latest": "string",
  "timeCreated": 0,
  "timeStarted": 0,
  "timeCompleted": 0,
  "status": "failed"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|query|string|true|none|none|
|earliest|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|latest|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|timeCreated|number|true|none|none|
|timeStarted|number|false|none|none|
|timeCompleted|number|false|none|none|
|status|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|failed|
|status|new|
|status|running|
|status|completed|
|status|canceled|
|status|queued|
|status|finalizing|

<h2 id="tocS_CacheStatusByDatasetId">CacheStatusByDatasetId</h2>
<!-- backwards compatibility -->
<a id="schemacachestatusbydatasetid"></a>
<a id="schema_CacheStatusByDatasetId"></a>
<a id="tocScachestatusbydatasetid"></a>
<a id="tocscachestatusbydatasetid"></a>

```json
{
  "property1": {
    "usedCache": true
  },
  "property2": {
    "usedCache": true
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|**additionalProperties**|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» usedCache|boolean|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» reason|string|true|none|none|
|»» usedCache|boolean|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|usedCache|true|
|usedCache|false|

<h2 id="tocS_Bucket">Bucket</h2>
<!-- backwards compatibility -->
<a id="schemabucket"></a>
<a id="schema_Bucket"></a>
<a id="tocSbucket"></a>
<a id="tocsbucket"></a>

```json
{
  "duration": 0,
  "earliest": 0,
  "eventCount": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|duration|number|true|none|none|
|earliest|number|true|none|none|
|eventCount|number|true|none|none|

<h2 id="tocS_Field">Field</h2>
<!-- backwards compatibility -->
<a id="schemafield"></a>
<a id="schema_Field"></a>
<a id="tocSfield"></a>
<a id="tocsfield"></a>

```json
{
  "buckets": [
    {
      "duration": 0,
      "earliest": 0,
      "eventCount": 0
    }
  ],
  "count": 0,
  "countDistinct": 0,
  "countNull": 0,
  "max": "string",
  "mean": 0,
  "min": "string",
  "name": "string",
  "stdev": 0,
  "topValues": [
    {
      "count": 0,
      "value": "string"
    }
  ],
  "type": "array"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|buckets|[[Bucket](#schemabucket)]|true|none|none|
|count|number|true|none|none|
|countDistinct|number|true|none|none|
|countNull|number|true|none|none|
|max|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|mean|number|false|none|none|
|min|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|true|none|none|
|stdev|number|false|none|none|
|topValues|[[TopValue](#schematopvalue)]|true|none|none|
|type|[FieldType](#schemafieldtype)|true|none|none|

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

<h2 id="tocS_EventBreakerRuleset">EventBreakerRuleset</h2>
<!-- backwards compatibility -->
<a id="schemaeventbreakerruleset"></a>
<a id="schema_EventBreakerRuleset"></a>
<a id="tocSeventbreakerruleset"></a>
<a id="tocseventbreakerruleset"></a>

```json
{
  "id": "string",
  "lib": "custom",
  "description": "string",
  "tags": "string",
  "minRawLength": 256,
  "rules": [
    {
      "name": "string",
      "condition": "true",
      "type": "regex",
      "timestampAnchorRegex": "/^/",
      "timestamp": {
        "type": "auto",
        "length": 150,
        "format": "string"
      },
      "timestampTimezone": "local",
      "timestampEarliest": "-420weeks",
      "timestampLatest": "+1week",
      "maxEventBytes": 51200,
      "fields": [
        {
          "name": "string",
          "value": "string"
        }
      ],
      "disabled": false,
      "parserEnabled": false,
      "shouldUseDataRaw": false
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|lib|string|false|none|none|
|description|string|false|none|none|
|tags|string|false|none|none|
|minRawLength|number|false|none|The  minimum number of characters in _raw to determine which rule to use|
|rules|[object]|false|none|A list of rules that will be applied, in order, to the input data stream|
|» name|string|true|none|none|
|» condition|string|true|none|JavaScript expression applied to the beginning of a file or object, to determine whether the rule applies to all contained events.|
|» type|string|true|none|none|
|» timestampAnchorRegex|string|true|none|The regex to match before attempting timestamp extraction. Use $ (end-of-string anchor) to prevent extraction.|
|» timestamp|object|true|none|Auto, manual format (strptime), or current time|
|»» type|string|true|none|none|
|»» length|number|false|none|none|
|»» format|string|false|none|none|
|» timestampTimezone|string|false|none|Timezone to assign to timestamps without timezone info|
|» timestampEarliest|string|false|none|The earliest timestamp value allowed relative to now. Example: -42years. Parsed values prior to this date will be set to current time.|
|» timestampLatest|string|false|none|The latest timestamp value allowed relative to now. Example: +42days. Parsed values after this date will be set to current time.|
|» maxEventBytes|number|false|none|The maximum number of bytes in an event before it is flushed to the pipelines|
|» fields|[object]|false|none|Key-value pairs to be added to each event|
|»» name|string|false|none|none|
|»» value|string|true|none|The JavaScript expression used to compute the field's value (can be constant)|
|» disabled|boolean|false|none|Disable this breaker rule (enabled by default)|
|» parserEnabled|boolean|false|none|none|
|» shouldUseDataRaw|boolean|false|none|Enable to set an internal field on events indicating that the field in the data called _raw should be used. This can be useful for post processors that want to use that field for event._raw, instead of replacing it with the actual raw event.|

#### Enumerated Values

|Property|Value|
|---|---|
|lib|custom|
|lib|cribl-custom|
|type|regex|
|type|json|
|type|json_array|
|type|header|
|type|timestamp|
|type|csv|
|type|aws_cloudtrail|
|type|aws_vpcflow|
|type|auto|
|type|format|
|type|current|

<h2 id="tocS_SearchParameterValue">SearchParameterValue</h2>
<!-- backwards compatibility -->
<a id="schemasearchparametervalue"></a>
<a id="schema_SearchParameterValue"></a>
<a id="tocSsearchparametervalue"></a>
<a id="tocssearchparametervalue"></a>

```json
"string"

```

### Properties

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|number|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|boolean|false|none|none|

<h2 id="tocS_SearchParameterType">SearchParameterType</h2>
<!-- backwards compatibility -->
<a id="schemasearchparametertype"></a>
<a id="schema_SearchParameterType"></a>
<a id="tocSsearchparametertype"></a>
<a id="tocssearchparametertype"></a>

```json
"string"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|string|
|*anonymous*|number|
|*anonymous*|boolean|

<h2 id="tocS_StageDependency">StageDependency</h2>
<!-- backwards compatibility -->
<a id="schemastagedependency"></a>
<a id="schema_StageDependency"></a>
<a id="tocSstagedependency"></a>
<a id="tocsstagedependency"></a>

```json
{
  "dependentFields": [
    "string"
  ],
  "id": "string",
  "type": "stage"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|dependentFields|[string]|false|none|none|
|id|[StageId](#schemastageid)|true|none|none|
|type|[StageDependencyType](#schemastagedependencytype)|true|none|none|

<h2 id="tocS_JobExecutionWarning">JobExecutionWarning</h2>
<!-- backwards compatibility -->
<a id="schemajobexecutionwarning"></a>
<a id="schema_JobExecutionWarning"></a>
<a id="tocSjobexecutionwarning"></a>
<a id="tocsjobexecutionwarning"></a>

```json
{
  "text": "string",
  "type": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|text|string|false|none|none|
|type|string|true|none|none|

<h2 id="tocS_SearchConfig">SearchConfig</h2>
<!-- backwards compatibility -->
<a id="schemasearchconfig"></a>
<a id="schema_SearchConfig"></a>
<a id="tocSsearchconfig"></a>
<a id="tocssearchconfig"></a>

```json
{
  "canComputeMetadataDistributively": true,
  "datasets": [
    "string"
  ],
  "hasSendOperator": true,
  "logicalPlans": {
    "Combined": {
      "property1": [
        {
          "isPreviewableOperation": true,
          "type": "aggregate"
        }
      ],
      "property2": [
        {
          "isPreviewableOperation": true,
          "type": "aggregate"
        }
      ]
    },
    "Coordinated": {
      "property1": [
        {
          "isPreviewableOperation": true,
          "type": "aggregate"
        }
      ],
      "property2": [
        {
          "isPreviewableOperation": true,
          "type": "aggregate"
        }
      ]
    },
    "Federated": {
      "property1": [
        {
          "isPreviewableOperation": true,
          "type": "aggregate"
        }
      ],
      "property2": [
        {
          "isPreviewableOperation": true,
          "type": "aggregate"
        }
      ]
    }
  },
  "orderedFieldNames": [
    "string"
  ],
  "pipelines": {
    "Combined": {
      "id": "string",
      "conf": {
        "asyncFuncTimeout": 10000,
        "output": "default",
        "description": "string",
        "streamtags": [],
        "functions": [
          {
            "filter": "true",
            "id": "string",
            "description": "string",
            "disabled": true,
            "final": true,
            "conf": {},
            "groupId": "string"
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
        }
      }
    },
    "Coordinated": {
      "id": "string",
      "conf": {
        "asyncFuncTimeout": 10000,
        "output": "default",
        "description": "string",
        "streamtags": [],
        "functions": [
          {
            "filter": "true",
            "id": "string",
            "description": "string",
            "disabled": true,
            "final": true,
            "conf": {},
            "groupId": "string"
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
        }
      }
    },
    "Federated": {
      "id": "string",
      "conf": {
        "asyncFuncTimeout": 10000,
        "output": "default",
        "description": "string",
        "streamtags": [],
        "functions": [
          {
            "filter": "true",
            "id": "string",
            "description": "string",
            "disabled": true,
            "final": true,
            "conf": {},
            "groupId": "string"
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
        }
      }
    }
  },
  "referencedColumnPaths": [
    [
      "string"
    ]
  ],
  "searchTerms": [
    {
      "isCaseSensitive": true,
      "term": "string"
    }
  ],
  "sortFields": [
    {
      "direction": "ascending",
      "fieldName": "string",
      "nullPosition": "nullsFirst"
    }
  ],
  "useFormattedVisualization": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|canComputeMetadataDistributively|boolean|false|none|none|
|datasets|[string]|true|none|none|
|hasSendOperator|boolean|true|none|none|
|logicalPlans|object|true|none|none|
|» Combined|[LogicalPlan](#schemalogicalplan)|false|none|none|
|» Coordinated|[LogicalPlan](#schemalogicalplan)|false|none|none|
|» Federated|[LogicalPlan](#schemalogicalplan)|false|none|none|
|orderedFieldNames|[string]|true|none|none|
|pipelines|object|true|none|none|
|» Combined|[Pipeline](#schemapipeline)|false|none|none|
|» Coordinated|[Pipeline](#schemapipeline)|false|none|none|
|» Federated|[Pipeline](#schemapipeline)|false|none|none|
|referencedColumnPaths|[[ColumnPath](#schemacolumnpath)]|false|none|none|
|searchTerms|[[SearchTerm](#schemasearchterm)]|true|none|none|
|sortFields|[[SortByField](#schemasortbyfield)]|false|none|none|
|useFormattedVisualization|boolean|true|none|none|

<h2 id="tocS_ColumnFilterSettings">ColumnFilterSettings</h2>
<!-- backwards compatibility -->
<a id="schemacolumnfiltersettings"></a>
<a id="schema_ColumnFilterSettings"></a>
<a id="tocScolumnfiltersettings"></a>
<a id="tocscolumnfiltersettings"></a>

```json
{
  "contains": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|contains|[ColumnSetting](#schemacolumnsetting)|true|none|none|

<h2 id="tocS_ColumnFormatSettings">ColumnFormatSettings</h2>
<!-- backwards compatibility -->
<a id="schemacolumnformatsettings"></a>
<a id="schema_ColumnFormatSettings"></a>
<a id="tocScolumnformatsettings"></a>
<a id="tocscolumnformatsettings"></a>

```json
{
  "palette": {},
  "precision": {},
  "prefix": {},
  "suffix": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|palette|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|precision|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|prefix|[ColumnSetting](#schemacolumnsetting)|true|none|none|
|suffix|[ColumnSetting](#schemacolumnsetting)|true|none|none|

<h2 id="tocS_ColumnOrderSettings">ColumnOrderSettings</h2>
<!-- backwards compatibility -->
<a id="schemacolumnordersettings"></a>
<a id="schema_ColumnOrderSettings"></a>
<a id="tocScolumnordersettings"></a>
<a id="tocscolumnordersettings"></a>

```json
{
  "order": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|order|[ColumnSetting](#schemacolumnsetting)|true|none|none|

<h2 id="tocS_ColumnSortSettings">ColumnSortSettings</h2>
<!-- backwards compatibility -->
<a id="schemacolumnsortsettings"></a>
<a id="schema_ColumnSortSettings"></a>
<a id="tocScolumnsortsettings"></a>
<a id="tocscolumnsortsettings"></a>

```json
{
  "sort": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|sort|[ColumnSetting](#schemacolumnsetting)|true|none|none|

<h2 id="tocS_TopValue">TopValue</h2>
<!-- backwards compatibility -->
<a id="schematopvalue"></a>
<a id="schema_TopValue"></a>
<a id="tocStopvalue"></a>
<a id="tocstopvalue"></a>

```json
{
  "count": 0,
  "value": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|count|number|true|none|none|
|value|string|true|none|none|

<h2 id="tocS_FieldType">FieldType</h2>
<!-- backwards compatibility -->
<a id="schemafieldtype"></a>
<a id="schema_FieldType"></a>
<a id="tocSfieldtype"></a>
<a id="tocsfieldtype"></a>

```json
"array"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|array|
|*anonymous*|boolean|
|*anonymous*|number|
|*anonymous*|object|
|*anonymous*|string|
|*anonymous*|timestamp|

<h2 id="tocS_StageId">StageId</h2>
<!-- backwards compatibility -->
<a id="schemastageid"></a>
<a id="schema_StageId"></a>
<a id="tocSstageid"></a>
<a id="tocsstageid"></a>

```json
"string"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

<h2 id="tocS_StageDependencyType">StageDependencyType</h2>
<!-- backwards compatibility -->
<a id="schemastagedependencytype"></a>
<a id="schema_StageDependencyType"></a>
<a id="tocSstagedependencytype"></a>
<a id="tocsstagedependencytype"></a>

```json
"stage"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|stage|
|*anonymous*|stage-scalar|

<h2 id="tocS_LogicalPlan">LogicalPlan</h2>
<!-- backwards compatibility -->
<a id="schemalogicalplan"></a>
<a id="schema_LogicalPlan"></a>
<a id="tocSlogicalplan"></a>
<a id="tocslogicalplan"></a>

```json
{
  "property1": [
    {
      "isPreviewableOperation": true,
      "type": "aggregate"
    }
  ],
  "property2": [
    {
      "isPreviewableOperation": true,
      "type": "aggregate"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|**additionalProperties**|[object]|false|none|none|
|» isPreviewableOperation|boolean|false|none|none|
|» type|[LogicalPlanNodeType](#schemalogicalplannodetype)|true|none|none|

<h2 id="tocS_Pipeline">Pipeline</h2>
<!-- backwards compatibility -->
<a id="schemapipeline"></a>
<a id="schema_Pipeline"></a>
<a id="tocSpipeline"></a>
<a id="tocspipeline"></a>

```json
{
  "id": "string",
  "conf": {
    "asyncFuncTimeout": 10000,
    "output": "default",
    "description": "string",
    "streamtags": [],
    "functions": [
      {
        "filter": "true",
        "id": "string",
        "description": "string",
        "disabled": true,
        "final": true,
        "conf": {},
        "groupId": "string"
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
    }
  }
}

```

Pipeline Settings

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|conf|object|true|none|none|
|» asyncFuncTimeout|integer|false|none|Time (in ms) to wait for an async function to complete processing of a data item|
|» output|string|false|none|The output destination for events processed by this Pipeline|
|» description|string|false|none|none|
|» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|» functions|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|none|List of Functions to pass data through|
|» groups|object|false|none|none|
|»» **additionalProperties**|object|false|none|none|
|»»» name|string|true|none|none|
|»»» description|string|false|none|Short description of this group|
|»»» disabled|boolean|false|none|Whether this group is disabled|

<h2 id="tocS_ColumnPath">ColumnPath</h2>
<!-- backwards compatibility -->
<a id="schemacolumnpath"></a>
<a id="schema_ColumnPath"></a>
<a id="tocScolumnpath"></a>
<a id="tocscolumnpath"></a>

```json
[
  "string"
]

```

### Properties

*None*

<h2 id="tocS_SearchTerm">SearchTerm</h2>
<!-- backwards compatibility -->
<a id="schemasearchterm"></a>
<a id="schema_SearchTerm"></a>
<a id="tocSsearchterm"></a>
<a id="tocssearchterm"></a>

```json
{
  "isCaseSensitive": true,
  "term": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|isCaseSensitive|boolean|true|none|none|
|term|string|true|none|none|

<h2 id="tocS_SortByField">SortByField</h2>
<!-- backwards compatibility -->
<a id="schemasortbyfield"></a>
<a id="schema_SortByField"></a>
<a id="tocSsortbyfield"></a>
<a id="tocssortbyfield"></a>

```json
{
  "direction": "ascending",
  "fieldName": "string",
  "nullPosition": "nullsFirst"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|direction|string|true|none|none|
|fieldName|string|true|none|none|
|nullPosition|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|direction|ascending|
|direction|descending|
|nullPosition|nullsFirst|
|nullPosition|nullsLast|

<h2 id="tocS_ColumnSetting">ColumnSetting</h2>
<!-- backwards compatibility -->
<a id="schemacolumnsetting"></a>
<a id="schema_ColumnSetting"></a>
<a id="tocScolumnsetting"></a>
<a id="tocscolumnsetting"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocS_LogicalPlanNodeType">LogicalPlanNodeType</h2>
<!-- backwards compatibility -->
<a id="schemalogicalplannodetype"></a>
<a id="schema_LogicalPlanNodeType"></a>
<a id="tocSlogicalplannodetype"></a>
<a id="tocslogicalplannodetype"></a>

```json
"aggregate"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|aggregate|
|*anonymous*|dedup|
|*anonymous*|distinct|
|*anonymous*|extract|
|*anonymous*|filter|
|*anonymous*|limit|
|*anonymous*|mv-expand|
|*anonymous*|mv-pull|
|*anonymous*|noop|
|*anonymous*|pivot|
|*anonymous*|project|
|*anonymous*|sort|

<h2 id="tocS_PipelineFunctionConf">PipelineFunctionConf</h2>
<!-- backwards compatibility -->
<a id="schemapipelinefunctionconf"></a>
<a id="schema_PipelineFunctionConf"></a>
<a id="tocSpipelinefunctionconf"></a>
<a id="tocspipelinefunctionconf"></a>

```json
{
  "filter": "true",
  "id": "string",
  "description": "string",
  "disabled": true,
  "final": true,
  "conf": {},
  "groupId": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|filter|string|false|none|Filter that selects data to be fed through this Function|
|id|string|true|none|Function ID|
|description|string|false|none|Simple description of this step|
|disabled|boolean|false|none|If true, data will not be pushed through this function|
|final|boolean|false|none|If enabled, stops the results of this Function from being passed to the downstream Functions|
|conf|object|true|none|none|
|groupId|string|false|none|Group ID|

