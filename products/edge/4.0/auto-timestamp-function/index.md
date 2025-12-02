# Auto Timestamp


The Auto Timestamp Function extracts time to a destination field, given a source field in the event. By default, Auto Timestamp makes a first best effort and populates `_time`.  When you [add a sample](data-preview) (via paste or a local file), you should accomplish time and event breaking at the same time you add the data. 

This Function allows fine-grained and powerful transformations to populate new time fields, or to edit existing time fields. You can use the Function's [Additional timestamps](#add_timestamps) section to create custom time fields using regex and custom JavaScript `strptime` functions. 

> The Auto Timestamp Function uses the same basic algorithm as the [Event Breaker](event-breakers#timestamp-settings) Function and the [C.Time.timestampFinder()](cribl-reference#time) native method.
>
{.box .success}

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. The default `true` setting passes all events through the Function.



**Description**: Simple description about this Function. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Source field**: Field to search for a timestamp. Defaults to `_raw`.

**Destination field**: Field to place extracted timestamp in. Defaults to `_time`. Supports nested addressing. 

**Default timezone**: Select a timezone to assign to timestamps that lack timezone info. Defaults to `Local`. (This drop-down includes support for legacy names: `EST5EDT`, `CST6CDT`, `MST7MDT`, and `PST8PDT`.)

**Additional timestamps**: To extract additional timestamp formats, click **+ Add Timestamp** to define each format. Each row will provide these fields: {{< id `add_timestamps` >}} 
- **Regex**: Regex, with first capturing group matching the timestamp.
- **Strptime format**: Select or enter the `strptime` format for the captured timestamp.

### **Advanced Settings**

**Time expression**: Expression with which to format extracted time. Current time, as a JavaScript Date object, is in global `time`. Defaults to `time.getTime() / 1000`. You can access other fields' values via `__e.<fieldName>`.

> For details about Cribl Edge's Library (native) time methods, see: [C.Time – Time Functions](cribl-reference#time).
>
{.box .info}

**Start scan offset**: How far into the string to look for a time string.

**Max timestamp scan depth**: Maximum string length at which to look for a timestamp.

**Default time**: How to set the time field if no timestamp is found. Defaults to **Current time**.

Two fields enable you to constrain (clamp) the parsed timestamp, to prevent the Function from mistakenly extracting non-time values as unrealistic timestamps:

- **Earliest timestamp allowed**: Enter a string that specifies the latest allowable timestamp, relative to now. (Sample value: `-42years`. Default value: `-420weeks`.) Parsed values earlier than this date will be set to the **Default time**.

- **Future timestamp allowed**: Enter a string that specifies the latest allowable timestamp, relative to now. (Sample value: `+42days`. Default value: `+1week`.) Parsed values after this date will be set to the **Default time**.

## Format Reference

This references https://github.com/d3/d3-time-format#locale_format. Directives annotated with a `(†)` symbol might be affected by the locale definition.

    %a - abbreviated weekday name. (†)
    %A - full weekday name. (†)
    %b - abbreviated month name. (†)
    %B - full month name. (†)
    %c - the locale’s date and time, such as %x, %X. (†)
    %d - zero-padded day of the month as a decimal number [01,31].
    %e - space-padded day of the month as a decimal number [ 1,31]; equivalent to %_d.
    %f - microseconds as a decimal number [000000, 999999].
    %H - hour (24-hour clock) as a decimal number [00,23].
    %I - hour (12-hour clock) as a decimal number [01,12].
    %j - day of the year as a decimal number [001,366].
    %m - month as a decimal number [01,12].
    %M - minute as a decimal number [00,59].
    %L - milliseconds as a decimal number [000, 999].
    %p - either AM or PM. (†)
    %Q - milliseconds since UNIX epoch.
    %s - seconds since UNIX epoch.
    %S - second as a decimal number [00,61].
    %u - Monday-based (ISO 8601) weekday as a decimal number [1,7].
    %U - Sunday-based week of the year as a decimal number [00,53].
    %V - ISO 8601 week of the year as a decimal number [01, 53].
    %w - Sunday-based weekday as a decimal number [0,6].
    %W - Monday-based week of the year as a decimal number [00,53].
    %x - the locale’s date, such as %-m/%-d/%Y. (†)
    %X - the locale’s time, such as %-I:%M:%S %p. (†)
    %y - year without century as a decimal number [00,99].
    %Y - year with century as a decimal number.
    %Z - time zone offset, such as -0700, -07:00, -07, or Z.
    %% - a literal percent sign (%).

### Complying with the Format

In order to use auto timestamping upon ingestion, the formatting used must match the `%Z` parameters above. E.g., this Function will automatically parse all of these formats:

- `2020/06/10T17:17:35.004-0700`
- `2020/06/10T17:17:35.004-07:00` 
- `2020/06/10T17:17:35.004-07`
- `2020/06/10T10:17:35.004Z`
- `2020/06/10T11:17:35.004 EST`

To parse other formats, you can use the [Additional Timestamps](#add_timestamps) section’s internal **Regex** or **Strptime Format** operators.

## Basic Example

Filter: `name.startsWith('kumquats') && value=='specific string here'`

This will allow the Auto Timestamp Function to act only on events matching the specified parameters.

Sample event: 

`Sep 20 12:03:55 PA-VM 1,2019/09/20 13:03:58,CRIBL,TRAFFIC,end,2049,2019/09/20 14:03:58,314.817.108.226,10.0.0.102,314.817.108.226,10.0.2.65,cribl,,,incomplete,vsys1,untrusted,trusted,ethernet1/3,ethernet1/2,log-forwarding-default,2018/09/20 13:03:58,574326,1,53722,8088,53722,8088,0x400064,tcp,allow,296,296,0,4,2018/09/20 13:03:45,7,any,0,730277,0x0,United States,10.0.0.0-10.255.255.255,0,4,0,aged-out,0,0,0,0,,PA-VM,from-policy,,,0,,0,,N/A,0,0,0,0`

To add this sample (after creating an Auto Timestamp Function with the above **Filter** expression): Go to **Preview** > **Add a Sample** > **Paste a Sample**, and add the data snippet above. Do not make any changes to timestamping or line breaking, and select **Save as Sample File**.

By default, Cribl Edge will inspect the first 150 characters, and will extract the first valid timestamp it sees. You can modify this character limit under **Advanced Settings** > **Max Timestamp Scan Depth**.

Cribl Edge will grab the first part of the event, and will settle on the first matching value to display for `time`: 

- `_time  1569006235`  
- **GMT**: Friday, 20 September 2019, 7:03:55 PM GMT  
- **Your Local Time**: Friday, 20 September 2019 PDT, 12:03:55 AM **GMT** -07:00

Because no explicit timezone has been set (under **Default Timezone**), `_time` will inherit the **Local** timezone, which in this example is `GMT -07:00`.

![](auto-timestamp-timezones-2.4.ded40a2767.png)

> ##### Timezone Dependencies and Details
>
> Cribl Edge uses ICU for timezone information. It does not query external files or the operating system. The bundled ICU is updated periodically.
> 
> For additional timezone details, see: https://www.iana.org/time-zones.
>
{.box .info}

## Advanced Settings Example

The `datetime.strptime()` method creates a datetime object from the string passed in by the **Regex** field.

Here, we'll use `datetime.strptime()` to match a timestamp in AM/PM format at the end of a line.

Sample: 

`This is a sample event that will push the datetime values further on inside the event. This is still a sample event and finally here is the datetime information!: Server_UTC_Timestamp="04/27/2020 2:30:15 PM"`

**Max timestamp scan depth**: `210`

Click to add **Additional timestamps**:

**Regex**:  `(\d{1,2}\/\d{2}\/\d{4}\s\d{1,2}:\d{2}:\d{2}\s\w{2})`

**Strptime format**: `'%m/%d/%Y %H:%M:%S %p'`

> ##### Gnarly Details
>
> - This Function supports the `%f` (microseconds) directive. 
> - Cribl Edge will truncate timestamps to three-digit (milliseconds) resolution, omitting trailing zeros.
> - For further examples, see [Extracting Timestamps from Messy Logs](https://cribl.io/blog/extracting-timestamps-from-messy-logs/).
>
{.box .info}
