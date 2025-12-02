# breakers.yml


Cribl's default Event Breaker Library is stored in `$CRIBL_HOME/default/cribl/breakers.yml`.

```yaml {title="$CRIBL_HOME/default/cribl/breakers.yml"}
breaker_id: # [object] 
  lib: # [string] Library
  description: # [string] Description - Brief description of this ruleset. Optional.
  tags: # [string] Tags - One or more tags related to this ruleset. Optional.
  rules: # [array] Rules - List of rules. Evaluated in order, top down.
    - name: # [string] Rule Name - Rule Name.
      condition: # [string] Filter Condition - Filter expression (JS) that matches data to apply rule to. To test your sample, use the maximize icon on the right.
      type: # [string] Event Breaker Type - Event Breaker Type
      timestampAnchorRegex: # [string] Timestamp Anchor - Regex to match before attempting timestamp extraction. Use $ (end of string anchor) to not perform extraction.
      timestamp: # [object] Timestamp Format - Auto, manual format (strptime) or current time.
        type: # [string] Timestamp Type
        length: # [number] Length
        format: # [string] Format
      timestampTimezone: # [string] Default timezone - Timezone to assign to timestamps without timezone info.
      timestampEarliest: # [string] Earliest timestamp allowed - The earliest timestamp value allowed relative to now. E.g., -42years. Parsed values prior to this date will be set to current time.
      timestampLatest: # [string] Future timestamp allowed - The latest timestamp value allowed relative to now. E.g., +42days. Parsed values after this date will be set to current time.
      maxEventBytes: # [number] Max Event Bytes - The maximum number of bytes that an event can be before being flushed to the pipelines
      fields: # [array] Fields - Key value pairs to be added to each event.
      - name: # [string] Name - Field Name.
        value: # [string] Value Expression - JavaScript expression to compute fields value (can be constant).
      disabled: # [boolean] Disabled - Allows breaker rule to be enabled or disabled, default is enabled.
```


