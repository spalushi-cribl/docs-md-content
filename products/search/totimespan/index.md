# totimespan


The `totimespan` function converts the input expression into a time interval, in seconds.


> This output format differs from the standard [Kusto](https://learn.microsoft.com/en-us/kusto/query/totimespan-function) `totimespan` function, which returns a timespan.
> 
> To convert [`datetime`](datetime) (Unix time) into a [`string`](string), or vice versa, use the [`strftime`](strftime) and [`strptime`](strptime) functions.
{.box .info}

## Syntax

`totimespan( Expression )`

### Arguments

- **Expression**: Expression that will be converted to timespan.

## Returns

If the conversion is successful, the result will be the number of seconds in the interval. Otherwise, the result will be `null`.

## Example

This example returns `3660`:

```kusto {runSearch=true}
print totimespan( "0.01:01:00" )
```
