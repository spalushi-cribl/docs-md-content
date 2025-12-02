# Cribl Edge on MacOS


Learn about running Cribl Edge on macOS.

---

> ##### Preview Feature {#preview}
>
> This Preview feature is still being developed. We do not recommend using it in a production environment,
> because the feature might not be fully tested or optimized for performance, and related documentation could be incomplete.
>
> Please continue to submit feedback through normal Cribl [support channels](/support/),
> but assistance might be limited while the feature remains in Preview.
> 
{.box .info}

Cribl Edge on macOS offers easy-to-use tools for exploring and collecting macOS events.
You can run Cribl Edge on a supported macOS machine to collect logs and track local files.

Edge on macOS does not include support for system metrics or system state at this time.
See below for a more detailed list of limitations.

## Limitations 

Cribl Edge on macOS is subject to the following limitations.

### Modes 

Cribl Edge on macOS supports only the following modes:

- `Edge: Single`
- `Edge: Managed Edge (managed by Leader)`

This means you can't switch Cribl Edge on macOS into Cribl Stream mode (Single-instance, Worker, or Leader).

You can, however, switch between the Cribl Edge supported modes via the UI,
in **Settings** > **Distributed Settings** > **Mode**.

> Do not select an unsupported mode from this drop-down! Doing so will cause the
> Cribl service to fail.
{.box .warning}

### Sources and Destinations

Cribl Edge on macOS supports the same [Sources](sources) and [Destinations](destinations) as Cribl Edge on Linux,
with the following exceptions.

#### Unavailable Sources

The following Sources are unavailable on macOS

- [System Metrics](sources-system-metrics) Source
- [System State](sources-system-state) Source
- [AppScope](sources-appscope) Source

#### Unavailable Destinations

The following Destination is unavailable on macOS:

- [Amazon Security Lake](destinations-security-lake) Destination

#### Other Limitations on Sources and Destinations

Other limitations include:

- The [Kafka](destinations-kafka) Destination does not support Kerberos authentication on macOS.
- The [File Monitor](sources-file-monitor) Source only allows **Manual** discovery mode, not **Auto**.

### Functions

The [Grok Function](grok-function) is unavailable on macOS.
If you include it in a Pipeline, Cribl Edge processing will skip over it. 

### Data Formats 

Cribl Edge on macOS does not support reading or writing Parquet files.
This is a limitation on the following Destinations: Amazon S3, Azure Blob Storage, MinIO, and FileSystem/NFS. 
