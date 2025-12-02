# service.yml


`service.yml` maintains configuration for Cribl Stream service processes.

In the UI, you can configure them on the Leader at **Settings** > **Global Settings** > **System** > **Service Processes** > **Services**.

> Be careful about reducing the predefined limits.
> Extremely low values can prevent some Cribl Stream components from functioning.
>
{.box .warning}

```yaml {title="$CRIBL_HOME/default/cribl/service.yml"}
connections: # [Object] - Heap memory limit for the connection listener processes.
  procs: # [number; default: 1, minimum: 1]
  memoryLimit: # [string; default: 2GB]
metrics: # [Object] - Heap memory limit for the metrics process
  memoryLimit: # [string; default: 2GB]
notifications: # [Object] - Heap memory limit for the notifications process
  memoryLimit: # [string; default: 2GB]
```
