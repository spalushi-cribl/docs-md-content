# Data Payloads for Connected Environments


In a [Connected Environment](cloud-connected-env) setup, on-premises Leaders connect to a Cribl.Cloud Leader in the same way that Edge Nodes connect to a Leader. The Cribl.Cloud Leader sends down a license, and the on-premises Leader sends up a heartbeat that includes telemetry, which encompasses metrics and other metadata.

## Why Is the Telemetry Important?

The telemetry sent from an on-premises Leader to a Cribl.Cloud Leader is encapsulated in an event. This event serves as a heartbeat, confirming that the connected environment is active and functioning properly. It includes key metadata such as the system’s status, timestamp, configuration details, and metrics. Cribl.Cloud uses this data to:
- Monitor connectivity between the on-premises Leader and the cloud.
- Track system health by evaluating fields like status, edgeNodes, and metrics.
- Ensure compliance with licensing by verifying the config.featuresRev field.
- Provide insights into data flow, such as input and output metrics.

### How Often Is the Heartbeat Sent?
The heartbeat interval is determined by the **Task heartbeat period** field, which specifies the number of seconds between each event. In the provided example below, the heartbeat is sent every `10` seconds. This interval is configurable, meaning administrators can adjust it to optimize performance based on their needs. For details, see [Task Manifest and Buffering Limits](settings-group-fleet#task-manifest-and-buffering-limits).

### What Happens if the Heartbeat Is Missed?
If a heartbeat event is not received within the expected interval, the following actions may occur:

- **Retries** – The on-premises Leader may attempt to resend the heartbeat after a short delay.
- **Alerting** – Cribl.Cloud can generate an alert if the heartbeat is missing for an extended period, indicating a potential issue.
- **Failure detection** – A prolonged absence of heartbeats may be interpreted as a disconnection or system failure, prompting further investigation or automated recovery steps.

## On-Premises Leader to Cribl.Cloud Leader Payload
This section describes the structure of the event payload sent from an on-premises Leader to a Cribl.Cloud Leader in a Connected Environment.

### Sample Event

Here is a sample event that Cribl.Cloud received from a connected environment.

```shell

{
  "__criblEventType": "event",
  "type": "info",
  "_time": 1737577024.567,
  "status": "healthy",
  "localTime": 1737577024,
  "config.hbPeriodSeconds": 10,
  "config.featuresRev": "YbK7TIGe4jp2rdujnFiJqq0OBV67r2FKGvrHr4C6I5g=",
  "edgeNodes": 50,
  "__socketAddr": 192.168.1.100:443,
  "__srcIpPort": 10.0.0.5:52314,
  "metrics": [
    [
      0,
      0,
      "total.in_bytes",
      233,
      1737578280038,
      1737578340038,
      [
        "ci",
        "tcp:in_tcp",
        "input",
        "tcp:in_tcp",
        "input_type",
        "tcp",
        "__worker_group",
        "default",
        "__dist_mode",
        "worker",
        "__worker_node",
        "46731aec-d220-4cb0-9971-0dd0a7907d54",
        "__product",
        "stream"
      ],
      null,
      "float32"
    ]
  ],
  "__raw": "{\"type\":\"info\",\"_time\":173757…}"
}

```
### Event Payload Fields 

The following table describes the important fields in the event payload.

| Field                   | Usage                                                              | Example |
|-------------------------|--------------------------------------------------------------------|---------|
| __criblEventType        | Type of event                                                    | event (always) |
| type                    | Type of event                                                    | info (always) |
| _time                   | Timestamp of the event as seconds since Unix epoch              | 1737577024.567 |
| status                  | Status of client                                                | healthy |
| localTime               | Local timestamp in seconds since epoch                          | 1737577024 |
| config.hbPeriodSeconds  | Seconds between client heartbeats                              | 10 |
| config.featuresRev      | Version of feature flags (internal use)                         | YbK7TIGe4jp2rdujnFiJqq0OBV67r2FKGvrHr4C6I5g= |
| edgeNodes               | Number of connected Edge Nodes                                  | 50 |
| __socketAddr            | Connection detail                                               | `192.168.1.100:443`  |
| __srcIpPort            | Connection detail                                               | `10.0.0.5:52314`|
| metrics                 | Array of metrics (as nested arrays) included in this heartbeat. This sample only shows one nested metric array but an actual heartbeat will contain many metrics such as in bytes, out bytes, in events, out events for each input as well as an aggregated total. | ```[ [ 0, 0, "total.in_bytes", 233, 1737578280038, 1737578340038, [ "ci", "tcp:in_tcp", "input", "tcp:in_tcp", "input_type", "tcp", "__worker_group", "default", "__dist_mode", "worker", "__worker_node", "46731aec-d220-4cb0-9971-0dd0a7907d54", "__product", "stream" ], null, "float32" ] ]``` |
| __raw                   | Holds the full raw data of the event as a JSON string           | ```{"type":"info","_time":173757…``` |
