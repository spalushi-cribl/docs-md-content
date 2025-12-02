# todatetime


The `todatetime` function converts the input into a [`datetime`](datetime) value (Unix time in seconds).





> To convert [`datetime`](datetime) (Unix time) into a [`string`](string), or vice versa, use the [`strftime`](strftime) and [`strptime`](strptime) functions.
{.box .info}


## Syntax

`todatetime( Expression )`

### Arguments

- **Expression**: Expression that will be converted into [`datetime`](datetime).

## Returns

If the conversion is successful, the result will be a [`datetime`](datetime) value, otherwise, the result will be
`null`.

## Example

This example returns `1,485,216,000`:

```kusto {runSearch=true}
print todatetime( "2015-25-24" )
```
