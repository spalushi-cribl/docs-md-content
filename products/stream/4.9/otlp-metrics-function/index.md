# OTLP Metrics


The OTLP Metrics Function transforms dimensional metrics events into the [OTLP](https://github.com/open-telemetry/opentelemetry-proto/blob/main/docs/README.md) (OpenTelemetry Protocol) format. This Function can be used to send metric events (events that contain the internal field `__criblMetrics`) to any OpenTelemetry Metrics-capable destinations. 

> For detailed insights into events that passed through the Function but were dropped, set the logging level for channel `func:otlp_metrics` to `debug`, and there will be a stats log message output once per minute with the relevant information. 
>
{.box .info}

> This Function will delete the internal attribute `__criblMetrics`, making events only suitable for destinations that use the OTLP format. 
>
{.box .warning}

## Usage

> Ideally, this Function should process events and/or metrics **after** other Functions have formatted them. Therefore Cribl recommends that you place this Function last in the Pipeline. 
> 
> If no metric events appear in Simple or Full Preview, try enabling **Show Dropped Events**: If Cribl Stream processed and dropped events, there's an issue with your data. Either the incoming events are malformed, or, the resource attributes in those events do not match any of the resource attribute prefixes you configured. See **Resource attribute prefixes** below. 
{.box .success}

**Filter**: Filter expression (JS) that selects data to feed through the Function. The default `true` setting passes all events through the Function.

**Description**: Simple description of this Function’s purpose in this Pipeline. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Resource attribute prefixes**: The prefixes of top-level attributes to add as resource attributes. See the OpenTelemetry documentation for [Resource Semantic Conventions](https://opentelemetry.io/docs/specs/semconv/resource/). 
  * Use periods and underscores for precise matching. For example, `process.path`. 
  * Remove all the prefixes if you want to bring none of the resources over. 
  * Use the [Eval Function](eval-function) to copy nested attributes to the top level for matching. For example, configure **Evaluate Fields** as follows to create a new top-level field named `process_path` with the value copied from the nested attribute `process.path`. This new field can then be matched using the OTLP Metrics Function's **Resource attribute prefixes**:
    * **Name**: `process_path`
    * **Value Expression**: `process.path`

**Drop non-metric events**: Whether or not to drop any non-metric events.

**OTLP version**: The drop-down offers `0.10.0` (default) and `1.3.1`.

**Batch OTLP metrics**: Whether to batch OTLP metrics by shared top-level `resource` attributes. When enabled, exposes these additional controls:

* **Batch size**: The number of metric data points after which a batch will be sent, regardless of whether the timeout duration has been reached.

* **Batch timeout (ms)**: Time duration after which a batch will be sent regardless of size.

* **Batch size limit (kb)**: Maximum batch size. `0` means no limit.

* **Batch metrics metadata keys**: Optionally, add metadata keys that you want the Function to create batcher instances for, in addition to any batching based on resource attributes. When set, this processor will create one batcher instance per distinct combination of values in the metadata.

* **Metadata cardinality limit**: Limits the number of unique combinations of metadata key values that the batcher will process. Once the limit has been reached, the Function will drop events whose metadata key values constitute a new combination.

### Choosing or Combining OTLP Versions {#otlp-versions}

Because this Function resides in between Cribl Stream OTel Sources and OTel Destinations, each of which can be independently configured to use the same or different OTLP versions, it's important to know the effects of different choices and combinations.

* Cribl Stream supports starting with OTLP 0.10.0 events and converting them to OTLP 1.3.1 events, but does not support the reverse (1.3.1 to 0.10.0).

* When **OTLP version** is set to `1.3.1`, Cribl Stream will attempt to convert metrics or logs formatted for any previous version to 1.3.1. Since beginning with version 1.0.0, OTLP has introduced no new breaking changes, you can use the Function configured for OTLP 1.3.1 as an "OTLP version 0.x to version 1" converter. Most downstream OTLP receivers comply with OTLP 1.0.0 at minimum; but you should verify this for whatever downstream receiver you use.

## Basic Example

Filter: `__inputId=='prometheus_rw:prom_rw_in'`

This will grab all events from a [Prometheus Remote Write Source](sources-prometheus-remote-write) and convert the metrics to OTLP format using only default settings in the Function.

> Ensure that the internal attribute `__criblMetrics` does not contain `null` or blank values in any of the child object arrays. 
>
{.box .info}

See the sample [incoming](#basic-incoming) and [outgoing](#basic-outgoing) events.

## Advanced Example

Here, we have two incoming events sent by a [OTel Source](sources-otel) with **Advanced Settings > Extract logs** turned on.

The Function, with **Batch OTLP logs** turned on, batches the Cribl events such that the Cribl Stream OTel Destination can ingest the batch and convert to a batched OTLP metric event.

See the sample [incoming](#advanced-incoming) and [outgoing](#advanced-outgoing) events.

## Sample Events {#sample-events}

### Basic Example Incoming Event {#basic-incoming}

Here is an example incoming event.  

```
{
  "__criblEventType": "event",
  "__ctrlFields": [],
  "__final": false,
  "__cloneCount": 0,
  "_time": 1700236380.37,
  "_value": 123.456,
  "_metric": "cribl_logstream_health_outputs",
  "_metric_type": "gauge",
  "cribl_host": "P4MD6R-Pv8x",
  "cribl_wp": "w0",
  "event_host": "P4MD6R-Pv8x",
  "event_source": "cribl",
  "host": "P4MD6R-Pv8x",
  "output": "open_telemetry:otel_webhook_dest",
  "source": "cribl",
  "__criblMetrics": [
    {
      "nameExpr": ["_metric"],
      "values": ["_value"],
      "types": ["gauge"],
      "dims": [
        "cribl_host",
        "cribl_wp",
        "event_host",
        "event_source",
        "host",
        "output",
        "source"
      ]
    }
  ],
  "__offset": 8099,
  "__bytes": 228,
  "__srcIpPort": "127.0.0.1:50868",
  "__inputId": "prometheus_rw:prom_rw_in",
  "service.name": "serviceName",
  "service.instanceId": "serviceInstanceId",
  "system": "system",
  "system.name": "systemName",
  "telemetry.sdk.name": "opentelemetry",
  "telemetry.sdk.language": "java",
  "k8s.cluster.name": "opentelemetry-cluster",
  "cloud.account.id": 1111111,
  "cloud.platform": "aws"
}
```

### Basic Example Outgoing Event {#basic-outgoing}

Here is the above sample event after conversion by the OTLP Metrics Function.

```
{
  "_time": 1700236380.37,
  "resource": {
    "attributes": [
      {
        "key": "host",
        "value": {
          "string_value": "P4MD6R-Pv8x"
        }
      },
      {
        "key": "service.name",
        "value": {
          "string_value": "serviceName"
        }
      },
      {
        "key": "service.instanceId",
        "value": {
          "string_value": "serviceInstanceId"
        }
      },
      {
        "key": "system",
        "value": {
          "string_value": "system"
        }
      },
      {
        "key": "system.name",
        "value": {
          "string_value": "systemName"
        }
      },
      {
        "key": "telemetry.sdk.name",
        "value": {
          "string_value": "opentelemetry"
        }
      },
      {
        "key": "telemetry.sdk.language",
        "value": {
          "string_value": "java"
        }
      },
      {
        "key": "k8s.cluster.name",
        "value": {
          "string_value": "opentelemetry-cluster"
        }
      },
      {
        "key": "cloud.account.id",
        "value": {
          "int_value": 1111111
        }
      },
      {
        "key": "cloud.platform",
        "value": {
          "string_value": "aws"
        }
      }
    ]
  },
  "instrumentation_library_metrics": [
    {
      "schema_url": "",
      "metrics": [
        {
          "name": "cribl_logstream_health_outputs",
          "gauge": {
            "data_points": [
              {
                "as_double": 123.456,
                "time_unix_nano": 1700236380370000000,
                "start_time_unix_nano": 1700236380370000000,
                "attributes": [
                  {
                    "key": "cribl_host",
                    "value": {
                      "string_value": "P4MD6R-Pv8x"
                    }
                  },
                  {
                    "key": "cribl_wp",
                    "value": {
                      "string_value": "w0"
                    }
                  },
                  {
                    "key": "event_host",
                    "value": {
                      "string_value": "P4MD6R-Pv8x"
                    }
                  },
                  {
                    "key": "event_source",
                    "value": {
                      "string_value": "cribl"
                    }
                  },
                  {
                    "key": "host",
                    "value": {
                      "string_value": "P4MD6R-Pv8x"
                    }
                  },
                  {
                    "key": "output",
                    "value": {
                      "string_value": "open_telemetry:otel_webhook_dest"
                    }
                  },
                  {
                    "key": "source",
                    "value": {
                      "string_value": "cribl"
                    }
                  }
                ]
              }
            ]
          },
          "__type": "gauge"
        }
      ]
    }
  ]
}

```
### Advanced Example Incoming Events {#advanced-incoming}

In order to show batching in the outgoing events, let's use two incoming events from an [OTel Source](sources-otel) where **Advanced Settings > Extract metrics** is turned on.

#### Incoming Sample Event 1 – Gauge Metric

```json
{
    "_time": 1700236380.37,
    "resource":
    {
        "attributes":
        [
            {
                "key": "service.name",
                "value":
                {
                    "string_value": "serviceName"
                }
            },
            {
                "key": "service.instanceId",
                "value":
                {
                    "string_value": "serviceInstanceId"
                }
            },
            {
                "key": "system.name",
                "value":
                {
                    "string_value": "systemName"
                }
            },
            {
                "key": "telemetry.sdk.name",
                "value":
                {
                    "string_value": "opentelemetry"
                }
            },
            {
                "key": "telemetry.sdk.language",
                "value":
                {
                    "string_value": "java"
                }
            },
            {
                "key": "k8s.cluster.name",
                "value":
                {
                    "string_value": "opentelemetry-cluster"
                }
            }
        ]
    },
    "instrumentation_library_metrics":
    [
        {
            "schema_url": "",
            "metrics":
            [
                {
                    "name": "cribl_logstream_health_outputs",
                    "gauge":
                    {
                        "data_points":
                        [
                            {
                                "as_double": 123.456,
                                "time_unix_nano": 1700236380370000000,
                                "start_time_unix_nano": 1700236380370000000,
                                "attributes":
                                [
                                    {
                                        "key": "cribl_host",
                                        "value":
                                        {
                                            "string_value": "P4MD6R-Pv8x"
                                        }
                                    },
                                    {
                                        "key": "cribl_wp",
                                        "value":
                                        {
                                            "string_value": "w0"
                                        }
                                    },
                                    {
                                        "key": "event_host",
                                        "value":
                                        {
                                            "string_value": "P4MD6R-Pv8x"
                                        }
                                    },
                                    {
                                        "key": "event_source",
                                        "value":
                                        {
                                            "string_value": "cribl"
                                        }
                                    },
                                    {
                                        "key": "host",
                                        "value":
                                        {
                                            "string_value": "P4MD6R-Pv8x"
                                        }
                                    },
                                    {
                                        "key": "output",
                                        "value":
                                        {
                                            "string_value": "open_telemetry:otel_webhook_dest"
                                        }
                                    },
                                    {
                                        "key": "source",
                                        "value":
                                        {
                                            "string_value": "cribl"
                                        }
                                    }
                                ]
                            }
                        ]
                    },
                    "__type": "gauge",
                    "description": "This is a metric description"
                }
            ],
            "instrumentation_library":
            {
                "name": "io.opentelemetry.sdk.trace",
                "version": "1.33.0-alpha"
            }
        }
    ],
    "__outLen": 1266,
    "__bufToken": "otlp-metrics-batcher1"
}
```

#### Incoming Sample Event 2 – Summary Metric

```json
{
    "__criblEventType": "event",
    "__ctrlFields":
    [],
    "__final": false,
    "__cloneCount": 0,
    "__bytes": 404,
    "__criblMetrics":
    [
        {
            "dims":
            [
                "cribl_host",
                "cribl_wp"
            ],
            "nameExpr":
            [
                "_metric"
            ],
            "types":
            [
                "summary"
            ],
            "values":
            [
                "_value"
            ]
        }
    ],
    "__inputId": "prometheus_rw:prom_rw_in",
    "__offset": 5083,
    "__srcIpPort": "127.0.0.1:59491",
    "_metric": "request_duration_summary",
    "_metric_type": "summary",
    "_time": 1701793930,
    "description": "This is an event description",
    "_value": 0,
    "cribl_host": "P4MD6R-Pv8x",
    "cribl_wp": "w0",
    "request_duration_summary_data":
    {
        "_count": 0,
        "_quantiles":
        {
            "0.5": 0,
            "0.9": 0
        },
        "_sum": 0
    },
    "instrumentation_library":
    {
        "name": "io.opentelemetry.sdk.trace",
        "version": "1.33.0-alpha"
    },
    "service.name": "serviceName",
    "service.instanceId": "serviceInstanceId",
    "system": "system",
    "system.name": "systemName",
    "telemetry.sdk.name": "opentelemetry",
    "telemetry.sdk.language": "java",
    "k8s.cluster.name": "opentelemetry-cluster",
    "cloud.account.id": 1111111,
    "cloud.platform": "aws"
}
```


### Advanced Example Outgoing Batched Events {#advanced-outgoing}

Here is the resulting converted OTLP metric events in a single batch.

```json
{
    "_time": 1700236380.37,
    "resource":
    {
        "attributes":
        [
            {
                "key": "service.name",
                "value":
                {
                    "string_value": "serviceName"
                }
            },
            {
                "key": "service.instanceId",
                "value":
                {
                    "string_value": "serviceInstanceId"
                }
            },
            {
                "key": "system.name",
                "value":
                {
                    "string_value": "systemName"
                }
            },
            {
                "key": "telemetry.sdk.name",
                "value":
                {
                    "string_value": "opentelemetry"
                }
            },
            {
                "key": "telemetry.sdk.language",
                "value":
                {
                    "string_value": "java"
                }
            },
            {
                "key": "k8s.cluster.name",
                "value":
                {
                    "string_value": "opentelemetry-cluster"
                }
            }
        ]
    },
    "instrumentation_library_metrics":
    [
        {
            "schema_url": "",
            "metrics":
            [
                {
                    "name": "cribl_logstream_health_outputs",
                    "gauge":
                    {
                        "data_points":
                        [
                            {
                                "as_double": 123.456,
                                "time_unix_nano": 1700236380370000000,
                                "start_time_unix_nano": 1700236380370000000,
                                "attributes":
                                [
                                    {
                                        "key": "cribl_host",
                                        "value":
                                        {
                                            "string_value": "P4MD6R-Pv8x"
                                        }
                                    },
                                    {
                                        "key": "cribl_wp",
                                        "value":
                                        {
                                            "string_value": "w0"
                                        }
                                    },
                                    {
                                        "key": "event_host",
                                        "value":
                                        {
                                            "string_value": "P4MD6R-Pv8x"
                                        }
                                    },
                                    {
                                        "key": "event_source",
                                        "value":
                                        {
                                            "string_value": "cribl"
                                        }
                                    },
                                    {
                                        "key": "host",
                                        "value":
                                        {
                                            "string_value": "P4MD6R-Pv8x"
                                        }
                                    },
                                    {
                                        "key": "output",
                                        "value":
                                        {
                                            "string_value": "open_telemetry:otel_webhook_dest"
                                        }
                                    },
                                    {
                                        "key": "source",
                                        "value":
                                        {
                                            "string_value": "cribl"
                                        }
                                    }
                                ]
                            }
                        ]
                    },
                    "__type": "gauge",
                    "description": "This is a metric description"
                },
                {
                    "name": "request_duration_summary",
                    "summary":
                    {
                        "data_points":
                        [
                            {
                                "as_int": 0,
                                "count": 0,
                                "sum": 0,
                                "quantile_values":
                                {
                                    "0.5": 0,
                                    "0.9": 0
                                },
                                "time_unix_nano": 1701793930000000000,
                                "start_time_unix_nano": 1701793930000000000,
                                "attributes":
                                [
                                    {
                                        "key": "cribl_host",
                                        "value":
                                        {
                                            "string_value": "P4MD6R-Pv8x"
                                        }
                                    },
                                    {
                                        "key": "cribl_wp",
                                        "value":
                                        {
                                            "string_value": "w0"
                                        }
                                    }
                                ]
                            }
                        ]
                    },
                    "__type": "summary",
                    "description": "This is an event description"
                }
            ],
            "instrumentation_library":
            {
                "name": "io.opentelemetry.sdk.trace",
                "version": "1.33.0-alpha"
            }
        }
    ],
    "__outLen": 1266,
    "__bufToken": "otlp-metrics-batcher1"
}
```

