# Schedule and Run Collector Jobs



Once you've configured a [Collector](collectors), you can either run it immediately ("ad hoc") to collect data, or schedule it to run on a recurring interval. Scheduling requires some extra configuration upfront, so we cover this option first.

> For **ad hoc** collection, you can configure whether a job interrupted by an unintended Cribl Stream shutdown will automatically resume upon Cribl Stream restart. 
> 
> But regardless of this configuration, if you **explicitly** restart or stop Cribl Stream, this will cancel any currently running jobs. This applies to executing the `./cribl restart` or `./cribl stop` CLI commands, as well as to selecting **Settings** > **Global** > **Controls** > **Restart**.
> 
> A **scheduled** job interrupted by a shutdown (whether explicit or unintended) will **not** resume upon restart.
>
{.box .info}


## Schedule Configuration {#schedule-config}

Select **Schedule** beside a configured Collector to display the **Schedule Collector** modal. This provides the following controls.

**Enabled**: Toggle on to enable this collection schedule.

> The scheduled job will keep running on this schedule forever, unless you toggle **Enabled** off, which preserves the schedule's configuration but prevents its execution.
>
{.box .warning}

**Cron schedule**: A [cron schedule](https://en.wikipedia.org/wiki/Cron) on which to run this job.

- The **Estimated schedule** below this field shows the next few collection runs, as examples of the cron interval you've scheduled.

**Concurrent run limit**: Sets the maximum number of instances of this scheduled job that Cribl Stream will simultaneously run.

**Skippable**: Skippable jobs can be delayed up to their next run time if the system is hitting concurrency limits. Defaults to toggled on. Toggling off displays this additional option:

- **Resume missed runs**: Default is toggled off. When toggled on, if Cribl Stream's Leader (or single instance) restarts, it will run all missed jobs according to their original schedules.

#### Skippable Jobs and Concurrency Limits {#limits}

If toggled on, the **Skippable** option obeys these concurrency limits configured separately in **Settings** > **Global** > **General Settings** > **Job Limits**:

- **Concurrent job limit**
- **Concurrent scheduled job limit**

> See [Job Limits](collectors-job-limits) for details on these and other limits that you can set  in global ⚙️ **Settings**.
>
{.box .success}

When the above limits delay a Skippable job:

- The Skippable job will be granted slightly higher priority than non-Skippable jobs.

- If the job receives resources to run before its next scheduled run, Cribl Stream will run the delayed job, then snap back to the original cron schedule.

- If resources do **not** free up before the next scheduled run: Cribl Stream will **skip** the delayed run, and snap back to the original cron schedule. 

Toggle **Skippable** off if you absolutely must have all your data, for compliance or other reasons. In this case, Cribl Stream will build up a backlog of jobs to run. 

Toggling **Skippable** off results in behavior more like the TCP protocol, and toggling **Skippable** on results in behavior more like UDP.

> **All** collection jobs are constrained by the following options in **Settings** > **Global** > **General Settings** > **Job Limits**:
> 
> -  **Concurrent task limit**
> -  **Task usage percentage limit**
>
{.box .warning}



## Run Configuration and Shared Settings {#run-config}

Most of the remaining fields and options below are shared with the **Run Collector** modal, which you can open by selecting **Run** beside a configured Collector.

### Mode

Depending on your requirements, you can schedule or run a collector in these modes: 

* [Preview](#preview) – Default for Run, but not offered for scheduled jobs.
* [Discovery](#discovery) – Default for scheduled jobs.
* [Full Run](#full-run).

#### Preview

In the Preview mode, a collection job will return only **a sample subset** of matching results (e.g., 100 events). This is very useful in cases when users need a data sample to:

   * Ensure that the correct data comes in.
   * Iterate on Filter expressions.
   * Capture a sample to iterate on Pipelines.

> **Schedule** configuration omits the Preview option, because Preview is designed for immediate analysis and decision making. To configure a scheduled job with high confidence, you can first manually run Preview jobs with the same Collector, to verify that you're collecting the data you expect.
>
{.box .info}

##### Preview Settings

In Preview mode, you can optionally configure these options:

**Capture time (sec)**: Maximum time interval (in seconds) to collect data.

**Capture up to N events**: Maximum number of events to capture.

**Where to capture**: Select the stage in the [event processing order](event-processing-order) where events should be captured during the Preview run. During a Preview run, the first two of the four capture options are available.

* `Before pre-processing Pipeline` (Default): Captures events immediately after they're received.
* `Before the Routes`: Captures events after pre-processing but before they're routed.

#### Discovery

In Discovery mode, a collection job will return only **the list of objects/files** to be collected, but none of the data. This mode is typically used to ensure that the Filter expression and time range are correct before a Full Run job collects unintended data.

##### Send to Routes

In Discovery mode, this toggle enables you to send discovery results to Cribl Stream Routes. Default is toggled off.

> This setting overrides the Collector configuration's **Result Routing > Send to Routes** setting.
>
{.box .info}

#### Full Run

In Full Run mode, the collection job is fully executed by Worker Nodes, and will return all data matching the Run configuration. Optionally, for the Database Collector, you can also configure [state tracking](#state).

### Time Range {#time-range}

Set an **Absolute** or **Relative** time range for data collection. The **Relative** option is the default, and is particularly useful for configuring scheduled jobs.

In Discovery and Full Run modes, time range filters are applied before events enter the pre-processing Pipeline.

For Preview mode, while the default is to filter before pre-processing, you can select alternative capture points with the **Where to capture** setting.

#### Absolute

Select **Absolute** to set fixed collection boundaries. Use the **Earliest** and **Latest** controls to set the start date/time and end date/time. Provide the correct time zone in the **Range timezone** drop-down.

#### Relative

Select **Relative** to set dynamic collection boundaries. For ad hoc runs, these are relative to the current epoch time. For scheduled jobs, boundaries are calculated based on the job's scheduled start time, not the current time, ensuring the collection window remains consistent even if the job is delayed. Next, use the **Earliest** and **Latest** fields to set start and end times like these:

- **Earliest** example values: `-1h`, `-42m`, -`42m@h`
- **Latest** example values: `now`, `-20m`, `+42m@h`

By default, Cribl Stream uses **Earliest** and **Latest** as time-based tokens, which it evaluates in two ways:

- By comparison to time values specified in the directory path.
- By comparison to the `_time` field.

The Google Cloud Storage, REST/API, and Splunk Search Collectors provide a **Disable time filter** option in their **Collector Settings**. When toggled on, scheduling or running these Collectors against a date range will cause Cribl Stream to ignore event time–based filtering entirely.

#### Relative Time Syntax

For Relative times, the **Earliest** and **Latest** controls accept the following syntax:

`[+|-]<time_integer><time_unit>@<snap-to_time_unit>` 

To break down this syntax:

Syntax Element | Values Supported
--- | ---
Offset | Specify: `-` for times in the past, `+` for times in the future, or omit with `now`.
<time_integer> | Specify any integer, or omit with `now`.
<time_unit> | Specify the `now` constant, or one of the following abbreviations: `s[econds]`, `m[inutes]`, `h[ours]`, `d[ays]`, `w[eeks]`, `mon[ths]`, `q[uarters]`, `y[ears]`.
@<snap-to_time_unit> | Optionally, you can append the `@` modifier, followed by any of the above `<time_unit>`s, to round down to the nearest instance of that unit. (See the next section for details.)

Cribl Stream validates relative time values using these rules:

- **Earliest** must not be later than **Latest**.
- Values without units get interpreted as seconds. (E.g., `-1` = `-1s`.)



#### Snap-to-Time Syntax

The `@` snap modifier always rounds **down** (backwards) from any specified time. This is true even in relative time expressions with `+` (future) offsets. For example:

- `@d` snaps back to the beginning of today, 12:00 AM (midnight). 

- `+128m@h` looks forward 128 minutes, then snaps back to the nearest round hour. (If you specified this in the `Latest` field, and ran the Collector at 4:20 PM, collection would end at 6:00 PM. The expression would look forward to 6:28 PM, but snap back to 6:00 PM.)

Other options:

- `@w` or `@w7` to snap back to the beginning of the week – defined here as the preceding Sunday.
- To snap back to other days of a week, use `w1` (Monday) through `w6` (Saturday).
- `@mon` to snap back to the 1st of a month.
- `@q` to snap back to the beginning of the most recent quarter – Jan. 1, Apr. 1, Jul. 1, or Oct. 1.
- `@y` to snap back to Jan. 1.

### Filter

This is a JavaScript filter expression that is evaluated against token values in the provided collector path (see below), and against the events being collected. The **Filter** value defaults to `true`, which matches all data, but this value can be customized almost arbitrarily. 

For example, if a [File System](collectors-filesystem) or [S3](collectors-s3) collector is run with this Filter: 

`host=='myHost' && source.endsWith('.log') || source.endsWith('.txt')`

...then only files/objects with `.log` or `.txt` extensions will be fetched. And, from those, only those events with host field `myHost` will be collected.

At the **Filter** field's right edge are a Copy button, an Expand button to open a validation modal, and a History button. For more extensive options, see [Tokens for Filtering](#tokens) below. 

### Advanced Settings

**Log level**: Level at which to set task logging. More-verbose levels are useful for troubleshooting jobs and tasks, but use them sparingly.

**Lower task bundle size**: Limits the bundle size for small tasks. E.g., bundle five 200KB files into one 1MB task bundle. Defaults to `1MB`.

**Upper task bundle size**: Limits the bundle size for files above the **Lower task bundle size**. E.g., bundle five 2MB files into one 10MB task bundle. Files greater than this size will be assigned to individual tasks. Defaults to `10MB`.

**Reschedule tasks**: Whether to automatically reschedule tasks that failed with non-fatal errors. Defaults to toggled on; does not apply to fatal errors.

**Task reschedule limit**: Maximum number of times a task can be rescheduled. Defaults to `1`.

**Job timeout**: Maximum time this job will be allowed to run. Units are seconds, if not specified. Sample values: `30`, `45s`, or `15m`. Minimum granularity is 10 seconds, so a `45s` value would round up to a 50-second timeout. Defaults to `60m`. Enter '0' for unlimited time (no timeout).

### State Tracking {#state}

**Enabled**: Toggle on to enable state tracking when running scheduled or full run jobs.

Subsequent options depend on the type of Collector you're working with.

For Database Collectors, see [Working with a Tracking Column](collectors-database#tracking-column) to learn how to craft a Collector query for state tracking.

For REST Collectors, see [Working with State Tracking](collectors-rest#state-tracking) to learn how enable and use state in your collection configuration.

> State tracking is available only for Database and REST Collectors.
> 
> Cribl recommends that when you enable state tracking on a Collector, you configure the schedule to allow only one job to run at a time.
> 
> This prevents one run from starting before another has finished, which avoids the collection of duplicate records and/or incorrect state tracking.
>
{.box .success}

### Tokens for Filtering {#tokens}

Let's look at the options for path-based (basic) and time-based token filtering.

#### Basic Tokens

In collectors with paths, such as [Filesystem](collectors-filesystem) or [S3](collectors-s3), Cribl Stream supports path filtering via token notation. Basic tokens' syntax follows that of [JS template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals): `${<token_name>}` – where `token_name` is the field (name) of interest. 

For example, if the path was set to `/var/log/${hostname}/${sourcetype}/`, you could use a Filter such as `hostname=='myHost' && sourcetype=='mySourcetype'` to collect data only from the `/var/log/myHost/mySourcetype/` subdirectory.

#### Time-based Tokens

In paths with time partitions, Cribl Stream supports further filtering via time-based tokens. This has a direct effect with earliest and latest boundaries. When a job runs against a path with time partitions, the job traverses a minimal superset of the required directories to satisfy the time range, before subsequent event `_time` filtering. 

##### About Partitions and Tokens

Cribl Stream processes time-based tokens as follows:

  * For each path, time partitions must be notated in descending order. So Year/Month/Day order is supported, but Day/Month/Year is not.
  * Paths may contain more than one partition. E.g., `/my/path/2020-04/20/`.
  * In a given path, each time component can be used only once. So `/my/path/${_time:%Y}/${_time:%m}/${_time:%d}/...` is a valid expression format, but `/my/path/${_time:%Y}/${_time:%m}/${host}/${_time:%Y}/...` (with a repeated `Y`) is not supported.
  * For each path, all extracted dates/times are considered in UTC.

The following strptime format components are allowed: 
*  `'Y'`, `'y'`, for years
*  `'m'`, `'B'`, `'b'`, `'e'`, for months
*  `'d'`, `'j'`, for days 
*  `'H'`, `'I'`, for hours
*  `'M'`, for minutes
*  `'S'`, for seconds

##### Token Syntax

Time-based token syntax follows that of a slightly modified [JS template literal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals):
`${_time: <some_strptime_format_component>}`. Examples: 

Filter | Matches
--- | ---
`/my/path/${_time:%Y}/${_time:%m}/${_time:%d}/...` | `/my/path/2020/04/20/...`
`/my/path/${_time:year=%Y}/${_time:month=%m}/${_time:day=%d}/...` | `/my/path/year=2020/month=05/day=20/...`
`/my/path/${_time:%Y-%m-%d}/...` | `/my/path/2020-05-20/...`

## Autoscale with Collection Jobs

Cribl Stream supports autoscaling for streaming data, but not for collection jobs.

If you run a collection job using Workers in an autoscaling group (such as [an AWS instance](https://docs.aws.amazon.com/autoscaling/ec2/userguide/auto-scaling-groups.html)), you must adequately size your Worker Group or Worker Groups and provision the Worker Nodes before you run or schedule a collection job. Otherwise, a race condition may occur between task scheduling and configuration download from the Leader.

## How Time Filtering Works for Scheduled Jobs {#time-filtering}

* In **Discovery** and **Full Run** modes, time range filters apply before events enter the pre-processing Pipeline.
* In **Preview** mode, filtering defaults to pre-processing, but you can adjust this using the **Where to capture** setting.

### Arrays

If a Collector returns events as an array, make sure you configure your **Event Breaker** correctly. For JSON arrays, you'll need a **JSON array Event Breaker** to separate individual events for proper filtering. You can see examples of this with the REST Collector in [Parse Discover Returns With Arrays as JSON Objects](/collectors-rest#json-object-arrays).

### REST and Database Collectors

REST and Database Collectors support the `${earliest}` and `${latest}` variables. These variables are automatically populated with the UNIX epoch time (in seconds) that represents the lower and upper bounds of the time range you specify when running or scheduling a job.

* **REST Collector**: Use these variables in request URLs, parameters, and headers. See [REST Collector - Time Range Variables](/collectors-rest#time-range) for more information.
* **Database Collector**: Use these variables directly in your SQL queries. See an [SQL query example](collectors-database#sql).

**Important Considerations**:

* For millisecond resolution, multiply these variables by 1,000.
* To get human-readable timestamps, use a function like `C.Time.strftime()`. Otherwise, you'll just get a number.
* Reference the [API documentation](/api-reference) to match the required time format.

**Other Collectors**

Other Collectors do not support `${earliest}` and `${latest}` variables. They use time-based tokens like `${_time:%Y}` in the **Path** or environment variables.

## Troubleshoot

### No Discover or Collector Event Results {#disable-time-filter}

Sometimes after you configure a Collector you might not see any results even after you verified those results exist outside of Cribl Stream and should have appeared in the run results.

**Potential cause:** One potential cause is the way time filtering works in Cribl Stream. There are two instances when time filters impact your data:

- When you submit a call for data within a specific time range.
- When you compare the data within a specific time range in Cribl Stream. 

When you gather data from a Collector and it returns time-bound data, Cribl Stream runs [Event Breakers](event-breakers) on the data to break the incoming data into discrete data events. Each discrete event has its own timestamp. If any of those events are outside the `${earliest}` and `${latest}` variables as defined for the run, Cribl Stream is going to discard those events by default.

**Recommendation** Turn off time filtering for that Collector. Typically this setting is under **Advanced Settings**. Toggle **Disable time filter** on, then retry the run. Note that this setting is not available for all Collectors.

> For additional help with time filtering, see [How Time Filtering Works for Scheduled Jobs](#time-filtering).
>
{.box .info}