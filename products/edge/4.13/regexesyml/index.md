# regexes.yml


`regexes.yml` maintains a list of regexes. Cribl's [Regex Library](regex-library) ships under `default`. Each regex is listed according to the following pattern:

```yaml {title="regexes.yml"}
regex_id: # [object] 
  lib: # [string] Library
  description: # [string] Description - Brief description of this regex. Optional.
  regex: # [string] Regex pattern - Regex pattern. Required.
  sampleData: # [string] Sample data - Sample data for this regex. Optional.
  tags: # [string] Tags - One or more tags related to this regex. Optional.
```
