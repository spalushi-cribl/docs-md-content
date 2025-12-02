# scripts.yml


`scripts.yml` stores configuration data for scripts configured at **Settings** > **Global** > **Scripts**:

```yaml {title="scripts.yml"}
# Script ID - Unique identifier for the script (cannot contain forward slashes)
# [string; required]
id:
# Command - Command to execute for this script
# [string; required]
command:
# Description - Description of the script
# [string]
description:
# Arguments - Arguments to pass when executing this script
args:
# Env variables - Extra environment variables to set when executing script
env:
```
