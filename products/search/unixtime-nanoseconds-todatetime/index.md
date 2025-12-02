# unixtime_nanoseconds_todatetime


The `unixtime_nanoseconds_todatetime` function converts the input into a [`datetime`](datetime) value (Unix
time in seconds).





> To convert [`datetime`](datetime) (Unix time) into a [`string`](string), or vice versa, use the [`strftime`](strftime) and [`strptime`](strptime) functions.
{.box .info}


## Syntax

`unixtime_nanoseconds_todatetime( Nanoseconds )`

### Arguments

- **Nanoseconds**: An epoch timestamp in nanoseconds. Time that occurs before the Unix epoch (1970-01-01 00:00:00) has a
  negative timestamp value.

## Returns

If the conversion is successful, the result will be a [`datetime`](datetime) value (Unix time in seconds). Otherwise,
the result will be `null`.

## Example

This example returns `1,546,300,800`:

```kusto {runSearch=true}
print unixtime_nanoseconds_todatetime( 1546300800000000000 )
```
