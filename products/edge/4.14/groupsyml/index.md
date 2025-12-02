# groups.yml


`groups.yml` maintains a list of groups and their configuration versions.

```yaml {title="groups.yml"}
# Worker Group type - Whether this group accepts Hybrid (on-prem/customer-managed) nodes, or only
# Cribl-managed Cribl.Cloud nodes. (This field is only available in Cribl.Cloud.)
# [boolean; default: true]
onPrem:
# ID - Unique identifier for the group
# [string; required]
id:
# Enable teleporting to nodes - Enable remote access to nodes in this group
# [boolean; default: false]
workerRemoteAccess:
# Description - Optional description for the group
# [string]
description:
# Config version - The active configuration version for this group
# [string]
configVersion:
# Tags - Tags associated with this group
streamtags:
# Cloud settings
cloud:
# Name - Display name for the group
name:
# Provisioned - Whether the group is provisioned
provisioned:
# Is Fleet - Fleets manage only Edge Nodes.
# [boolean; default: false]
isFleet:
# Is Search group - Search groups manage search configuration.
# [boolean; default: false]
isSearch:
# Upgrade version - Upgrade version to follow for this group
# [string]
upgradeVersion:
# Estimated ingest rate
estimatedIngestRate:
# Max worker age
maxWorkerAge:
```
