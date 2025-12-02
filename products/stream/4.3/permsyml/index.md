# perms.yml


`perms.yml` stores permissions configured for Cribl Stream users.

```yaml title="$CRIBL_HOME/local/cribl/users/stream/perms.yml"
perms:
  - gid:    # [string]
    id:     # [string] Not used if type is 'groups'.
    type:   # [string] Resource type, one of: 'groups' | 'datasets' | 'dataset-providers' | 'projects'.
    policy: # [string] Policy name.
```