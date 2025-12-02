# limits.yml


`limits.yml` maintains parameters for the system. In a distributed deployment, these parameters are configured on both the Leader and the Workers. 

In the UI, you can configure them:
- On the Leader at **Settings** > **Global** > **General Settings > Limits**.
- On a single instance at **Settings** > **General Settings > Limits**.
- On Fleets at `<Group/Fleet‑name>` > **Fleet Settings** > **General Settings > Limits**.

Where we do not specify or validate a minimum or maximum value, Cribl provides a prudent default. Adjust this initial value to match your architecture and needs.

```yaml {title="limits.yml"}
# Default managed node heartbeat period - How many seconds a managed node will wait to send back a heartbeat to
# the Cribl control plane
# [number; min: 1]
defaultHeartbeatSeconds:
# Samples
samples:
  # Sample size limit - Maximum file size for the sample, in binary units (KB, MB). (Max. 3MB.)
  # [string]
  maxSize:
# Min free disk space - The minimum amount of disk space in the system before various features will take
# measures to prevent disk usage (KB, MB, etc.)
# [string; default: 5 GB]
minFreeSpace:
# Worker Process PQ size limit - Highest accepted value for individual integrations' persistent queue 'Max
# queue size' parameters. Consult Cribl Support before increasing the default 1 TB.
# [string; default: 1TB]
maxPQSize:
# Metrics GC period - The interval at which the system attempts to free memory by pruning stale metrics from
# the Stream system metrics store
# [string; default: 60s]
metricsGCPeriod:
# Number of metrics allowed - The system's total allowed number of metric series. Consult Cribl Support
# before updating this value.
# [number; default: 1000000]
maxMetrics:
# Worker metrics limit - The maximum number of metric series allowed per worker. Consult Cribl Support
# before updating this value.
# [number; default: 100000]
workerMaxMetrics:
# Metrics cardinality limit - The system's allowed number of permutations of a given metric name
# [number; default: 1000]
metricsMaxCardinality:
# Metrics disk space limit - Maximum allowed disk space for persisting metrics to disk
# [string; default: 64GB]
metricsMaxDiskSpace:
# Metrics worker tracking - List of metric names for which to disable tracking of Worker Node ID.
# Supports wildcards.
# [array; default: host.*, source.*, sourcetype.*, index.*]
metricsWorkerIdBlacklist:
# Metrics never-drop list - List metric names for which to ensure delivery. Supports wildcards.
# [array; default: total.*]
metricsNeverDropList:
# Disable field metrics - List of event fields for which to disable metric collection
# [array; default: source, host, index, sourcetype]
metricsFieldsBlacklist:
# Metrics directory - Directory to store metrics on disk
# [string; default: $CRIBL_STATE_DIR/metrics]
metricsDirectory:
# CPU profile TTL - The time to live for collected CPU profiles
# [string; default: 30m]
cpuProfileTTL:
# Event metadata sources - List of event metadata sources to enable
eventsMetadataSources:
# Metrics to send from Edge Nodes - Choose a set of metrics for Edge Nodes to send to the Leader, or define
# a custom set. Details about pre-defined sets are in [the docs](https://docs.cribl.io/edge/internal-metrics#controlling-metrics).
# One of: basic | detailed | none | custom
# [string; default: basic]
edgeMetricsMode:
# Metrics expression - JavaScript expression to filter metrics
# [string; required]
edgeMetricsCustomExpression:
# Persist metrics - Disable to stop writing metrics to disk
# [boolean; default: true]
enableMetricsPersistence:
# Edge Nodes estimated count - Used to pre-allocate connection handlers. Changing this value could restart
# currently running connection handlers, causing all Edge Nodes to reconnect. A blank entry applies the 0
# default, which starts a handler on demand when an Edge Node attempts to connect.
# [number; min: 0; max: 250000; default: 0]
edgeNodesCount:
# Persist Nodes - Persist Node connection data across restarts. Caution: Leader will restart when this
# setting is changed.
# [boolean; default: false]
enableWorkerPersistence:
# Enable KVStore WAL - Enable the write-ahead log for the KVStore
# [boolean; default: false]
kvStoreEnableWAL:
# KVStore persist period - How often the KVStore syncs itself to disk. When WAL mode is enabled, deltas to
# the KVStore are synced to disk immediately as they happen; however, the WAL is merged back into the overall
# KVStore on this interval.
# [number; min: 10000]
kvStoreServicePeriod:
# KVStore WAL file bytes limit - The amount of bytes to allow a WAL file to get to before committing the WAL
# back into the KVStore. This may result in performance degradation if adjusted.
# [number; min: 1024; default: 5242880]
kvStoreWALMaxBytes:
# Config bundle download timeout (secs) - The maximum time a Worker will wait for a successful Leader
# connection before canceling a download of a new configuration bundle. A 0 value means wait indefinitely,
# which could cause Workers to hang.
# [number; min: 0; default: 0]
bundleDownloadTimeoutSeconds:
# Minimum reconnect interval - The minimum time to wait before attempting to reconnect to the Leader after
# being disconnected. The interval is doubled with each failed reconnect attempt until the maximum limit is
# reached.
# [string; default: 2s]
minReconnectInterval:
# Maximum reconnect interval - The maximum time to wait before attempting to reconnect to the Leader after
# being disconnected.
# [string; default: 1m]
maxReconnectInterval:
# Random reconnect interval - Upper limit on the random time added to each reconnect interval. Set this to a
# non-zero interval to randomly spread out reconnection attempts. Useful for Leaders with a large number of
# Worker or Edge nodes.
# [string; default: 0s]
randomReconnectInterval:
# Use Windows tools to collect process info - Enable PowerShell to collect process information instead of
# using the native API
# [boolean; default: false]
enableProcessInfoUsingWindowsTools:
# Lookup file max size - Maximum allowed disk space for persisting single lookup data file
# [string; default: 500MB]
lookupMaxSize:
# Lookup files disk limit - Maximum allowed disk space for persisting lookup data and metadata files
# [string; default: 2GB]
lookupMaxTotalSize:
# Disable metrics accessor cache - Consult Cribl Support before adjusting this advanced setting. Disable
# the metrics accessor cache
# [boolean; default: false]
disableMetricsAccessorCache:
# Large events threshold - Consult Cribl Support before adjusting this advanced setting. Threshold for
# tracking large events in metrics. Events larger than this size will be tracked separately.
# [string; default: 1MB]
largeEventsThreshold:
# NetFlow template flush interval - Consult Cribl Support before adjusting this advanced setting. It
# determines how often the NetFlow template service sends templates to the central KVStore. The default value
# is 250ms. Lower values speed up template propagation but increase the RPC load on the central KVStore.
# [number; min: 1; default: 250]
netFlowTemplateFlushInterval:
```
