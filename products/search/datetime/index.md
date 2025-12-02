# datetime


The `datetime (date)` data type represents an instant in time. A `datetime` value can be any of the following:

1. A complete timestamp, expressed in [Unix epoch time](https://en.wikipedia.org/wiki/Unix_time). Examples:

   - `1700511360` (a whole number, representing seconds)

   - `1700511420.123` (a fractional/decimal number, representing milliseconds)

2. Time only, assuming “today” UTC as the date. Examples:

   - `12:34`

   - `12:34:56`

   - `12:34:56.789`

3. Date only, assuming midnight UTC as the date line. Example:

   - `2025-01-23`

4. A combination of date and time. Examples:

   - `2025-01-23 12:34:56.789` (supports nonstandard format with space)

   - `2025-01-23T12:34:56.789Z` (supports ISO "Z" for time zone)

   - `2025-01-23T12:34:56.789+00:00` (supports ISO variation with offset)