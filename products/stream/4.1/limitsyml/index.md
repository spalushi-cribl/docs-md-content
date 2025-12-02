# limits.yml


`limits.yml` maintains parameters for the system. In a distributed deployment, these parameters are configured on both the Leader and the Workers. 

In the UI, you can configure them:
- On the Leader at **Settings** > **Global Settings** > **General Settings > Limits**.
- On a single instance at **Settings** > **General Settings > Limits**.
- On Worker Groups at `<group‑name>` > **Worker Group Settings** > **General Settings > Limits**.

```yaml {title="$CRIBL_HOME/default/cribl/limits.yml"}
samples: # [object] Samples
  maxSize: # [string] Max sample size - Maximum file size for the sample, in binary units (KB, MB). (Max. 3MB.)
minFreeSpace: # [string] Min free disk space - The minimum amount of disk space in the system before various features will take measures to prevent disk usage (KB, MB, etc.).
metricsGCPeriod: # [string] Metrics GC period - The interval on which the system attempts to free memory, by pruning stale metrics from the Stream system metrics store.
metricsMaxCardinality: # [number] Metrics cardinality limit - The system's allowed number of permutations of a given metric name.
metricsMaxDiskSpace: # [string] Metrics max disk space - Maximum allowed disk space for persisting metrics to disk.
metricsWorkerIdBlacklist: # [array of strings] Metrics worker tracking - List of metric names for which to disable tracking of Worker Node ID. Supports wildcards.
metricsNeverDropList: # [array of strings] Metrics never drop list - List metric names for which to ensure delivery. Supports wildcards.
metricsFieldsBlacklist: # [array of strings] Disable field metrics - List of event fields for which to disable metric collection.
metricsDirectory: # [string] Metrics directory - Directory to store metrics on disk.
cpuProfileTTL: # [string] CPU profile TTL - The time-to-live for collected CPU profiles.
eventsMetadataSources: # [array of strings] Event metadata sources - List of event metadata sources to enable.
```
