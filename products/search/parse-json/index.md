# parse_json


The `parse_json` function interprets a `string` as a JSON value and returns the value as `dynamic`. Where possible, it converts the value into relevant data types. 

> For strict parsing with no data type conversion, use the [extract](extract) or [extract_json](extract-json) function instead.
{.box .info}

Alias: `todynamic`

This function is better than `extract_json` when you need to extract more than one element of a JSON compound object.

## Syntax

    `parse_json( JSON )`

### Arguments

* **JSON**: An expression of type `string`. It represents a [JSON-formatted](https://json.org/) value, or an expression of type dynamic, representing the actual `dynamic` value.

## Results

An object of type `dynamic` that is determined by the value of **JSON**:

* If **JSON** is of type `dynamic`, its value is used as-is.
* If **JSON** is of type `string`, and is a properly formatted JSON string, then the string is parsed, and the value produced is returned.
* If **JSON** is of type `string`, but it isn't a properly formatted JSON string, then the returned value is an object of type `dynamic` that holds the original `string` value.

## Example

```kusto {runSearch=true}
print theData='{"theValue":123,"theString":"Hello!"}' | extend parsed_data=parse_json(theData) | render event
```

In the following example, when `context_custom_metrics` is a `string` that looks like this:

```json
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

...then the following KQL fragment retrieves the value of the `duration` slot in the object, and from that it retrieves two slots, `duration.value` and `duration.min` (`118.0` and `110.0`, respectively):

```kusto
dataset=myDataset
| extend d=parse_json(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```
