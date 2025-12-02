# pack


Manages [Cribl Packs](https://docs.cribl.io/docs/packs).

**Sub-commands:**

- [`pack export`](#pack-export)
- [`pack install`](#pack-install)
- [`pack list`](#pack-list)
- [`pack uninstall`](#pack-uninstall)
- [`pack upgrade`](#pack-upgrade)

## Sub-commands and Options

### `pack export` {#pack-export}

Exports Cribl packs.

**Usage:**

```text
./cribl pack export -m merge
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-m <mode>` | Mode to export. Accepts: merge_safe, merge, default_only. |
| `-o <filename>` | Where to export the Pack on disk. |
| `-n <name>` | Name to override the installed Pack's name on export. |
| `-g <group>` | The Worker Group/Fleet to execute within. |

### `pack install` {#pack-install}

Install a Cribl Pack.

**Usage:**

```text
./cribl pack install -g default https://packs.cribl.io/dl/cribl-sysdig-events/1.0.3/cribl-sysdig-events-1.0.3.crbl
```

> In this command, `default` Worker Group ID.
>
{.box .info}

**Sample response:**

```text
{"time":"2025-11-13T08:14:05.599Z","cid":"api","channel":"ConfFeatureFlagsMgr","level":"info","message":"Flags mode","mode":"on-prem"}
{"time":"2025-11-13T08:14:05.600Z","cid":"api","channel":"ConfFeatureFlagsMgr","level":"info","message":"Final enabled feature set","enabledFeatures":["awssdk-v3-integ-aws-kms","awssdk-v3-integ-kinesis","awssdk-v3-integ-prometheus-in","awssdk-v3-integ-rpc-credentials-in","awssdk-v3-integ-s3-collector","awssdk-v3-integ-s3-in","awssdk-v3-integ-s3-in-client-cache","awssdk-v3-integ-s3-out","awssdk-v3-integ-sqs-in","awssdk-v3-integ-sqs-out","awssdk-v3-integ-vault-kms","awssdk-v3-platform-secrets-manager","awssdk-v3-platform-sns","awssdk-v3-search-lambda","awssdk-v3-search-other","awssdk-v3-search-s3","collector-azure-blob","collector-database","collector-filesystem","collector-google-cloud-storage","collector-rest","collector-s3","collector-script","collector-splunk","connection-proxy","destination-cribl-http","destination-cribl-tcp","destination-disk-spool","destination-exabeam","destination-filesystem","destination-filesystem-stage-path","destination-xsiam","edge-metricpacket-agg","feature-auto-profile","feature-custom-command","feature-full-fidelity-metrics-namespace","feature-hec-json-parser","feature-lazy-notifications","feature-leader-upgrade","feature-licensing","feature-metrics-store-track-memory-usage","feature-netflow-cached-template-manager","feature-optimize-json-stringify","feature-persistent-queue","feature-search-usage-groups","feature-splunk-v4-skip-tz-cloning","function-teefunc","jobs-service","legacy-connection-load-shedder","metadata-aws","metadata-cribl","metadata-env","metadata-kube","metadata-os","monitoring-licensing","pack-custom-function","settings-api-tls","settings-auth","settings-limits-metrics-directory","settings-limits-services-limits","settings-service-processes","settings-system-intercom","source-appscope","source-appscope-filter","source-appscope-persistence","source-azure-blob","source-confluent-cloud","source-cribl","source-cribl-http","source-cribl-tcp","source-criblmetrics","source-crowdstrike","source-edge-prometheus","source-edge-prometheus-persistence","source-eventhub","source-exec","source-file","source-google-pubsub","source-journal-files","source-kafka","source-kinesis","source-kube-events","source-kube-logs","source-kube-metrics","source-office365-mgmt","source-office365-msg-trace","source-office365-service","source-prometheus","source-s3","source-splunk-search","source-sqs","source-system-metrics","source-system-state","source-win-event-logs","source-windows-metrics","system-diagnostics","system-roles","system-users","worker-database"]}
{"time":"2025-11-13T08:14:05.661Z","cid":"api","channel":"collectors","level":"info","message":"starting lister"}
{"time":"2025-11-13T08:14:05.671Z","cid":"api","channel":"LookupDiskUsageLimiterFactory","level":"info","message":"Lookup disk usage and limits","diskUsed":30049,"diskAvailable":2147453599,"lookupMaxSize":524288000,"lookupMaxTotalSize":2147483648}
{"time":"2025-11-13T08:14:05.672Z","cid":"api","channel":"LookupDiskUsageLimiterFactory","level":"info","message":"Lookup disk usage and limits","diskUsed":30049,"diskAvailable":2147453599,"lookupMaxSize":524288000,"lookupMaxTotalSize":2147483648}
{"time":"2025-11-13T08:14:05.687Z","cid":"api","channel":"collectors","level":"info","message":"updated functions list","enabled":13,"all":13}
{"time":"2025-11-13T08:14:05.691Z","cid":"api","channel":"PackMgr","level":"info","message":"refreshing package file from disk","meta":{"name":"LogStream","version":"4.14.0-837595d5","dependencies":{"HelloPacks":"file:$CRIBL_HOME/default/HelloPacks","cribl":"file:$CRIBL_HOME/default/cribl"}}}
{"time":"2025-11-13T08:14:05.695Z","cid":"api","channel":"PackMgr","level":"info","message":"finished refreshing package file from disk","meta":{"name":"LogStream","version":"4.14.0-837595d5","dependencies":{"HelloPacks":"file:$CRIBL_HOME/default/HelloPacks","cribl":"file:$CRIBL_HOME/default/cribl"}}}
{"time":"2025-11-13T08:14:05.695Z","cid":"api","channel":"PackMgr","level":"info","message":"begin Packs install","toDo":["https://packs.cribl.io/dl/cribl-sysdig-events/1.0.3/cribl-sysdig-events-1.0.3.crbl"],"opts":{"force":false,"group":"default","allowCustomFunctions":true}}
{"time":"2025-11-13T08:14:06.330Z","cid":"api","channel":"PackMgr","level":"info","message":"installing url-based dependency","name":"cribl-sysdig-events","source":"https://packs.cribl.io/dl/cribl-sysdig-events/1.0.3/cribl-sysdig-events-1.0.3.crbl"}
https://packs.cribl.io/dl/cribl-sysdig-events/1.0.3/cribl-sysdig-events-1.0.3.crbl: successfully installed cribl-sysdig-events
{"time":"2025-11-13T08:14:06.993Z","cid":"api","channel":"PackMgr","level":"info","message":"finished installing Packs","toDo":["https://packs.cribl.io/dl/cribl-sysdig-events/1.0.3/cribl-sysdig-events-1.0.3.crbl"],"result":[{"result":{"id":"cribl-sysdig-events","source":"https://packs.cribl.io/dl/cribl-sysdig-events/1.0.3/cribl-sysdig-events-1.0.3.crbl","version":"1.0.3"},"warnings":[]}]}
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-d` | Run install in debug. |
| `-f` | Force install. |
| `-c` | Disallow installation of Packs with custom functions. |
| `-n <name>` | Name of the Pack to install, defaults to source. |
| `-g <group>` | The Worker Group/Fleet to execute within. Required for distributed deployments. |

### `pack list` {#pack-list}

Lists Cribl Packs.

**Usage:**

```text
./cribl pack list
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-v` | Display all Pack information in verbose mode. |
| `-g <group>` | The Worker Group/Fleet to execute within. |

### `pack uninstall` {#pack-uninstall}

Uninstall a Cribl Pack.

**Usage:**

```text
./cribl pack uninstall
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-d` | Run uninstall in debug. |
| `-g <group>` | The Worker Group/Fleet to execute within. |

### `pack upgrade` {#pack-upgrade}

Upgrade a Cribl Pack.

**Usage:**

```text
./cribl pack upgrade -s https://packs.cribl.io/packs/cribl-sysdig-events
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-d` | Run upgrade in debug. |
| `-s <source>` | Provide the Pack source. |
| `-m <minor>` | Only upgrade to minor version. |
| `-g <group>` | The Worker Group/Fleet to execute within. |