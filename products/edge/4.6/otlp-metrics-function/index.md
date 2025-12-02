# OTLP Metrics


The OTLP Metrics Function transforms dimensional metrics events into the [OTLP](https://github.com/open-telemetry/opentelemetry-proto/blob/main/docs/README.md) (OpenTelemetry Protocol) format. This Function can be used to send metric events (events that contain the internal field `__criblMetrics`) to any OpenTelemetry Metrics-capable destinations. 

> For detailed insights into events that passed through the Function but were dropped, set the logging level for channel `func:otlp_metrics` to `debug`, and there will be a stats log message output once per minute with the relevant information. 
>
{.box .info}

> This Function will delete the internal attribute `__criblMetrics`, making events only suitable for destinations that use the OTLP format. 
>
{.box .warning}

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. The default `true` setting passes all events through the Function.

**Description**: Simple description of this Function’s purpose in this Pipeline. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Resource attribute prefixes**: The prefixes of top-level attributes to add as resource attributes. Each attribute must match the regex pattern `^[a-zA-Z0-9]+$`. Use Eval to copy nested attributes to the top level for matching. See the OpenTelemetry documentation for [Resource Semantic Conventions](https://opentelemetry.io/docs/specs/semconv/resource/). Remove all the prefixes if you want to bring none of the Resources over.

**Drop non-metric events**: Whether or not to drop any non-metric events.

## Basic Example

Filter: `__inputId=='prometheus_rw:prom_rw_in'`

This will grab all events from a [Prometheus Remote Write Source](sources-prometheus-remote-write) and convert the metrics to OTLP format.

> Ensure that the internal attribute `__criblMetrics` does not contain `null` or blank values in any of the child object arrays. 
>
{.box .info}
 
Sample event: 

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

The same sample event after conversion by the OTLP Metrics Function:

```
{
  "_time": 1700236380.37,
  "resource": {
    "attributes": [
      {
        "key": "env",
        "value": { "string_value": "prod" }
      },
      {
        "key": "hostname",
        "value": { "string_value": "myhost" }
      },
      {
        "key": "datacenter",
        "value": { "string_value": "sdc" }
      },
      {
        "key": "region",
        "value": { "string_value": "europe" }
      },
      {
        "key": "owner",
        "value": { "string_value": "frontend" }
      },
      {
        "key": "host",
        "value": { "string_value": "6V2GQ1-Ehi1" }
      },
      {
        "key": "source",
        "value": { "string_value": "http://localhost:9100/metrics" }
      },
      {
        "key": "service.name",
        "value": { "string_value": "serviceName" }
      },
      {
        "key": "service.instanceId",
        "value": { "string_value": "serviceInstanceId" }
      },
      {
        "key": "system.name",
        "value": { "string_value": "systemName" }
      },
      {
        "key": "telemetry.sdk.name",
        "value": { "string_value": "opentelemetry" }
      },
      {
        "key": "telemetry.sdk.language",
        "value": { "string_value": "java" }
      },
      {
        "key": "k8s.cluster.name",
        "value": { "string_value": "opentelemetry-cluster" }
      },
      {
        "key": "cloud.account.id",
        "value": { "int_value": 1111111 }
      },
      {
        "key": "cloud.platform",
        "value": { "string_value": "aws" }
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
                    "value": { "string_value": "P4MD6R-Pv8x" }
                  },
                  {
                    "key": "cribl_wp",
                    "value": { "string_value": "w0" }
                  },
                  {
                    "key": "event_host",
                    "value": { "string_value": "P4MD6R-Pv8x" }
                  },
                  {
                    "key": "event_source",
                    "value": { "string_value": "cribl" }
                  },
                  {
                    "key": "host",
                    "value": { "string_value": "P4MD6R-Pv8x" }
                  },
                  {
                    "key": "output",
                    "value": {
                      "string_value": "open_telemetry:otel_webhook_dest"
                    }
                  },
                  {
                    "key": "source",
                    "value": { "string_value": "cribl" }
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
