# unixtime_microseconds_todatetime


The `unixtime_microseconds_todatetime` function converts the input into a [`datetime`](datetime) value (Unix time in
seconds).





> To convert [`datetime`](datetime) (Unix time) into a [`string`](string), or vice versa, use the [`strftime`](strftime) and [`strptime`](strptime) functions.
{.box .info}


## Syntax

`unixtime_microseconds_todatetime( Microseconds )`

### Arguments

- **Microseconds**: An epoch timestamp in microseconds. Time that occurs before the Unix epoch (1970-01-01 00:00:00) has
  a negative timestamp value.

## Returns

If the conversion is successful, the result will be a [`datetime`](datetime) value (Unix time in seconds). Otherwise,
the result will be `null`.

## Example

This example returns `1,546,300,800`:

```kusto {runSearch=true}
print unixtime_microseconds_todatetime( 1546300800000000 )
```
