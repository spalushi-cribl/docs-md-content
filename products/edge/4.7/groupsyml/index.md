# groups.yml


`groups.yml` maintains a list of groups and their configuration versions.

```yaml {title="$CRIBL_HOME/local/cribl/groups.yml"}
group_id: # [object] 
  configVersion: # [string] Config Version - Configuration version that is active for this group
  onPrem: # [boolean] On prem - Whether the group accepts on-prem or cloud worker nodes (this field is only available on cloud deployments)
  isFleet: # [boolean] Is Fleet - Fleet groups only manage edge nodes.
  workerRemoteAccess: # [boolean] UI access - Enable authenticated viewing of Workers' UI from the Leader.
```
