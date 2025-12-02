# mappings.yml and fleet-mappings.yml


Mapping ruleset configurations for Stream Workers and Edge Node
are stored in `$CRIBL_HOME/local/cribl/mappings.yml` and `$CRIBL_HOME/local/cribl/fleet-mappings.yml`, respectively.

```yaml {title="mappings.yml or fleet-mappings.yml"}
# Mapping ID - Unique identifier for the mapping ruleset
# [string; required]
id:
# Configuration - Mapping ruleset configuration
conf:
  # Functions - List of functions to pass data through
  functions:
# Active - Whether this mapping ruleset is active
# [boolean]
active:
```
