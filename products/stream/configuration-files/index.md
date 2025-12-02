# Configuration Files


Configuration files store the configuration changes locally on your system.
They reflect the changes you make to configuration settings in the UI.

You can preview how settings are translated into configuration changes in the Git Changes modal
whenever you commit your changes via the Version Control option.

![Modifications to configuration files presented as Git changes](config-files-git-changes-4.13.png)
{border="true" scale="90%" caption="Git Changes screen showing Git diff with changes to configuration files."}

Typically, you do not modify configuration files manually.
If you need to edit the configuration outside the UI, Cribl recommends using the API.
However, if your particular use case requires it, in an on-prem deployment the files are open for editing.

## Configuration File Locations

In an on-prem deployment, configuration files are stored in the `$CRIBL_HOME/local/cribl` directory
(where `$CRIBL_HOME` is your installation directory, for example, `/opt/cribl/`).
Configuration for specific Worker Groups or Fleets is located `$CRIBL_HOME/groups/<group-or-fleet-name>/local/cribl`.

You can view the default configurations by taking a look in the `default` directory, instead of `local`.

> Pack configuration resides within a separate subdirectory for each Pack: `$CRIBL_HOME/default/<pack_name>`.
{.box .info}

| Configuration file/folder | Description |
| --- | --- |
| `ai.yml` | [Cribl Copilot](/copilot/) settings.|
| `app-limits.yml` | Internal limit configuration. |
| `appscope.yml` | [Appscope](https://appscope.dev/docs/overview/) configuration. |
| `breakers-search.yml` | [Cribl Search](/search/) Event Breaker settings. |
| [`breakers.yml`](breakersyml) | Event Breakers configuration. |
| [`certificates.yml`](certificatesyml) | TLS/SSL certificates. |
| `collectors` | Directory storing individual Collector configurations. |
| `conditions` | Directory storing configurations for Notification conditions. |
| [`cribl.yml`](criblyml) | General system configuration. |
| `dataset-providers.yml` | Cribl Search Dataset Providers. |
| `datasets.yml` | Cribl Search Datasets. |
| `email-templates.yml` | Templates for [Email Notifications](email-notifications). |
| `executors` | Executor job configurations. |
| [`fleet-mappings.yml`](mappingsyml) | [Mapping rulesets](/edge/mapping-edge-nodes) for Edge Nodes. |
| `functions` | Directory storing individual Function configurations. |
| `grok-patterns` | Directory storing individual [Grok Pattern](grok-patterns-library) configurations. |
| [`groups.yml`](groupsyml) | Worker Group and Edge Fleet settings. |
| [`inputs.yml`](inputsyml) | Sources configuration. |
| [`instance.yml`](instanceyml) | Configuration for the current instance. |
| [`iometrics.yml`](/stream/iometricsyml) | Metrics levels for individual Sources and Destinations in Cribl Stream. |
| `ipfix-information-elements.yml` | Predefined IPFIX fields for handling and decoding IPFIX fields from the [NetFlow & IPFIX Source](sources-netflow). |
| [`jobs.yml`](jobsyml) | Collector configuration. |
| [`job-limits.yml`](job-limitsyml) | Parameters for Collection jobs and system tasks. |
| `kms.yml` | KMS provider configuration. |
| `lake-config.yml` | Configuration for [Cribl Lake](/lake/). |
| `lakes.yml` | Configuration for individual Cribl Lake datasets. |
| [`leader.yml`](leaderyml) | Secondary Leader configuration. |
| `licenses.yml` | Licenses. |
| [`limits.yml`](limitsyml) | Limit configuration. |
| [`logger.yml`](loggeryml) | Logging levels and redactions. |
| [`mappings.yml`](mappingsyml) | [Mapping rulesets](/stream/mapping-workers-to-worker-groups) for Stream Workers. |
| [`messages.yml`](messagesyml) | Messages displayed in the UI's `Messages` fly-out. |
| `mdt-devices.yml` | Configuration for MDT (Model Driven Telemetry) devices. |
| [`notifications.yml`](notificationsyml) | Notifications and Notification targets. |
| `notification-templates.yml` | Templates for email Notifications. |
| [`outpost.yml`](outpostyml) | Configuration for [Cribl Outpost](/edge/set-up-outpost). |
| [`outputs.yml`](outputsyml) | Destinations configuration. |
| `parquet-schemas.yml` | [Parquet Schemas](parquet-schemas). |
| `parquet-schemas` | Directory storing individual Parquet Schema configurations. |
| [`parsers.yml`](parsersyml) | [Parsers Library](parsers-library). |
| `perms.yml` | Permission configuration. |
| [`persistent-queue.yml`](persistent-queueyml) | Persistent queue configuration. |
| `pipelines` | Directory storing individual Pipeline configurations. |
| `policies.yml` | RBAC Policies. |
| `protobuf-libraries.yml` | Configuration for Protobuf libraries. |
| `redis-cache-limits.yml` | Redis cache settings. |
| `redis-limits.yml` | Redis connection settings. |
| [`regexes.yml`](regexesyml) | [Regex Library](regex-library). |
| `roles.yml` | RBAC Roles. |
| `route.yml` | Route configuration. Located in `local/cribl/pipelines/routes.yml`. |
| [`samples.yml`](samplesyml) | Metadata about about stored sample data files. |
| `saved-queries.yml` | Saved searches in Cribl Search. |
| [`schemas.yml`](schemasyml) | [Schema Library](schema-library). |
| `schemas` | Directory storing individual schema configurations. |
| [`scripts.yml`](scriptsyml) | Scripts configured at **Settings** > **Global** > **Scripts**. |
| `sds-rules.yml` | [Cribl Guard](/guard/) rules. |
| `sds-rulesets.yml` | Cribl Guard rulesets. |
| `search-limits.yml` | Limit configuration for Cribl Search. |
| `search-usage-groups` | Cribl Search Usage Group configuration. |
| `secrets.yml` | Secrets. |
| `scope_protocol.yml` | Appscope configuration for protocol detection. |
| `scope.yml` | Appscope configuration. |
| `search-limits.yml` | Cribl Search limits. |
| `search-usage-groups.yml` | Cribl Search Usage Groups. |
| [`service.yml`](serviceyml) | Service processes. |
| [`vars.yml`](varsyml) | [Variables Library](global-variables-library). |


## Configurations and Restart {#restart}

> You can **Restart** and **Reload** via the UI. In the sidebar, select **Settings**, then **Global**. Under **System** > **Controls** select **Reload** or **Restart**. 
>
{.box .info}

In a distributed environment, Worker Nodes poll the Leader for configuration changes. Many of these changes require a quick reload to read the new configuration, while others require a restart of the Cribl processes on the Worker Node.

Upon restarts, be aware of the following:
- Syslog data still being received over UDP might be dropped. 
- Worker Nodes will temporarily disappear from the Leader’s **Workers** or **Edge Nodes** page.
- Aggregation and suppression operations will start over.
- Worker Nodes' local copies of Monitoring metrics will be dropped.
- Cribl Stream will drop any events still in RAM that were bound for persistent queues. (However, PQ data already written to disk will persist through the restart.)

Changes that require reloads include configuration changes to: 
- Functions
- Pipelines
- Packs
- Routes
- Lookups
- Parquet schemas
- Global variables
- Group Settings > Limits 
- Group Settings > Logging > Levels

Changes that require restarts include configuration changes to: 
- Distributed mode (Leader versus Managed Worker Node or Single instance)
- Worker Group assignment
- Event Breakers
- QuickConnect configs
- Sources
- Destinations
- Group Settings > General Settings > TLS
- Group Settings > General Settings > Advanced
- Group Settings > Worker Processes > Process count and Memory

Some general guidelines to keep in mind: 

* Configuration changes generated by most UI interactions – for instance, changing the order of Functions in a Pipeline, or changing the order of Routes – **do not require restarts**.
* Some configuration changes in the **Settings** UI **do require restarts**. These will prompt you for confirmation before restarting.
* All direct edits of configuration files in `$CRIBL_HOME/local/` **will require restarts**. 
* Worker Nodes might temporarily disappear from the Leader's **Workers** or **Edge Nodes** tab while restarting.
* A `git commit` command on the Leader Node's host (using a freestanding `git` client not embedded in Cribl's CLI or UI) **will require either a reload or restart**. 
* When using the [Cribl App for Splunk](/stream/deploy-splunkapp), changes to Splunk configuration files might or might not require restarts. Please check current [Splunk docs](http://docs.splunk.com).  
