# C.Time – Time Methods


## `C.Time.adjustTZ()`

```js
Time.adjustTZ(epochTime: number, tzTo: string, tzFrom: string = 'UTC'): number | undefined
```

Adjusts a timestamp from one timezone to another.

Returns the adjusted timestamp, in UNIX epoch time (ms).

|Parameter|Type|Description|
|---|---|---|
|`epochTime` | number | Timestamp to adjust, in UNIX epoch time.
|`tzTo` | string | Timezone to adjust to (full name).
|`tzFrom` (optional) | string | Timezone of the timestamp.

> Cribl relies on [this list of TZ Database Time Zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List).
>
> Note that these are timezone identifiers (full names), not abbreviations such as "CET", "PST", "EST", and so on.
{.box .success}

### Examples

To adjust a timestamp found in the `_time` field to a selected timezone (here, Australia/Melbourne), use:

```js
C.Time.adjustTZ(_time, "Australia/Melbourne")
```

The method returns the timestamp in milliseconds.
If you want to further operate on it using methods such as [`strftime()`](#strftime),
which takes as parameter UNIX epoch time in seconds, you can divide it by 1000:

```js
C.Time.strftime(C.Time.adjustTZ(_time, "Australia/Melbourne") / 1000, "%d %B %Y %H %M %Z" )
```

## `C.Time.clamp()` {#clamp}

```js
Time.clamp(date: T, earliest: T, latest: T, defaultDate?: T ): T  | undefined
```

Constrains a parsed timestamp to realistic earliest and latest boundaries.

Returns a JavaScript Date object or timestamp, depending on the format of the `date` parameter.

|Parameter|Type|Description|
|---|---|---|
|`date` | Date \| number | Timestamp to constrain, in UNIX epoch time (ms) or JavaScript Date format.
|`earliest` | Date \| number | Earliest allowable timestamp, in UNIX epoch time (ms) or JavaScript Date format.
|`latest` |Date \| number| Latest allowable timestamp, in UNIX epoch time (ms) or JavaScript Date format.
|`defaultDate` (optional) |Date \| number| Default date, in UNIX epoch time (ms) or JavaScript Date format. This date is used for values that fall outside the `earliest` or `latest` boundaries.

### Examples

Suppose you have a date in a "YYYY-MM-DD" format in a `time` field.
To make sure this date does not exceed the range of 1 Jan 2020 – 1 Jan 2025, you can use:

```js
C.Time.clamp(time, "2020-01-01", "2025-01-01")
```

|Input|Output|
|---|---|
|"2016-02-11"|"2020-01-01" (set to the earliest value)|
|"2026-01-01"|"2025-01-01" (set to the latest value)|
|"2023-12-24"|"2023-12-24" (unchanged, because it fits inside the defined range)|

To make sure that all dates that fall outside the bounds are changed to a specific value, add the third parameter:

```js
C.Time.clamp(time, "2020-01-01", "2025-01-01", "2022-02-22")
```

|Input|Output|
|---|---|
|"2016-02-11"|"2022-02-22" (set to the default value)|
|"2026-01-01"|"2022-02-22" (set to the default value)|
|"2023-12-24"|"2023-12-24" (unchanged, because it fits inside the defined range)|

## `C.Time.strftime()` {#strftime}

```js
Time.strftime(date: number | Date | string, format: string, utc: boolean = true): string | undefined
```

Formats a [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) object or timestamp number as a time string,
using [d3js time format](https://d3js.org/d3-time-format#locale_format).

Returns representation of the given date in the specified format.

|Parameter|Type|Description|
|---|---|---|
|`date` | number \| Date \| string | Date to format, in UNIX epoch time (in seconds) or JavaScript [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) format.
|`format` | string | New format for the date.
|`utc` (optional) | boolean | Whether to output the time in UTC (`true`), rather than in local timezone.

### Examples

To transform a `_time` field containing a UNIX timestamp to a string with time formatted,
for example: `15 November 2023, 08:03`, you can use:

```js
C.Time.strftime(_time, "%d %B %Y, %H:%M")
```

Some common formats (assuming UNIX timestamp of 1702460134) are:

|Format|Output|
|---|---|
|"%Y-%m-%d" (ISO date)|"2023-12-13"|
|"%d %B %Y %H:%M:%S"|"13 December 2023 09:35:34"|
|"%x %X" (the locale's date and time format) |"12/13/2023 9:35:34 AM"|

:::success
For detailed reference of the date and time tokens, see [d3js reference](https://d3js.org/d3-time-format#locale_format)
or [Auto Timestamp](auto-timestamp-function#format-reference).
:::

## `C.Time.strptime()`

```js
Time.strptime(str: string, format: string, utc: boolean = true, strict: boolean = false): Date
```

Extracts time from a string using [d3js time format](https://d3js.org/d3-time-format#locale_format) that defines the format of the incoming string.

Returns a parsed JavaScript [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) object,
or `null` if the specified `format` did not match.

|Parameter|Type|Description|
|---|---|---|
|`str` | string | Timestamp to parse.
|`format` | string | Format to parse.
|`utc` (optional) | boolean | Whether to interpret times as UTC (`true`), rather than as local time.
|`strict` (optional) | boolean | Whether to return `null` if there are any extra characters after timestamp.

### Examples

To convert a string in the "YYYY-MM-DD" format, for example "2023-12-12", to a Date object, use:

```js
C.Time.strptime(F, "%Y-%m-%d")
```

Additionally, to interpret the time in UTC, and make sure null is returned if the timestamp contains any trailing characters, use:

```js
C.Time.strptime(F, "%Y-%m-%d", true, true)
```

:::success
For detailed reference of the date and time tokens, see [d3js reference](https://d3js.org/d3-time-format#locale_format)
or [Auto Timestamp](auto-timestamp-function#format-reference).
:::

## `C.Time.timestampFinder()` {#time-finder}

```js
Time.timestampFinder(utc?: boolean).find(str: string): IAutoTimeParser
```

Extracts time from the specified field, using the same algorithm as the [Auto Timestamp](auto-timestamp-function) Function and the [Event Breaker](event-breakers#timestamp-settings) Function.

Returns representation of the extracted time; truncates timestamps to three-digit (milliseconds) resolution, omitting trailing zeros.

|Parameter|Type|Description|
|---|---|---|
|`utc` | boolean | Whether to output the time in UTC (`true`), rather than in local timezone.
|`str` | string | The field in which to search for the time.  

### Examples

To find timestamps in the `_raw` field and output them in UTC, you can use:

```js
C.Time.timestampFinder(true).find(_raw)
```

You can then format the string output with [`strftime()`](#strftime):

```js
C.Time.strftime(C.Time.timestampFinder(true).find(_raw), "%d.%m.%Y, %H.%M")
```
