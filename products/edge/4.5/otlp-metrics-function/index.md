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
  "__inputId": "prometheus_rw:prom_rw_in"
}
```

The same sample event after conversion by the OTLP Metrics Function:

```
{
  "_time": 1700236380.37,
  "instrumentation_library_metrics": [
    {
      "schema_url": "",
      "metrics": [
        {
          "__type": "gauge",
          "gauge": {
            "data_points": [
              {
                "attributes": [
                  {
                    key: "cribl_host",
                    value: {
                      string_value: "P4MD6R-Pv8x",
                    }
                  },
                  {
                    key: "cribl_wp",
                    value: {
                      string_value: "w0",
                    }
                  },
                  {
                    key: "event_host",
                    value: {
                      string_value: "P4MD6R-Pv8x",
                    }
                  },
                  {
                    key: "event_source",
                    value: {
                      string_value: "cribl",
                    }
                  },
                  {
                    key: "host",
                    value: {
                      string_value: "P4MD6R-Pv8x",
                    }
                  },
                  {
                    key: "output",
                    value: {
                      string_value: "open_telemetry:otel_webhook_dest",
                    }
                  },
                  {
                    key: "source",
                    value: {
                      string_value: "cribl"
                    }
                  },
                ],
                "start_time_unix_nano": 1700236380370000000,
                "time_unix_nano": 1700236380370000000,
                "as_double": 123.456
              }
            ]
          },
          "name": "cribl_logstream_health_outputs"
        }
      ]
    }
  ]
}
```
