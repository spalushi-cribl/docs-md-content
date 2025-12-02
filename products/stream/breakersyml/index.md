# breakers.yml


Event Breaker configuration is stored in `breakers.yml`.

```yaml {title="breakers.yml"}
# ID - ID can contain only letters (A-Z, a-z), numbers (0-9), dashes, underscores, and spaces
# [string; required]
id:
# Library - Library classification for the ruleset
# One of: custom | cribl-custom
# [string; default: custom]
lib:
# Description - Description of the event breaker ruleset
# [string]
description:
# Tags - Tags associated with the ruleset
# [string]
tags:
# Min raw length - The minimum number of characters in _raw to determine which rule to use
# [number; min: 50; max: 100000; default: 1000]
minRawLength:
# Rules - A list of rules that will be applied, in order, to the input data stream
rules:
  # Name - Name of the rule
  # [string; required]
  name:
  # Filter condition - JavaScript expression applied to the beginning of a file or object, to determine
  # whether the rule applies to all contained events.
  # [string; required]
  condition:
  # Event Breaker type - Type of event breaker to use
  # One of: regex | json | json_array | header | timestamp | csv | aws_cloudtrail | aws_vpcflow
  # [string; required]
  type:
  # Index - Rule index for ordering
  # [number]
  index:
  # Timestamp anchor - The regex to match before attempting timestamp extraction. Use $ (end-of-string
  # anchor) to prevent extraction.
  # [string; required]
  timestampAnchorRegex:
  # Default timezone - Default timezone for timestamp parsing
  # [string; required]
  timestampTimezone:
  # Earliest timestamp - Earliest allowable timestamp
  # [string]
  timestampEarliest:
  # Latest timestamp - Latest allowable timestamp
  # [string]
  timestampLatest:
  # Timestamp configuration - Timestamp extraction configuration
  # [required]
  timestamp:
    # Timestamp type - Type of timestamp extraction
    # One of: auto | format | current
    # [string; required]
    type:
    # Timestamp length - Length of timestamp to extract
    # [number]
    length:
    # Timestamp format - Format string for timestamp parsing
    # [string]
    format:
  # Fields - Field definitions for extraction
  fields:
  # Event byte limit - The maximum number of bytes that an event can be before being flushed
  # [number; required]
  maxEventBytes:
  # Disabled - Whether this rule is disabled
  # [boolean]
  disabled:
  # Parser enabled - Whether to enable parser for this rule
  # [boolean]
  parserEnabled:
  # Parser configuration - Parser settings for this rule
  parser:
  # Should use data raw - Use the field in the data called _raw for post processors
  # [boolean]
  shouldUseDataRaw:
  # Event Breaker - The regex used to break the stream into events at the beginning of the match (regex type only)
  # [string]
  eventBreakerRegex:
  # JSON Array Field - Name of the Array field to parse events from (json_array type only)
  # [string]
  jsonArrayField:
  # JSON Extract All - Whether json_array breaker should extract fields (json_array type only)
  # [boolean]
  jsonExtractAll:
  # JSON Time Field - Name of the field to use as _time (json_array type only)
  # [string]
  jsonTimeField:
  # Parent Fields to Copy - List of fields to copy from parent to produced events (json_array type only)
  parentFieldsToCopy:
  # Field delimiter - Field delimiter regex (header type only)
  # [string]
  delimiterRegex:
  # Fields regex - Regex with one capturing group, capturing all the fields (header type only)
  # [string]
  fieldsLineRegex:
  # Header line - Regex matching a file header line (header type only)
  # [string]
  headerLineRegex:
  # Clean fields - Clean field names by replacing non [a-zA-Z0-9] characters with _ (header type only)
  # [boolean]
  cleanFields:
  # Null value - Representation of a null value (header type only)
  # [string]
  nullFieldVal:
  # Delimiter - Field delimiter character for CSV parsing (csv type only)
  # [string]
  delimiter:
  # Quote char - Character used to quote literal values (csv type only)
  # [string]
  quoteChar:
  # Escape char - Character used to escape the quote character in field values (csv type only)
  # [string]
  escapeChar:
  # Time field - Optional timestamp field name in extracted events (csv type only)
  # [string]
  timeField:
```
