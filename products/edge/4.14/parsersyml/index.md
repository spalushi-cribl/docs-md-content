# parsers.yml


`parsers.yml` stores configuration data for the [Knowledge > Parsers Library](parsers-library).

```yaml {title="parsers.yml"}
# Parser ID - Unique identifier for this parser
# [string; required]
id:
# Library - Library classification for the parser
# One of: cribl | cribl-custom | custom
# [string]
lib:
# Description - Description of the parser
# [string]
description:
# Tags - Tags associated with the parser
# [string]
tags:
# Type - Parser or formatter type to use
# One of: csv | elff | clf | kvp | json | delim | regex | grok
# [string; required]
type:
# Fields - Field names to extract
fields:
# Mode - Operation mode - Extract creates new fields. Reserialize extracts and filters fields, and then
# reserializes.
# One of: extract | reserialize
# [string; default: extract]
mode:
# Source field - Field containing text to be parsed
# [string; default: _raw]
srcField:
# Destination field - Destination field name
# [string]
dstField:
# Clean fields - Clean field names by replacing non [a-zA-Z0-9] characters with _
# [boolean]
cleanFields:
# Field filter expression - Field filter expression
# [string]
fieldFilterExpr:
# Keep - Field names to keep
keep:
# Remove - Field names to remove
remove:
# Regular expression - Regular expression for parsing
# [string]
regex:
# Regular expression list - List of regular expressions for parsing
regexList:
# Field name expression - Field name format expression
# [string]
fieldNameExpression:
# Pattern - Grok pattern for parsing
# [string]
pattern:
# Pattern list - List of grok patterns
patternList:
# Delimiter character - Character used to delimit fields
# [string]
delimiterChar:
# Quote character - Character used to quote literal values
# [string]
quoteChar:
# Escape character - Character used to escape the quote character in field values
# [string]
escapeChar:
# Null value - Representation of a null value. Null fields are not added to events.
# [string]
nullValue:
# Allowed key characters - Characters allowed in keys
allowedKeyChars:
# Allowed value characters - Characters allowed in values
allowedValueChars:
# Iterations - Maximum number of iterations for regex matching
# [number]
iterations:
# Overwrite - Whether to overwrite existing fields
# [boolean]
overwrite:
# Delimiter - Field delimiter for CSV parsing
# [string]
delimiter:
# Time field - Optional timestamp field name in extracted events
# [string]
timeField:
```
