# parse_csv


The `parse_csv` function splits a given string representing a single record of comma-separated values and returns a string array with these values.

## Syntax

    `parse_csv( Source )`

### Arguments

* **Source**: The source string representing a single record of comma-separated values.

## Results

A string array that contains the split values.

Embedded line feeds, commas, and quotes may be escaped using the double quotation mark `""`. This function doesn't support multiple records per row (only the first record is taken).

## Examples

This example splits a simple comma-separated phrase to a simple array:

```kusto {runSearch=true}
print theData="Hello, world" | extend parsed_data=parse_csv(theData) | render event
```

This example:

```kusto
parse_csv('aa,"b,b,b",cc,"Escaping quotes: ""Title""","line1\nline2"')
```

...returns:

```
[
  "aa",
  "b,b,b",
  "cc",
  "Escaping quotes: "Title"",
  "line1\nline2"
]
```

This example:

```kusto
parse_csv('record1,a,b,c\nrecord2,x,y,z')
```

...returns:

```
[
  "record1",
  "a",
  "b",
  "c"
]
```
