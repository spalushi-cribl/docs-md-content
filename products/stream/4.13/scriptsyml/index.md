# scripts.yml


`scripts.yml` stores configuration data for scripts configured at **Settings** > **Global** > **Scripts**:

```yaml {title="scripts.yml"}
script_id: # [object] 
  command: # [string] Command - Command to execute for this script
  description: # [string] Description - Brief description of this script. Optional.
  args: # [array of strings] Arguments - Arguments to pass when executing this script
  env: # [object] Env Variables - Extra environment variables to set when executing script
```
