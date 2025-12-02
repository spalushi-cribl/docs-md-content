# service.yml


`service.yml` maintains configuration for Cribl Stream service processes.

In the UI, you can configure some of these settings on the Leader at **Settings** > **Global** > **System** > **Service Processes** > **Services**.

> Be careful about reducing the predefined `memoryLimit` values.
> Extremely low values can prevent some Cribl Stream components from functioning.
>
{.box .warning}



```yaml {title="service.yml"}
# Service configuration - Configuration for various Cribl services
# Available service types: connections, metrics, notifications, stream_connections, edge_connections, jobs, outpost
connections:
  # Connections listener number of processes - Negative values are offsets from the number of available cores
  # on the host.
  # [number; default: 1]
  procs:
  # Connections listener heap memory limit - Heap memory limit for the connection listener processes
  # [string; default: 2GB]
  memoryLimit:
metrics:
  # Metrics number of processes - Single process; values above 1 will be treated as 1, negative values are
  # offsets from the number of available cores on the host.
  # [number; default: 1]
  procs:
  # Metrics heap memory limit - Heap memory limit for the metrics process
  # [string; default: 2GB]
  memoryLimit:
notifications:
  # Notifications number of processes - Single process; values above 1 will be treated as 1, negative values
  # are offsets from the number of available cores on the host.
  # [number; default: 1]
  procs:
  # Notifications heap memory limit - Heap memory limit for the notifications process
  # [string; default: 2GB]
  memoryLimit:
stream_connections:
  # Stream connection processes - Negative values are offsets from the number of available cores on the host.
  # [number; default: 1]
  procs:
  # Auto scale - Do not modify this setting. Defines whether Leader can start the service once qualifying
  # Worker node connects to it.
  # [boolean]
  autoScale:
  # Stream connection memory limit - Heap memory limit for the Stream connection processes
  # [string; default: 2GB]
  memoryLimit:
edge_connections:
  # Edge connection processes - Negative values are offsets from the number of available cores on the host.
  # [number; default: 1]
  procs:
  # Edge auto scale - Do not modify this setting. Defines whether Leader can start the service once
  # qualifying Edge node connects to it.
  # [boolean]
  autoScale:
  # Edge connection memory limit - Heap memory limit for the Edge connection processes
  # [string; default: 2GB]
  memoryLimit:
jobs:
  # Jobs service processes - Number of processes to spawn for jobs service. Set to 0 to disable the service.
  # [number; default: 1]
  procs:
  # Jobs service memory limit - Heap memory limit per process.
  # [string; default: 2GB]
  memoryLimit:
outpost:
  # Outpost service processes - Number of processes to spawn for outpost service. Set to 0 to disable the
  # service.
  # [number; default: -2]
  procs:
  # Outpost service memory limit - Heap memory limit per process.
  # [string; default: 2GB]
  memoryLimit:
```
