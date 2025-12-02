# ago



The `ago` function subtracts the given timespan from the current UTC clock time.

Like [now](now), this function can be used multiple times in a statement and the UTC clock time being referenced will be the same for all instantiations.

## Syntax

    `ago( Timespan )`

### Arguments

* **Timespan**: Interval to subtract from the current UTC clock time.

## Returns

`now() - Timespan`

{#example}
## Examples

This example subtracts two days from the current time, using, [strftime](strftime) to format the result as a human-readable date:


```kusto {runSearch=true}
print newDate = ago(2d)
| extend prettyDate = strftime( newDate, '%x' )
```

This example returns all rows with a timestamp in the past hour:

```kusto
dataset=myDataset
| where Timestamp > ago(1h)
```
