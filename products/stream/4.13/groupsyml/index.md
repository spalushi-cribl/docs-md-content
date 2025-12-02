# groups.yml


`groups.yml` maintains a list of groups and their configuration versions.

```yaml {title="groups.yml"}
group_id: # [object] 
  configVersion: # [string] Config Version - Configuration version that is active for this group
  isFleet: # [boolean] Is Fleet - Fleet groups only manage edge nodes.
  workerRemoteAccess: # [boolean] UI access - Enable authenticated viewing of Workers' UI from the Leader.
  description: # [string] Description
  streamtags: # [array of strings] Tags
  isSearch: # [boolean] Is Search group - Search groups manage search configuration.
  upgradeVersion: # [string] Upgrade version - Upgrade version to follow for this Worker Group
  maxWorkerAge: # [string] Time to keep disconnected Worker Nodes - Maximum time to keep disconnected Worker Nodes before deleting. Format examples: 8h, 5d, 1w. Defaults to days if not specified. If left blank, defaults to 1 day.
  lookupDeployments: # [array] Deployment status for disk-based lookups
    - context: # [string]
      lookups:
        - file: # [string]
          deployedVersion: # [string]
  deployingWorkerCount: # [number]
  workerCount: # [number]
```
