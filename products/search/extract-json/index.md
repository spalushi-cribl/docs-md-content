# extract_json



The `extract_json` function gets a specified element out of a JSON text using a path expression. Optionally convert the extracted string to a specific type.

## Syntax

    `extract_json( JsonPath, DataSource, Type )`

### Arguments

* **JsonPath**: A JSONPath string that defines an accessor into the JSON document. The `[ ]` notation and dot `.` notation are equivalent; see the examples below.
* **DataSource**: A JSON document.
* **Type**: An optional type literal (for example, `typeof(long)`). If provided, the extracted value is converted to this type.

## Results

This function performs a JSONPath query into dataSource, which contains a valid JSON string, optionally converting that value to another type depending on the third argument.

## Examples

Extract response times to integers: 

```kusto {runSearch=true}
dataset="cribl_internal_logs" response_time 
| limit 10 
| extend foo=extract_json("$.response_time", _raw, typeof(int))
```

Extract hosts' available MB to integers:

```kusto
extract_json("$.hosts[1].AvailableMB", EventText, typeof(int))
```

Same as the previous query, but with equivalent brackets notation:

```kusto
extract_json("$['hosts'][1]['AvailableMB']", EventText, typeof(int))
```

