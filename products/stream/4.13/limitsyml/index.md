# limits.yml


`limits.yml` maintains parameters for the system. In a distributed deployment, these parameters are configured on both the Leader and the Workers. 

In the UI, you can configure them:
- On the Leader at **Settings** > **Global** > **General Settings > Limits**.
- On a single instance at **Settings** > **General Settings > Limits**.
- On Worker Groups at `<Group/Fleet‑name>` > **Group Settings** > **General Settings > Limits**.

Where we do not specify or validate a minimum or maximum value, Cribl provides a prudent default. Adjust this initial value to match your architecture and needs.

```yaml {title="limits.yml"}
defaultHeartbeatSeconds: # [number; minimum: 1] Default managed node heartbeat period - How many seconds a managed node will wait to send back a heartbeat to the Cribl control plane
samples: # [object] Samples
  maxSize: # [string] Sample size limit - Maximum file size for the sample, in binary units (KB, MB). (Max. 3MB.)
minFreeSpace: # [string] Min free disk space - The minimum amount of disk space in the system before various features will take measures to prevent disk usage (KB, MB, etc.)
maxPQSize: # [string] Worker Process PQ size limit - Highest accepted value for individual integrations' persistent queue 'Max queue size' parameters. Consult Cribl Support before increasing the default 1 TB.
metricsGCPeriod: # [string] Metrics GC period - The interval at which the system attempts to free memory by pruning stale metrics from the Stream system metrics store
maxMetrics: # [number] Number of metrics allowed - The system's total allowed number of metric series. Consult Cribl Support before updating this value.
workerMaxMetrics: # [number] Worker metrics limit - The maximum number of metric series allowed per worker. Consult Cribl Support before updating this value.
metricsMaxCardinality: # [number] Metrics cardinality limit - The system's allowed number of permutations of a given metric name
metricsMaxDiskSpace: # [string] Metrics disk space limit - Maximum allowed disk space for persisting metrics to disk
metricsWorkerIdBlacklist: # [array of strings] Metrics worker tracking - List of metric names for which to disable tracking of Worker Node ID. Supports wildcards.
metricsNeverDropList: # [array of strings] Metrics never-drop list - List metric names for which to ensure delivery. Supports wildcards.
metricsFieldsBlacklist: # [array of strings] Disable field metrics - List of event fields for which to disable metric collection
metricsDirectory: # [string] Metrics directory - Directory to store metrics on disk
cpuProfileTTL: # [string] CPU profile TTL - The time to live for collected CPU profiles
eventsMetadataSources: # [array of strings] Event metadata sources - List of event metadata sources to enable
edgeMetricsMode: # [string] Metrics to send from Edge Nodes - Choose a set of metrics for Edge Nodes to send to the Leader, or define a custom set. Details about pre-defined sets are in [the docs](https://docs.cribl.io/edge/internal-metrics#controlling-metrics).

# -------------- if edgeMetricsMode is custom ---------------

edgeMetricsCustomExpression: # [string] Metrics expression - JavaScript expression to filter metrics

# --------------------------------------------------------

enableMetricsPersistence: # [boolean] Persist metrics - Disable to stop writing metrics to disk
edgeNodesCount: # [number; maximum: 250000] Edge Nodes estimated count - Used to pre-allocate connection handlers. Changing this value could restart currently running connection handlers, causing all Edge Nodes to reconnect. A blank entry applies the 0 default, which starts a handler on demand when an Edge Node attempts to connect.
enableWorkerPersistence: # [boolean] Persist Nodes - Persist Node connection data across restarts.  Caution: Leader will restart when this setting is changed.
kvStoreEnableWAL: # [boolean] Enable KVStore WAL - Enable the write-ahead log for the KVStore

# -------------- if kvStoreEnableWAL is true ---------------

kvStoreWALMaxBytes: # [number; minimum: 1024] KVStore WAL file bytes limit - The amount of bytes to allow a WAL file to get to before committing the WAL back into the KVStore. This may result in performance degradation if adjusted.

# --------------------------------------------------------

kvStoreServicePeriod: # [number; minimum: 10000] KVStore persist period - How often the KVStore syncs itself to disk. When WAL mode is enabled, deltas to the KVStore are synced to disk immediately as they happen; however, the WAL is merged back into the overall KVStore on this interval.
bundleDownloadTimeoutSeconds: # [number] Config bundle download timeout (secs) - The maximum time a Worker will wait for a successful Leader connection before canceling a download of a new configuration bundle. A 0 value means wait indefinitely, which could cause Workers to hang.
enableProcessInfoUsingWindowsTools: # [boolean] Use Windows tools to collect process info - Enable PowerShell to collect process information instead of using the native API
lookupMaxSize: # [string] Lookup file max size - Maximum allowed disk space for persisting single lookup data file
lookupMaxTotalSize: # [string] Lookup files disk limit - Maximum allowed disk space for persisting lookup data and metadata files
disableMetricsAccessorCache: # [boolean] Disable metrics accessor cache - Advanced setting - do not change without consulting Cribl Support. Disable the metrics accessor cache
```
