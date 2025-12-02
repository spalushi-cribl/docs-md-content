# Cribl Edge 4.5.1


> If the auth token for your Leader Node contains special characters, you
> must update the token to contain only allowed characters. Starting in version
> 4.5.1, check your auth token for any disallowed characters before you upgrade
> Cribl Edge to 4.5.1 and newer. The auth token can only contain these allowed
> characters:

> - a-z: Lowercase letters from a to z. 
> - A-Z: Uppercase letters from A to Z.
> - 0-9: Digits from 0 to 9. 
> - _: Underscore character. 
> 
> See [How to Secure the Auth Token for the Leader Node](securing-auth-token#changing-the-auth-token-value) for instructions.
{.box .warning}

## Corrections

- The [Kubernetes Logs Source](sources-kubernetes-logs) in Edge now successfully
  retrieves logs even when the kubelet responds with a connection reset. This
  issue was happening for several reasons, including when the kubelet restarted
  gracefully. CRIBL-22895

- We fixed a [Windows Event Log Source](sources-windows-event-logs) memory leak. CRIBL-23162

- [Fleet-wide log searches](monitoring#log_search) (**Manage > Logs**) are disabled in the Leader UI for
  Fleets with [**Disable Jobs/Tasks**](settings-group-fleet#job-limits) turned on.
  Log searches across Fleets are conducted by issuing jobs across all members of
  the Fleet, and these searches don’t work when job/task execution is disabled.
  CRIBL-22716

- Since the **Enable automatic upgrades** and **Enable Legacy Edge upgrades** toggles
  don’t make sense in the context of standalone Edge instances, we’ve removed them
  as options in **Settings > Global Settings > System > Upgrade** for standalone
  instances of Edge. CRIBL-22766

- You can now search the Node to explore field on the [Edge **Explore** tab](explore-edge#explore-tab) by hostname
  or GUID. To help improve UI responsiveness when there are a large number of
  Nodes in the list, the initial results in the drop-down are limited to 50 Nodes,
  listed by hostname in alphabetical order. CRIBL-23232

- The [**Manage > Mappings**](mapping-edge-nodes) page is limited to 10,000 results. CRIBL-14454 

- In the [**Status** tab](monitoring#source-and-destination-health-status) of a
  Source/Destination, the **Status** column has been removed for any Source or
  Destination whose Leaders have more than 50 Nodes connected. Instead, expand an
  individual Node to view its status. CRIBL-23203

- On Windows instances of Edge, the [`CRIBL_VOLUME_DIR` environment variable](environment-variables#single-instance-deployment-variables) is  now
  defined in the `cribl` service process instead of globally. Check any shell
  commands that rely on the `CRIBL_VOLUME_DIR` environment variable, such as when
  gathering diags and heap snapshots via CLI, as they may be affected. CRIBL-18897

## Shared Corrections

Stream and Edge share the following corrections:

- Auth tokens that contain special characters are now properly flagged as not
  allowed when you're deploying managed Edge Nodes using the
  `CRIBL_DIST_MASTER_URL` (for example, in containers). CRIBL-17047

- [Cribl TCP](sources-cribl-tcp), [Cribl HTTP](sources-cribl-http), and [TCP JSON](sources-tcp-json) Sources and Destinations now respond with
  more meaningful messages in case of connection problems. CRIBL-16537

### Sources

- You can now configure **Fields** in the [Windows Event Forwarder Source](sources-wef) in
  Subscriptions and Processing Settings. CRIBL-22891

- [Splunk HEC](sources-splunk-hec), [HTTP/S](sources-raw-http), and
  [Elasticsearch API](sources-elastic) Sources now log time-based metrics about
  incoming requests and ingested events. CRIBL-20638

- In the Raw HTTP Source you can now add request headers to events, in the
  `__headers` field. CRIBL-23106

- Fixed a bug in the [Raw HTTP](sources-raw-http) Source where the `event.headers` field only
  replaced the first hyphen in a header name with an underscore instead of the
  intended behavior of replacing all hyphens with underscores. CRIBL-23106

- [Syslog Source](sources-syslog) now correctly respects the Default timezone setting. CRIBL-23031

- [Cribl TCP](sources-cribl-tcp), [TCP JSON](sources-tcp-json), and
  [AppScope](sources-appscope) Sources now log stats once per minute to the
  `metrics.log` file. CRIBL-21742

### Destinations

- The [Amazon S3](destinations-s3) Destination offers an additional storage
  class: Glacier Instant Retrieval. Other storage classes are renamed for
  clarity and to match the names used by S3: “Glacier” is now “Glacier Flexible
  Retrieval”, while “Deep Archive” changed to “Glacier Deep Archive”.
  CRIBL-21619

- [Kafka](destinations-kafka) Destination now outputs a timestamp field based on
  `__kafkaTime`. If `__kafkaTime` is not present, the timestamp defaults to
  `Date.now()`. CRIBL-22966
