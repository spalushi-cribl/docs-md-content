# sort


The `sort` operator arranges events into order by one or more fields.

Alias: [`order`](order) ([`order`](order) and `sort` are synonyms.)

## Syntax

`Scope | sort [ topN=MaxNoOfOutputEvents ] [ maxEvents=MaxNoOfInputEvents ] by Field [ asc | desc ] [ nulls first | nulls last ] [, ...]`

### Arguments

| Name                          | Type   | Required | Description                                                                                                                                                                                    |
| ----------------------------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Scope**                     | String | Yes      | The events to search.                                                                                                                                                                          |
| **MaxNoOfOutputEvents**       | Int    | No       | Maximum number of events to produce. The output is limited to `10000` events, so entering a higher **MaxNoOfOutputEvents** value effectively sets a limit of `10000`.                          |
| **MaxNoOfInputEvents**        | Int    | No       | Maximum number of events to handle and arrange. Usually, this value is already determined by the [`limit`](limit) operator used earlier in the query, but you can also set it explicitly here. |
| **Field**                     | String | Yes      | Field to sort by. The type of the field values must be numeric, date, time, or string.                                                                                                         |
| `asc` or `desc`               | String | No       | `asc` sorts into ascending order, low to high. Default is `desc`, high to low. For more details, see [Sorting Rules](#rules).                                                                  |
| `nulls first` or `nulls last` | String | No       | `nulls first` will place the null values at the beginning and `nulls last` will place the null values at the end. Default for `asc` is `nulls first`. Default for `desc` is `nulls last`.      |

## Sorting Rules {#rules}




- Numeric values appear before other data types. An exception to that may be [`null`](null), whose behavior depends on
  the `nulls first`/`nulls last` setting above.
- Numeric strings are converted to numbers when sorted. For example, `“100”` and `“5”` are compared as `100` and `5`.
- By default: for ascending order, [`null`](null)s appear first, and for descending order, [`null`](null)s appear last.
  You can change this with the `nulls first`/`nulls last` setting above.


## Example

All events with a specific `ClientRequestId`, sorted by their `Timestamp`.

```kusto
dataset=myDataset
| where ClientRequestId == "5a848f70-9996-eb17-15ed-21b8eb94bf0e"
| sort by Timestamp asc
```

Sort results by the field `event` in descending order.

```kusto {runSearch=true}
dataset=$vt_dummy event<100
| extend parity=iif(event%2==0, 'even', 'odd')
| order by event desc
```
