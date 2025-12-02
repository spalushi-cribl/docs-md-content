# parsers.yml


`parsers.yml` stores configuration data for the [Knowledge > Parsers Library](parsers-library).

```yaml {title="$CRIBL_HOME/default/cribl/parsers.yml"}
parser_id: # [object] 
  lib: # [string] Library
  description: # [string] Description - Brief description of this parser. Optional.
  tags: # [string] Tags - One or more tags related to this parser. Optional.
  type: # [string] Type - Parser/Formatter type to use.
  fields: # [array of strings] List of Fields - Fields expected to be extracted, in order. If not specified parser will auto-generate.
```
