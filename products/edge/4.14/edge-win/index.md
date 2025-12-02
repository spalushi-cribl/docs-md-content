# Cribl Edge on Windows


Check the options and limitations for running Cribl Edge on Windows.

---

Cribl Edge on Windows offers easy-to-use tools for exploring and collecting Windows events.
You can run Cribl Edge on a supported Windows environment and collect events via the Windows Events API. 

## Limitations 

Cribl Edge on Windows is currently subject to the following limitations:

### Modes 

Cribl Edge on Windows supports only the following modes:

- `Edge: Single`
- `Edge: Managed Edge (managed by Leader)`

This means you can't switch Cribl Edge on Windows into Cribl Stream mode (Single-instance, Worker, or Leader).

You can, however, switch between the Cribl Edge supported modes via the UI, in
 **Settings** > **Distributed Settings** > **Mode**.

> Do not select an unsupported mode from this drop-down! Doing so will cause the
> Cribl service to fail.
{.box .warning}

### Sources and Destinations  

Cribl Edge on Windows supports the same [Sources](sources) and [Destinations](destinations) as Cribl Edge on Linux, with the following exceptions:
- The [AppScope](sources-appscope) and [System Metrics](sources-system-metrics) Sources are unavailable on Windows.
- The [Kubernetes Logs](sources-kubernetes-logs) and [Kubernetes Metrics](sources-kubernetes-metrics) Sources. Cribl Edge on Windows does not support Kubernetes deployments.
- The [Kafka](destinations-kafka) Destination does not support Kerberos authentication on Windows.  

### Sources on Windows Only 

The following Sources are available **only** when running Cribl Edge on Windows (not on Linux): 

- [Windows Event Logs](sources-windows-event-logs)
- [Window Metrics](sources-windows-metrics) 

### Functions

The [Grok Function](grok-function) is unavailable on Windows. If you include it in a Pipeline, Cribl Edge processing will skip over it. 

### Data Formats 

Cribl Edge on Windows does not support reading or writing Parquet files. This is a limitation on the following Destinations: Amazon S3, Azure Blob Storage, MinIO, and FileSystem/NFS. 
