# mv-pull


Pull key-value pairs from array objects into a top-level event, or into a dedicated object/bag.  If you’re curious, mv stands for multiple value.

## Syntax

    `<scope> | mv-pull [key=<nameOfKeyFieldInData>]  [value=<nameOfValueFieldInData>] [delete_original=<boolean>] <nameOrPathOfFieldToArray> [as <targetFieldName>]`

### Arguments

|Name|Type|Required|Description|
|--|--|--|--|
|**delete_original**| boolean | No | If set to `true`, deletes the source array in the event after pulling up all key-value pairs. With the `false` default, the source array is unchanged. |
|**nameOfKeyFieldInData**| string| No | Name (or column path) of the key in each array element. Defaults to literal `Name`. |
|**nameOfValueFieldInData**| string | No | Name (or column path) of the value in each array element. Defaults to literal `Value`. |
|**nameOrPathOfFieldToArray**| string | Yes | Name (or column path) of the field that contains the array of objects from which to pull key-value pairs. |
|**targetFieldName**| string | No | Name or column path of an object/bag to contain the pulled fields. Specify this to group all pulled fields into one object. If omitted, the output will be separate fields at the event's root level. |

## Returns

If **targetFieldName** is specified, returns a corresponding object/bag containing all the extracted key-value pairs as fields. Otherwise, returns the extracted key-value pairs as separate top-level fields.

## Examples

This example pulls a simple array's elements into a single event. Uncomment the indicated line to display the elements within that event:

```kusto {runSearch=true}
print eventData=dynamic( { "eventArray": [ { "Name": "TotalBytes", "Value": 536 }, { "Name": "TotalPackets", "Value": 16 } ] } )
| mv-pull eventData.eventArray
//| project-away eventData // uncomment this line to see the results by themselves
| extend  _time=now()
| render event
```

A query with this argument would pull key-value pairs from an array named `eventData` into separate top-level fields in the target event:

```kusto
... | mv-pull eventData | ...
```

The output might take the following form (with fictitious data):

```json
{ 
  _time: 1724241414.121,
  dataset: "metrics-store",
  eventData: [ 
    { Name: "numBytesRead", Value: 1234 },
    { Name: "numBytesWrite", Value: 200 },
    { Name: "numRequests", Value: 42 },
  ],
  numBytesRead: 1234,
  numBytesWrite: 200,
  numRequests: 42,
}
```

Here is the same query with additional arguments, targeting an array whose key-value pairs have unusual naming:

```kusto
... | mv-pull key="n" value="v" delete_original=true otherEventData as itsMyBag ...
```

The output would instead have this format:

```json
{ 
  _time: 1724241414.121,
  dataset: "metrics-store",
  itsMyBag: { numBytesRead: 1234, numBytesWrite: 200, numRequests: 42 }
}
```
