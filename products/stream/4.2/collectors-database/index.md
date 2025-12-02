# Database



You can configure Database Collectors to pull events from database management systems (DBMSs). This enables you to combine such structured data with unstructured machine data, and then route combined data to downstream systems of analysis to gain new insights. 

Cribl Stream 4.1 and later support integrating this Collector with MySQL, SQL Server, and Postgres databases.

## How the Collector Pulls Data

Like other Cribl Stream Collectors, a Database Collector can perform [Preview, Discovery, and Full Run](collectors-schedule-run#run-config) operations in ad hoc or scheduled runs.

Database Collectors rely on Cribl Stream [Database Connections](database-connections) Knowledge objects. Before you configure a Collector, configure a Database Connection to negotiate authenticated communication with the appropriate database type.

> ##### Database Collectors and Worker Nodes
>
> A Database Collector runs a single `collect` task, which only runs on – and executes a SQL query from – a single Worker Process. However, if the Database Collector is scheduled to run – or run in Preview mode – while the first `collect` task is still running, it will start a simultaneous `collect` task on a different Worker Process.
>
{.box .info}

## Configuring a Database Collector 

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, select **Data** > **Sources**, then select **Collectors** > **Database** from the **Manage Sources** page's tiles or left nav. Click **New Collector** to open a modal that provides the following options and fields.

> The sections described below are spread across several tabs. Click the tab links at left to navigate among tabs. Click **Save** when you've configured your Collector.
> 
> Collector Sources currently cannot be selected or enabled in the QuickConnect UI.
>
{.box .info}

### Collector Settings

The Collector Settings determine how data is collected before processing.

**Collector ID**: Unique ID for this Collector. E.g., `sh2GetStuff`.

**Connection**: Use the drop-down to select a [Database Connections](database-connections) already configured on your Cribl Stream installation.

**SQL query**: Enter a JavaScript expression, enclosed in backticks, that resolves to a query string for selecting data from the database. Supports only a single statement, and only `SELECT`. Has access to the special `${earliest}` and `${latest}` variables, which will resolve to the Collector run's start and end time. Example:

```
`SELECT * FROM table_name WHERE timestamp > ${earliest} AND timestamp_field < ${latest} ;`
```

You can also craft a query that says, in effect, "give me everything new since my last collection job." To do this, you specify a **tracking column** in the query. The tracking column (also known as a **rising column**) must be one whose values you know always increase from one record to the next. See the explanation and example [below](#tracking-column).

### Optional Settings

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Result Settings

The Result Settings determine how Cribl Stream transforms and routes the collected data.

#### Custom Command

In this section, you can pass the data from this Collector to an external command for processing, before the data continues downstream.

**Enabled**: Defaults to `No`. Toggle to `Yes` to enable the custom command.

**Command**: Enter the command that will consume the data (via `stdin`) and will process its output (via `stdout`).

**Arguments**: Click **Add Argument** to add each argument to the command. You can drag arguments vertically to resequence them.

#### Event Breakers

In this section, you can apply event breaking rules to convert data streams to discrete events.

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied, in order, to the input data stream. Defaults to `System Default Rule`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute the field's value (can be a constant).

#### Result Routing

**Send to Routes**: If set to `Yes` (the default), Cribl Stream will send events to normal routing and event processing. Toggle to `No` to select a specific Pipeline/Destination combination to receive the events. The `No` setting exposes these two fields:
- **Pipeline**: Select a Pipeline to process results.
- **Destination**: Select a Destination to receive results.

The default `Yes` setting instead exposes this field: 
- **Pre-processing Pipeline**: Optionally, use the drop-down to select an existing Pipeline to process results before sending them to Routes. 

This final field is always exposed:
- **Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, `GB`, etc. (Example: `42 MB`.) Default value of `0` indicates no throttling.

> You might disable **Send to Routes** when you're configuring a Collector that will connect data from a specific Source to a specific Pipeline and Destination. This keeps the Collector's configuration self‑contained and separate from Cribl Stream's routing table for live data – potentially simplifying the Routes structure.
>
{.box .info}

### Advanced Settings

Advanced Settings enable you to customize post-processing and administrative options.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Remove Discover fields** : List of fields to remove from the Discover results. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface. You can specify wildcards (such as `aws*`).

**Resume job on boot**: Toggle to `Yes` to resume ad hoc collection jobs if Cribl Stream restarts during the jobs' execution.

### Working with a Tracking Column {#tracking-column}

You can configure the Collector to use a **tracking column** (also known as a **rising column**). This prevents overlap between jobs – overlap being when the Collector fetches the same record more than once. It likewise prevents the occurrence of gaps between data collected by consecutive jobs. (If your database already has mechanisms that protect against overlap and gaps, you probably don't need to use a tracking column.)

For your tracking column, you can choose any column whose values always increase from one record to the next. (A sequence of values that behaves like this is said to increase [monotonically](https://www.statisticshowto.com/sequence-and-series/monotonic-sequence-series-function/).) Many database tables have monotonically-increasing `id`, `time`, and/or `date` columns.

State tracking is available only for scheduled collection (and not ad hoc collection). Once you configure your Collector, it's in the process of [scheduling its jobs](collectors-schedule-run#state) that you enable state tracking and specify a tracking column.

To explain the syntax of queries that work with state tracking, we'll break down a few examples.

#### Tracking Column Queries: A Basic Example

```sql
`SELECT * FROM my_table WHERE ${state && state.id ? `id > ${state.id.value}` : '1 = 1'} ORDER BY id`
```

The `SELECT` statement must satisfy one requirement: that the tracking column be among the columns named. `SELECT * FROM my_table` satisfies this requirement because the wildcard (`*`) means "all columns." 

The `WHERE` clause illustrates the key techniques required for state tracking:
* First, we verify that state has been saved (on a prior run of the Collector); and, if it has, that it includes the desired tracking column – for this example, a column named `id`. 
    ```sql
    WHERE ${state && state.id
    ```
* If state **has** been saved, we'll apply the test right after the `?` in the ternary operator. This determines whether the value of the `id` column for a given row is greater than the last-saved value for that column. If it is not, that means that the row was collected already, and should not be collected again.
    ```sql
    ? `id > ${state.id.value}` : '1 = 1'}`
    ```
* If state has **not** been saved, this means that the Collector is now about to run **for the first time**. 

> Calculated temporary columns cannot be used directly in the `WHERE` clause. The `WHERE` clause can only reference existing columns or aliases of existing columns.
>
{.box .warning}

The example assumes that we want to collect all rows. To do this, we need an expression that always evaluates to true, so every row gets collected. The expression `'1 = 1'` after the ternary operator's colon (`:`) does exactly that.

Finally, the `ORDER BY` clause, `ORDER BY id`, guarantees that the query returns rows such that the value of the tracking column (here, `id`) increases monotonically from row to row. For state tracking to work correctly, rows **must** be returned such that the value of the tracking column increases monotonically. Some databases have mechanisms that achieve this without an `ORDER BY` clause, but one way or another, this requirement must be satisfied.

#### Tracking Column Queries: Starting in the Middle

The previous example assumes that you want the Collector's first run to start with the first row of your table. Sometimes, though, you might need the Collector to start somewhere in the middle. If so, you'll need to replace the expression `'1 = 1'` with an expression that finds the desired starting point.

For example: 

* If the tracking column is `id` and you want the Collector to begin with the record whose `id` is `4200`, you'd write `id > 4199` instead of `'1 = 1'`. 
* Now, the first Collector run will start where `id`'s value is `4200`, and its saved state will include the location where the Collector leaves off. 
* In all subsequent runs, the initial test of `state && state.id` will evaluate to true – because there is saved state. 
* This means that the Collector will apply `id > ${state.id.value}` and start each new run with next not-yet-collected record.

#### Tracking Column Queries: Advanced Example

Timestamp columns are a popular choice for state tracking. Here's an example:

```sql
SELECT id, text_data, timestamp, UNIX_TIMESTAMP(timestamp) as tracking_timestamp FROM my_table \
WHERE ${state && state.tracking_timestamp ? `UNIX_TIMESTAMP(timestamp) > ${state.tracking_timestamp.value}` : "1=1"} \
ORDER BY tracking_timestamp
```
This query differs from the basic example above in one important way: We use the SQL function `UNIX_TIMESTAMP()` to convert the value in the database's `timestamp` column, from a date to an integer. (This example assumes that we're dealing with a MySQL database, where the `timestamp` column's value will be a date.) 

Cribl Stream requires the tracking column's value to be an integer; and here, we satisfy that requirement by means of the `UNIX_TIMESTAMP()` function. In your own queries, depending on the nature of the columns you want to use for tracking, you might be able to adapt this syntax – or you might need to get more creative.

## Clearing a Collector's State {#clear-state}

You'll find the **Clear state** button along the bottom edge of the Database Collector's configuration modal, between the **Scheduled** and **Delete Collector** buttons. This button exists because deleting the Collector does **not** delete its saved state information. In this section, we'll explore the benefits of decoupling these two operations.

To begin with, note that Collector state is stored on disk, not in Git. Each Collector's state is stored separately.

Secondly, the uses of state-clearing differ depending on whether your Database Collector is on a single-instance deployment of Cribl Stream, or a distributed deployment.

#### Clearing Collector State in a Single-Instance Deployment

When you first configure a Database Collector, you might need to experiment with SQL queries until you arrive at one that returns the desired results. In doing this, you might need the queries you're testing to always start from the same place in the database. If you're using a tracking column, this won't work, because each query will move you further along in the database. The solution is to clear state between iterations of the query you're building.

#### Clearing Collector State in a Distributed Deployment

Just like in single-instance deployments, in distributed deployments it can be useful to clear Collector state when you're testing SQL queries for a Collector with state tracking enabled.

Beyond that, with distributed deployments it's important to know that Cribl Stream decouples the deleting of Collectors themselves from the deleting of their saved state, and to see the benefits of that decoupling.

Suppose you have been running a Collector with state tracking enabled, and it's working its way through a database. At some point you decide you're not happy with that Collector and you click the **Delete Collector** button. What happens next? There are at least three possibilities:

* If you're sure you want to delete the Collector, you should Commit and Deploy. Once you do that, the Leader pushes the change out to the Worker Nodes, and the Collector is gone (although you can recover it by rolling back to a previous commit in Git).

* If you realize that you made a mistake, and want to keep the Collector after all, then you should **not** Commit and Deploy. Instead, click the **Undo All Changes** button in the **Git Changes** modal – this un-deletes the Collector. And, since the Collector's state remains on disk,  the Collector will pick up where it left off.

* If you deleted the Collector because you want to create another Collector with the same ID, the same tracking column, but an otherwise different configuration – here, you should go ahead and Commit and Deploy, then create your new Collector. Because the old Collector's state remains on disk, the new Collector with the same ID picks up the saved state. When you begin using the new Collector, it will pick up where the old Collector left off. On the other hand, if you prefer the new Collector to start from zero, just click the **Clear state** button before you start using it.

The moral of the story is that Cribl Stream's decoupling of Collector deletion from state deletion gives you control, such that you can avert messy situations where your Collector cannot find the state it needs.
