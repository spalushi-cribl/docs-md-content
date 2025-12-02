# limits.yml


`limits.yml` maintains parameters for the system. In a distributed deployment, these parameters are configured on both the Leader and the Workers. 

In the UI, you can configure them:
- On the Leader at **Settings** > **Global** > **General Settings > Limits**.
- On a single instance at **Settings** > **General Settings > Limits**.
- On Worker Groups at `<Group/Fleet‑name>` > **Group Settings** > **General Settings > Limits**.

Where we do not specify or validate a minimum or maximum value, Cribl provides a prudent default. Adjust this initial value to match your architecture and needs.

```yaml {title="$CRIBL_HOME/default/cribl/limits.yml"}
samples: # [object] Samples
  maxSize: # [string; maximum: 3 MB; default: 256 KB] Max sample size - Maximum file size for the sample, in binary units (KB, MB).
maxMetrics: # [number; default: 1000000] Total allowed number of metric series, configurable on the Leader and per Group/Fleet. The Cribl server ignores 0 or negative values. This setting impacts memory usage and CPU overhead. Setting it too high will consume too much resources (memory and CPU) and setting it too low will increase the CPU load. Adjust these settings carefully, to maintain optimal system performance.
minFreeSpace: # [string; default: 5 GB] Min free disk space - The minimum amount of disk space on the host before various features will take measures to prevent disk usage (units are KB, MB, etc.).
maxPQSize: # [string; default: 1 TB] Maximum persistent queue size per Worker Process. Sets the highest accepted value for individual integrations' PQ 'Max queue size' parameters. Consult Cribl Support before increasing the default 1 TB.
metricsGCPeriod: # [string; default: 60s] Metrics GC period - The interval on which the system attempts to free memory, by pruning stale metrics from the Stream system metrics store.
metricsMaxCardinality: # [number; default: 1000] Metrics cardinality limit - The system's allowed number of permutations of a given metric name.
metricsMaxDiskSpace: # [string; default: 64GB] Metrics max disk space - Maximum allowed disk space for persisting metrics to disk.
metricsWorkerIdBlacklist: # [array of strings] Metrics worker tracking - List of metric names for which to disable tracking of Worker Node ID. Supports wildcards. Default values: host.*, source.*, sourcetype.*, index.*.
metricsNeverDropList: # [array of strings] Metrics never-drop list - List of metric names for which to ensure delivery. Supports wildcards. Default values: total.*, system.*.
metricsFieldsBlacklist: # [array of strings] Disable field metrics - List of event fields for which to disable metric collection. Default values: host, source, sourcetype, index, project.
metricsDirectory: # [string] Metrics directory - Directory in which to store metrics on disk. Defaults to: $CRIBL_HOME/state/metrics.
cpuProfileTTL: # [string; default: 30m] CPU profile TTL - The time-to-live for collected CPU profiles.
eventsMetadataSources: # [array of strings] Event metadata sources - List of event metadata sources to enable. Defaults to an empty array.
edgeMetricsMode: # [string] Metrics to send from Edge Nodes - Choose a set of metrics for Edge Nodes to send to the Leader, or define a custom set. Default value: basic. Other predefined sets: minimal, all, custom. Details about predefined sets are at: https://docs.cribl.io/edge/internal-metrics#controlling-metrics.
kvStoreEnableWAL: # [boolean; default: false] Use Write-Ahead Log (WAL) mode – When enabled, changes to the key-value store are first written to a WAL file before being applied to the main state file, improving reliability and performance under high load conditions.
kvStoreWALMaxBytes: # [number; default: 5242880] Maximum size of the WAL file in bytes before it is merged back into the main state file. This helps manage the size of the WAL file and ensures it does not grow indefinitely.
kvStoreServicePeriod: # [number; enabled default: 60000; disabled default: 10000] Interval in milliseconds that the key-value store service runs. This controls how frequently the WAL file is merged back into the main state file and other maintenance tasks are performed.
bundleDownloadTimeoutSeconds: # [number; default: 0] Maximum time a Cribl Stream Worker will wait for a successful Leader connection before canceling a download of a new configuration bundle. This timeout helps prevent Workers from hanging indefinitely when there are network issues or other delays during the download process. A 0 value means wait indefinitely, which could cause Workers to hang.

# -------------- if edgeMetricsMode is custom ---------------

edgeMetricsCustomExpression: # [string] Metrics expression - JavaScript expression to filter metrics.

# --------------------------------------------------------

enableMetricsPersistence: # [boolean; default: true] Persist metrics - Set this to false to disable writing metrics to disk.
```
