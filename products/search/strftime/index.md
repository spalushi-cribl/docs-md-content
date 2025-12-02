# strftime


The `strftime` function converts a [`datetime`](datetime) (date) object to a human-readable string.

Alias: `format_time`

## Syntax

    `strftime( TimeSeconds, FormatString )`

### Arguments

* **TimeSeconds**: The time value in seconds to convert to a string.
* **FormatString**: The [time pattern](#timePattern) to convert the date into.

#### Time Pattern {#timePattern}

The time pattern string supports the following directives:

* `%a` - abbreviated weekday name.
* `%A` - full weekday name.
* `%b` - abbreviated month name.
* `%B` - full month name.
* `%c` - the locale’s date and time, such as `%x, %X`.
* `%d` - zero-padded day of the month as a decimal number [01,31].
* `%e` - space-padded day of the month as a decimal number [ 1,31]; equivalent to `%_d`.
* `%f` - microseconds as a decimal number [000000, 999999].
* `%g` - ISO 8601 week-based year without century as a decimal number [00,99].
* `%G` - ISO 8601 week-based year with century as a decimal number.
* `%H` - hour (24-hour clock) as a decimal number [00,23].
* `%I` - hour (12-hour clock) as a decimal number [01,12].
* `%j` - day of the year as a decimal number [001,366].
* `%m` - month as a decimal number [01,12].
* `%M` - minute as a decimal number [00,59].
* `%L` - milliseconds as a decimal number [000, 999].
* `%p` - either AM or PM.
* `%q` - quarter of the year as a decimal number [1,4].
* `%Q` - milliseconds since UNIX epoch.
* `%s` - seconds since UNIX epoch.
* `%S` - second as a decimal number [00,61].
* `%u` - Monday-based (ISO 8601) weekday as a decimal number [1,7].
* `%U` - Sunday-based week of the year as a decimal number [00,53].
* `%V` - ISO 8601 week of the year as a decimal number [01, 53].
* `%w` - Sunday-based weekday as a decimal number [0,6].
* `%W` - Monday-based week of the year as a decimal number [00,53].
* `%x` - the locale’s date, such as `%-m/%-d/%Y`.
* `%X` - the locale’s time, such as `%-I:%M:%S %p`.
* `%y` - year without century as a decimal number [00,99].
* `%Y` - year with century as a decimal number, such as `1999`.
* `%Z` - time zone offset, such as `-0700`, `-07:00`, `-07`, or `Z`.
* `%%` - a literal percent sign (`%`).

For more details, see [format specifiers](https://github.com/d3/d3-time-format#locale_format).

## Example

This example returns `"2022-06-27T17:00:26.499+0000"`:

```kusto {runSearch=true}
print strftime( 1656349226.499, '%Y-%m-%dT%H:%M:%S.%L%Z' )
```
