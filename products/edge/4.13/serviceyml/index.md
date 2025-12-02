# service.yml


`service.yml` maintains configuration for Cribl Edge service processes.

In the UI, you can configure some of these settings on the Leader at **Settings** > **Global** > **System** > **Service Processes** > **Services**.

> Be careful about reducing the predefined `memoryLimit` values.
> Extremely low values can prevent some Cribl Edge components from functioning.
>
{.box .warning}

```yaml {title="service.yml"}
connections: # [Object] - Configuration for the connection listener processes.
  procs: # [number; default: 1, minimum: 1]
  memoryLimit: # [string; default: 2GB] - Heap memory limit for the connection listener processes.
metrics: # [Object] - Configuration for the metrics process
  procs: # [number; default: 1] – Single process; values other than 1 will be treated as 1
  memoryLimit: # [string; default: 2GB] - Heap memory limit for the metrics process
notifications: # [Object] - Configuration for the notifications process
  procs: # [number; default: 1] – Single process; values other than 1 will be treated as 1
  memoryLimit: # [string; default: 2GB] - Heap memory limit for the notifications process
stream_connections: # [Object] - Configuration for the Stream connection processes
  procs: # [number; default: 1, minimum: 1]
  autoScale: # [boolean] - Do not modify this setting. Defines whether Leader can start the service once qualifying Worker node connects to it.
  memoryLimit: # [string; default: 2GB] - Heap memory limit for the Stream connection processes
edge_connections: # [Object] - Configuration for the Edge connection processes
  procs: # [number; default: 1, minimum: 1]
  autoScale: # [boolean] - Do not modify this setting. Defines whether Leader can start the service once qualifying Edge node connects to it.
  memoryLimit: # [string; default: 2GB] - Heap memory limit for the Edge connection processes
```
