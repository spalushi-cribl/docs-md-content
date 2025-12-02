# extract


The `extract` operator extracts information from a field (which defaults to `_raw`), using either a predefined [Parser](parsers) or various `Type` options (such as `delimited`, `regex`, `kvp`, `json`, `csv`, `elff`, or `clf`).

> To extract a substring from a source string, using an RE2 regular expression, see the `extract` scalar string [function](extract).
{.box .info}

## Syntax

<div style={{paddingLeft: "2%"}}>
    <code>... | extract [ SourceOption ] [ type=Type | parser=Parser ]</code>
    <ul>
      <li>
        <code>[ type=delimited [ DelimOption ] FieldList ]</code>
      </li>
      <li>
        <code>[ [ type=regex ] [ RegexOption ] RegexLiteral [RegexLiteral[...]] ] </code>
      </li>
      <li>
        <code>[ type=keyvalue [ FieldList ]]</code>
      </li>
      <li>
        <code>[ type=json [ FieldList ]]</code>
      </li>
      <li>
        <code>[ type=csv FieldList ]</code>
      </li>
      <li>
        <code>[ type=elff FieldList ]</code>
      </li>
      <li>
        <code>[ type=clf [ FieldList ]]</code>
      </li>
    </ul>
</div>

### Rules 

This operator automatically detects the `type` when you define **RegexLiteral** or **DelimOption**. For example, if you provide **RegexLiteral**, it's assigned as `type=regex`. Or, if you specify **DelimOption**, it's assigned as `type=delimited`.

You can define either **DelimOption** or **RegexOption**, not both.

You can have multiple **FieldList**s. These will be merged into one larger field list.

A **FieldList** is optional for the key value, JSON, and CLF types. If no **FieldList** is specified: 

- Key value and JSON types extract all fields.

- CLF type extracts `clientip`, `ident`, `user`, `timestamp`, `request`, `status`, and `bytes`.

### Arguments

Here are syntax details and options for each of this operator's arguments.

#### SourceOption 

Syntax – `[ SourceIdent=FieldName ]`. 

Defaults to `source=_raw`.

- **SourceIdent**: Define as `source`, `sourceField`, or `src`.

- **FieldName**: Name of the field to extract from. Case sensitive.

#### Type

Syntax – `[ Type=TypeName ]`. Example: `type=csv`

- **Type**: Define as either `type` or `format`.

- **TypeName**: See the following table.

**TypeName** | Define as
--- | ---
Delimiter | `delimited`, `del`, or `delim`
Regex | `regex`, `regexp`, or `re`
Key Value | `kvp`, `keyvalue`, or `kv`
JSON | `json` or `js`
CSV | `csv`
ELF | `elff` or `extended`
CLF | `clf` or `common` (case insensitive)

#### FieldList

Syntax – `"FieldName" [, "FieldNameList" ]`. 

Case-sensitive. Example: `"field1,field2"`

Alternative syntax – `FieldIdent="FieldNameList"`. 

Example: `fields=field1,field2`

- **FieldIdent**: Define as either `field` or `fields`.

- **FieldNameList**: Syntax – `FieldName [, FieldNameList ]`. Case sensitive.

#### Parser

Syntax – `parser="KnownParserName"`. 

Use a predefined [Parser](parsers). Case-sensitive.

#### RegexLiteral 

Syntax – `@'NamedRegularExpression'`. 

At least one named capture group is required. Example: `@'metric1=(?<metric1>\d+)'` 
  
To extract key-value pairs and assign them to fields, use the capture group names `_NAME_0` and `_VALUE_0`. Example: `@'\"(?<_NAME_0>\w+)\s\/(?<_VALUE_0>.*)\"'`

Alternative syntax – `RegexIdent=@'NamedRegularExpression'`. 

Example: `regex=@'metric1=(?<metric1>\d+)'`

- **RegexIdent**: Define as either `regex` or `regexp`.

#### DelimOption

Syntax – `[[ DelimCharOpt ] [ EscapeCharOpt ] [ QuoteCharOpt] [ NullValOpt ]]`

Argument | Define as |  Syntax | Default Value | Example
--- | --- | --- | --- | ---
DelimCharOpt | `delimchar`, `delimiter`, or `delim` | `DelimCharOpt=char` | `;` | `delimiter=:`
EscapeCharOpt | `escapechar`, `escape`, or `esc` | `EscapeCharOpt=char` | `\` | `esc=\`
QuoteCharOpt | `quotechar` or `quote` | `QuoteCharOpt=char` | `"` | `quotechar='`
NullValOpt |  `nullvalue` or `undefined` | | |

#### RegexOption

Syntax – `[[ IterationOpt ] [ OverwriteOpt ]]`
    
Argument | Define as | Syntax | Default Value | Example
--- | --- | --- | --- | ---
IterationOpt |  `numiterations`, `iterations`, or `iter` | `IterationOpt=integer` | `100` | `iterations=11`
OverwriteOpt | `overwrite` or `ow` | `OverwriteOpt=boolean` | `false` | `ow=true` 

## Examples

This example extracts the domain name from an email address:

```kusto {runSearch=true}
dataset=$vt_dummy event<10 
| extend email="test@example.com" 
| extract source=email @'@(?<domainName>.*$)'
```

This example extracts delimited field values:

```kusto {runSearch=true}
dataset=$vt_dummy event<10 
| extend myField="value1|value2|value3" 
| extract source=myField type=delim delimChar="|" "field1,field2,field3"
```

This example extracts and names four fields from the CSV-formatted source field `foobar`:

```kusto {runSearch=true}
dataset=$vt_dummy events < 10
| extend foobar="foo,bar,baz,quux" 
| extract source=foobar type=csv "field1,field2,field3,field4"
```

This example extracts all key-value pairs from the default source field `_raw`:

```kusto
dataset=myDataset
| extract type=keyvalue
```

This example extracts all key-value pairs from the named source field `payload`:

```kusto {runSearch=true}
dataset=$vt_dummy events<10
| extend payload="time=now key=value message=\"many words here\""
| extract source=payload type=keyvalue
```

This examplee uses a Parser to extract all fields from `mySource`:

```kusto
dataset=myDataset
| extract source="mySource" parser="Apache Common Log Format"
```

This example extracts delimited values (from `_raw`) using the `|` as the delimiter. The `type` specification could be omitted in this case, because the `type` is implicitly given by specifying the `delimChar` option:

```kusto
dataset=myDataset
| extract type=delim delimChar="|" "field1,field2"
```

This example extracts the fields `metric1` and `foo` from `_raw` (default source) via named regular expressions. The `type` specification could be omitted in this case, because the `type` is implicitly given by specifying regular expressions.

```kusto
dataset=myDataset
| extract type=regex @'metric1=(?<foo>\d+)'
```
