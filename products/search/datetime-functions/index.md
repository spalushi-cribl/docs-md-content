# DateTime Functions


A list of all DateTime functions supported by Cribl Search.

---


| Name                                                                   | Description                                                                                                          |
| ---------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| [`ago`](ago)                                                           | Subtracts the given timespan from the current UTC clock time.                                                        |
| [`datetime_add`](datetime-add)                                         | Calculates a new datetime from a specified datepart multiplied by a specified amount, added to a specified datetime. |
| [`datetime_diff`](datetime-diff)                                       | Calculates the number of the specified periods between two datetime values.                                          |
| [`datetime_part`](datetime-part)                                       | Extracts the requested date part as an integer value.                                                                |
| [`dayofmonth`](dayofmonth)                                             | Returns the integer number representing the day number of the given month.                                           |
| [`dayofweek`](dayofweek)                                               | Returns an integer between `0` and `6` representing the day of the week, beginning on Sunday.                                      |
| [`dayofyear`](dayofyear)                                               | Returns the integer number representing the day number of the given year.                                            |
| [`endofday`](endofday)                                                 | Returns the end of the day containing the date, shifted by an offset, if provided.                                   |
| [`endofmonth`](endofmonth)                                             | Returns the end of the month containing the date, shifted by an offset, if provided.                                 |
| [`endofweek`](endofweek)                                               | Returns the end of the week containing the date, shifted by an offset, if provided.                                  |
| [`endofyear`](endofyear)                                               | Returns the end of the year containing the date, shifted by an offset, if provided.                                  |
| [`format_datetime`](format-datetime)                                   | Formats a datetime according to the provided format.                                                                 |
| [`format_timespan`](format-timespan)                                   | Formats a timespan according to the provided format.                                                                 |
| [`getmonth`](getmonth) ([`monthofyear`](monthofyear))                  | Gets the month number (1–12) from a datetime.                                                                        |
| [`getyear`](getyear) (`yearofyear`)                                    | Returns the year part of a datetime.                                                                                 |
| [`hourofday`](hourofday)                                               | Returns the integer number representing the hour number of the given date.                                           |
| [`make_datetime`](make-datetime)                                       | Converts the specified date and time into a datetime value (Unix time in seconds).                                   |
| [`make_timespan`](make-timespan)                                       | Converts the specified time period into a datetime value (Unix time in seconds).                                     |
| [`now`](now)                                                           | Returns the current UTC clock time as a datetime value (Unix time in seconds).                                       |
| [`startofday`](startofday)                                             | Returns the start of the day containing the date, shifted by an offset, if provided.                                 |
| [`startofmonth`](startofmonth)                                         | Returns the start of the month containing the date, shifted by an offset, if provided.                               |
| [`startofweek`](startofweek)                                           | Returns the start of the week containing the date, shifted by an offset, if provided.                                |
| [`startofyear`](startofyear)                                           | Returns the start of the year containing the date, shifted by an offset, if provided.                                |
| [`strftime`](strftime) (`format_time`)                                 | Converts a datetime (date) object to a human-readable string.                                                        |
| [`strptime`](strptime) (`parse_time`)                                  | Converts a string to a datetime.                                                                                     |
| [`todatetime`](todatetime)                                             | Converts the input into a datetime value (Unix time in seconds).                                                     |
| [`totimespan`](totimespan)                                             | Converts the input into a number of seconds.                                                                            |
| [`unixtime_microseconds_todatetime`](unixtime-microseconds-todatetime) | Converts the input into a datetime value (Unix time in seconds).                                                     |
| [`unixtime_milliseconds_todatetime`](unixtime-milliseconds-todatetime) | Converts the input into a datetime value (Unix time in seconds).                                                     |
| [`unixtime-nanoseconds_todatetime`](unixtime-nanoseconds-todatetime)   | Converts the input into a datetime value (Unix time in seconds).                                                     |
| [`unixtime_seconds_todatetime`](unixtime-seconds-todatetime)           | Converts the input into a datetime value (Unix time in seconds).                                                     |
| [`week_of_year`](week-of-year)                                         | Returns an integer representing the week number.                                                                     |

