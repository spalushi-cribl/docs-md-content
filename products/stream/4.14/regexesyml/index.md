# regexes.yml


`regexes.yml` maintains a list of regexes. Cribl's [Regex Library](regex-library) ships under `default`. Each regex is listed according to the following pattern:

```yaml {title="regexes.yml"}
# Regex ID - Unique identifier for the regex pattern
# [string; required]
id:
# Library - Library classification for the regex
# One of: cribl | cribl-custom | custom
# [string]
lib:
# Description - Description of the regex pattern
# [string]
description:
# Regex pattern - The regular expression pattern
# [string; required]
regex:
# Sample data - Optionally, paste in sample data to match against this regex
# [string]
sampleData:
# Tags - Tags associated with the regex
# [string]
tags:
```
