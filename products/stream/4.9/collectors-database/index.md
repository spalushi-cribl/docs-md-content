# Database Collector


You can configure Database Collectors to pull events from MySQL, Oracle, Postgres, and SQL Server database management systems (DBMSs). This enables you to combine such structured data with unstructured machine data, and then route combined data to downstream systems of analysis to gain new insights.

## How the Collector Pulls Data

Like other Cribl Stream Collectors, a Database Collector can perform [Preview, Discovery, and Full Run](collectors-schedule-run#run-config) operations in ad hoc or scheduled runs.

Database Collectors rely on Cribl Stream [Database Connections](database-connections) Knowledge objects. Before you configure a Collector, configure a Database Connection to negotiate authenticated communication with the appropriate database type.

> ##### Database Collectors and Worker Nodes
>
> A Database Collector runs a single `collect` task, which only runs on – and executes a SQL query from – a single Worker Process. However, if the Database Collector is scheduled to run – or run in Preview mode – while the first `collect` task is still running, it will start a simultaneous `collect` task on a different Worker Process.
>
{.box .info}

## Configure a Database Collector {#configuring-db-collector}

1. Navigate to **Products** > **Stream** > **Worker Groups**. Select a Worker Group, then go to **Data** > **Sources**. Choose the Source and select **Add Collector**. 
2. In the **New Collector** modal, configure the following under **Collector Settings**:
   - **Collector ID**: Unique ID for this Collector. For example, `sh2GetStuff`. If you clone this Collector, Cribl Stream will add `-CLONE` to the original **Collector ID**.
   - **Description**: Optionally, enter a description.
   - **Connection**: Use the drop-down to select a [Database Connections](database-connections) already configured on your Cribl Stream installation.
   - **SQL query**: Enter a JavaScript expression, enclosed in backticks, that resolves to a query string for selecting data from the database. For details, see [SQL Query Example](#sql).
3. Next, you can configure the following **Optional Settings**: 
   - **Validate query**: Enforces a basic validation, allowing only a single, self-contained `SELECT` statement. Disable for more complex queries or when using semicolons. Defaults to toggled on.
      > Disabling query validation allows potentially destructive queries to be executed, such as data definition language (DDL) and data manipulation language (DML) statements. Such queries can be harmful to your database. Back up your database before running such a query, and proceed with caution.
      {.box .warning}
   - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

4. Optionally, configure any [Result](#result-settings), [Result Routing](#result-routing), and [Advanced](#advanced-settings) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**.
6. To verify that the Collector actually collects data, you can start a single run in the [Preview](/stream/collectors-schedule-run#mode/) mode.

> The sections described below are spread across several tabs. Click the tab links at left to navigate among tabs. 
> 
> Collector Sources currently cannot be selected or enabled in the QuickConnect UI.
>
{.box .info}


### Result Settings {#result-settings}

The Result Settings determine how Cribl Stream transforms and routes the collected data.

#### Event Breakers

In this section, you can apply event breaking rules to convert data streams to discrete events.

- **Event Breaker rulesets**: A list of event breaking rulesets that will be applied, in order, to the input data stream. Defaults to `System Default Rule`.

- **Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

- **Name**: Field name.

- **Value**: JavaScript expression to compute the field's value (can be a constant).

#### Result Routing {#result-routing}

**Send to Routes**: If set to `Yes` (the default), Cribl Stream will send events to normal routing and event processing. Toggle to `No` to select a specific Pipeline/Destination combination to receive the events. The `No` setting exposes these two fields:
- **Pipeline**: Select a Pipeline to process results.
- **Destination**: Select a Destination to receive results.

The default `Yes` setting instead exposes this field: 
- **Pre-processing Pipeline**: Optionally, use the drop-down to select an existing Pipeline to process results before sending them to Routes. 

This final field is always exposed:
- **Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, `GB`, and so forth. (Example: `42 MB`.) Default value of `0` indicates no throttling.

> You might disable **Send to Routes** when you're configuring a Collector that will connect data from a specific Source to a specific Pipeline and Destination. This keeps the Collector's configuration self‑contained and separate from Cribl Stream's routing table for live data – potentially simplifying the Routes structure.
>
{.box .info}

### Advanced Settings {#advanced-settings}

Advanced Settings enable you to customize post-processing and administrative options.

- **Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

- **Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

- **Remove Discover fields** : List of fields to remove from the Discovery results. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface. You can specify wildcards (such as `aws*`).

- **Resume job on boot**: Toggle to `Yes` to resume ad hoc collection jobs if Cribl Stream restarts during the jobs' execution.

### SQL Query Example {#sql}
 Supports only a single statement, and only `SELECT`. Has access to the special `${earliest}` and `${latest}` variables, which will resolve to the Collector run's start and end time. 
     - For Oracle, do not include a semicolon `;` at the end of the **SQL query**.
     - The Database Collector's `earliest` and `latest` variables are formatted using ISO 8601 conventions, unlike other Collectors' corresponding variables.
  
  Here's an example:

```js
`SELECT * FROM table_name WHERE timestamp_field > ${earliest} AND timestamp_field < ${latest} ;`
```

You can also craft a query that says, in effect, "give me everything new since my last collection job." To do this, you specify a **tracking column** in the query. The tracking column (also known as a **rising column**) must be one whose values you know always increase from one record to the next. See the explanation and example [below](#tracking-column).

### Working with a Tracking Column {#tracking-column}

You can enable state tracking under the `State Tracking` section when [scheduling](collectors-schedule-run#schedule-config) or performing a [full run](collectors-schedule-run#full-run) of the Collector. (Preview and Discovery runs do not support state tracking.) With state tracking enabled, the following field will appear:

**Tracking Column**: A numeric column whose values increase monotonically. The Collector will track the column's greatest value seen in a previous run.

You can configure the Collector to use a **tracking column** (also known as a **rising column**). This prevents overlap between jobs, which occurs when the Collector fetches the same record more than once. It likewise prevents the occurrence of gaps between data collected by consecutive jobs. (If your database already has mechanisms that protect against overlap and gaps, you probably don't need to use a tracking column.)

For your tracking column, you can choose any column whose values always increase from one record to the next. (A sequence of values that behaves like this is said to increase [monotonically](https://www.statisticshowto.com/sequence-and-series/monotonic-sequence-series-function/).) Many database tables have monotonically increasing `id`, `time`, and/or `date` columns.

State tracking is available only for scheduled and full ad-hoc collection (but not for preview or discovery ad-hoc runs). Once you configure your Collector, you can enable state tracking and specify a tracking column while [scheduling a job](collectors-schedule-run#state) or performing a full ad-hoc run.

To explain the syntax of queries that work with state tracking, we'll break down a few examples.

#### Tracking Column Queries: A Basic Example

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

#### Tracking Column Queries: Starting in the Middle

The previous example assumes that you want the Collector's first run to start with the first row of your table. Sometimes, though, you might need the Collector to start somewhere in the middle. If so, you'll need to replace the expression `'1 = 1'` with an expression that finds the desired starting point.

For example: 

* If the tracking column is `id` and you want the Collector to begin with the record whose `id` is `4200`, you'd write `id > 4199` instead of `'1 = 1'`. 
* Now, the first Collector run will start where `id`'s value is `4200`, and its saved state will include the location where the Collector leaves off. 
* In all subsequent runs, the initial test of `state && state.id` will evaluate to true – because there is saved state. 
* This means that the Collector will apply `id > ${state.id.value}` and start each new run with next not-yet-collected record.

#### Tracking Column Queries: Advanced Example

Timestamp columns are a popular choice for state tracking. Here's an example:

```sql
SELECT id, text_data, timestamp, UNIX_TIMESTAMP(timestamp) as tracking_timestamp FROM my_table \
WHERE ${state && state.tracking_timestamp ? `UNIX_TIMESTAMP(timestamp) > ${state.tracking_timestamp.value}` : "1=1"} \
ORDER BY tracking_timestamp
```
This query differs from the basic example above in one important way: We use the SQL function `UNIX_TIMESTAMP()` to convert the value in the database's `timestamp` column, from a date to an integer. (This example assumes that we're dealing with a MySQL database, where the `timestamp` column's value will be a date.) 

Cribl Stream requires the tracking column's value to be an integer; and here, we satisfy that requirement by using the `UNIX_TIMESTAMP()` function. In your own queries, depending on the nature of the columns you want to use for tracking, you might be able to adapt this syntax – or you might need to get more creative.

### Working with Stateful Columns {#stateful-columns}

You can configure a Database Collector to process tables only when new rows have been added. Two configurations are possible – one using [stateful sequence columns](#stateful-sequence) and one using [stateful timestamp columns](#stateful-timestamp).

In either case, using a stateful column is a four-step process:

1. Set up a database if you do not already have one.
1. Create a [database connection](#create-db-connection) in Cribl Stream.
1. [Create a Database Collector](#configuring-db-collector).
1. Configure the Database Collector to track the [sequence](#stateful-sequence) or [timestamp](#stateful-timestamp).

The first time your Collector runs, it will process all rows in the relevant table. On subsequent runs, it will process only rows added since the last run.

#### Creating a Database Connection in Cribl Stream {#create-db-connection}

To create a database connection:

1. Select **Processing** > **Knowledge** > **Database Connections**.

1. Click **Add Database Connection**.

1. Give the connection an ID of your choosing.

1. Select the **Database type** appropriate for your application.

1. Enter a connection string that is [appropriate for your database](database-connections#authentication-settings). The following example is for a SQL Server database:

   `Server=<server-name>; Database=<database>;User Id=<user>; Password=<password>;trustServerCertificate=true;`

1. Click **Test Connection** to verify the connection.

1. Click **Save**.

1. Commit and deploy your changes.

#### Stateful Column Queries: Sequence Example {#stateful-sequence}

To set up a Database Collector to use stateful sequence columns:

1. [Create a Collector](#configuring-db-collector).

1. Give the connection an ID of your choosing.

1. Select the [database connection](#create-db-connection) you created previously.

1. For **SQL Query**, input the following string, including the backticks:
```sql
   `SELECT * FROM my_table WHERE ${state && state.id ? `id > ${state.id.value}` : '1 = 1'} ORDER BY id`
```
1. Click **Save**. The Collector modal will close.

1. Commit and deploy your changes.

1. Reopen your Collector and click **Schedule**.

In the scheduling modal:

1. Enable scheduling for the Collector.

1. Under **State Tracking**, set **Enabled** to `Yes` and **Tracking Column** to the name of the column storing your auto-incrementing ID.

1. Click **Save**.

The Collector will process all rows in the table when you run it for the first time. On subsequent runs, it will process only rows added since the last run.

#### Stateful Column Queries: Timestamp Example {#stateful-timestamp}

To set up a Database Collector to use stateful timestamp columns:

1. [Create a Collector](#configuring-db-collector).

1. Give the connection an ID of your choosing.

1. Select the [database connection](#create-db-connection) you created previously.

1. For **SQL Query**, input a string like the following. Include the backticks, and use the table and column names from your own database. In this example, the `DATEDIFF` function is specific to a SQL Server database; the function for your database may differ.

   `` `select <id>, <text_data>, <timestamp>, DATEDIFF(s, '1970-01-01 00:00:00', timestamp) as tracking_timestamp from <table-name> where ${state  && state.tracking_timestamp ? `DATEDIFF(s, '1970-01-01 00:00:00', timestamp) > ${state.tracking_timestamp.value}` : "1=1"} order by tracking_timestamp` ``

1. Click **Save**. The Collector modal will close.

1. Commit and deploy your changes.

1. Reopen your Collector and click **Schedule**.

In the scheduling modal:

1. Enable scheduling for the Collector.

1. Under **State Tracking**, set **Enabled** to `Yes` and **Tracking Column** to the name of the column that contains the timestamp.

1. Click **Save**.

The Collector will process all rows in the table when you run it for the first time. On subsequent runs, it will process only rows added since the last run.

> In this example the SQL Server function `DATEDIFF(s, '1970-01-01 00:00:00', <timestampColumn>)` must be used to convert the database date value to a numeric Epic (seconds since Epoch) value. This conversion is needed because most databases return a formatted timestamp string `mm/dd/yyyy hh:mm:ss (2023-07-11 14:24:18)`, which must be converted to a number for state tracking. In this case, Cribl Stream returns the raw timestamp column as well as the timestamp converted to a number, which is required for state tracking.
> 
> Use a Function like Eval to remove `tracking_timestamp` from events if you don't want to send this field to a Destination.
>
{.box .success}

## Manage Collector State

See the Manage Collector State topic for instructions on [editing](collectors-manage-state#edit-state) or [deleting](collectors-manage-state#delete-state) Collector state.

## Internal Fields {#internal-fields}

Cribl Stream uses a set of internal fields to assist in data handling. These "meta" fields are **not** part of an event, but they are accessible, and you can use them in [Functions](functions) to make processing decisions.

Relevant fields for this Collector:

* `__collectible` – This object's nested fields contain metadata about each collection job.
  * `collectorType`: Indicates the type of Collector used for the job.
  * `collectorId`: Represents the **Collector ID** of the Collector, as configured during setup.
* `__inputId` – Uniquely identifies the origin of data for a [collection job](collectors-schedule-run). Its format varies depending on whether the job is ad hoc or scheduled:
  * Ad hoc jobs are formatted as `collection:<timestamp>.<randomId>.adhoc.<Collector ID>`.
    * `<timestamp>`: The Unix timestamp when the job was initiated.
    * `<randomId>`: A random identifier to ensure uniqueness.
    * `adhoc`: Indicates the job was manually triggered.
    * `<Collector ID>`: The ID of the Collector.
  * Scheduled jobs are formatted as `collection:<Collector ID>`.
    * `<Collector ID>`: The ID of the Collector.
