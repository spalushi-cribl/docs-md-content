# parsers.yml


`parsers.yml` stores configuration data for the [Knowledge > Parsers Library](parsers-library).

```yaml {title="parsers.yml"}
parser_id: # [object] 
  lib: # [string] Library
  description: # [string] Description
  tags: # [string] Tags - Optionally, add tags that you can use for filtering
  type: # [string] Type - Parser or formatter type to use

  # -------------- if type is kvp ---------------

  cleanFields: # [boolean] Clean fields - Clean field names by replacing non [a-zA-Z0-9] characters with _
  allowedKeyChars: # [array of strings] Allowed key characters - A list of characters that may be present in a key name, even though they are normally separator or control characters
  allowedValueChars: # [array of strings] Allowed value characters - A list of characters that may be present in a value, even though they are normally separator or control characters

  # --------------------------------------------------------


  # -------------- if type is delim ---------------

  delimChar: # [string] Delimiter - Delimiter character to use to split values
  quoteChar: # [string] Quote char - Character used to quote literal values
  escapeChar: # [string] Escape char - Escape character used to escape delimiter or quote character
  nullValue: # [string] Null value - Field value representing the null value. Null fields will be omitted.

  # --------------------------------------------------------


  # -------------- if type is csv ---------------


  # --------------------------------------------------------


  # -------------- if type is json ---------------


  # --------------------------------------------------------


  # -------------- if type is regex ---------------

  regex: # [string] Regex - Regex literal with named capturing groups, such as (?<foo>bar), or _NAME_ and _VALUE_ capturing groups, such as(?<_NAME_0>[^ =]+)=(?<_VALUE_0>[^,]+)
  regexList: # [array] Additional regex
    - regex: # [string] Regex - Regex literal with named capturing groups, such as (?<foo>bar), or _NAME_ and _VALUE_ capturing groups, such as (?<_NAME_0>[^ =]+)=(?<_VALUE_0>[^,]+)
  iterations: # [number; minimum: 1] Max exec - The maximum number of times to apply regex to source field when the global flag is set, or when using _NAME_ and _VALUE_ capturing groups
  fieldNameExpression: # [string] Field name format expression - JavaScript expression to format field names when _NAME_n and _VALUE_n capturing groups are used. Original field name is in global variable 'name'. Example: To append XX to all field names, use `${name}_XX` (backticks are literal). If empty, names will be sanitized using this regex: /^[_0-9]+|[^a-zA-Z0-9_]+/g. You can access other fields values via __e.<fieldName>.
  overwrite: # [boolean] Overwrite existing fields - Overwrite existing event fields with extracted values. If disabled, existing fields will be converted to an array.

  # --------------------------------------------------------


  # -------------- if type is grok ---------------

  pattern: # [string] Pattern - Grok pattern to extract fields. Syntax supported: %{PATTERN_NAME:FIELD_NAME}
  patternList: # [array] Additional Grok patterns
    - pattern: # [string] Pattern - Grok pattern to extract fields. Syntax supported: %{PATTERN_NAME:FIELD_NAME}

  # --------------------------------------------------------

```
