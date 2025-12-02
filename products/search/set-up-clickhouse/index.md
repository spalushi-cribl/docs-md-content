# Connect Cribl Search to ClickHouse


Configure Cribl Search to query ClickHouse Cloud or a self-managed ClickHouse server.

---

[ClickHouse](https://clickhouse.com/) is a column-oriented SQL database management system for online analytical
processing, available both as open-source software and a cloud offering.

To start [searching](search-overview) your ClickHouse databases with Cribl Search, you need:

- A [ClickHouse Dataset Provider](#provider), which contains access credentials, and specifies what ClickHouse server or
  Cloud service to query.
- A [ClickHouse Dataset](#dataset), which points Cribl Search at a specific table, view, or query in a ClickHouse
  database.

Each Dataset Provider can have multiple Datasets assigned to it.

## Add a ClickHouse Dataset Provider {#provider}

Before adding a ClickHouse Dataset Provider, do the following:

1. Make sure your ClickHouse server or Cloud service has the
   [HTTP interface](https://clickhouse.com/docs/en/interfaces/http) enabled. Ideally, use HTTPS over the default
   port 8443.
1. Check your ClickHouse URL, username, and password.

To add a ClickHouse Dataset Provider in Cribl Search:

1. Log in to Cribl Search as an Admin or Editor [Search Member](cloud-managing#search-member-roles).
1. Select **Data**, then **Dataset Providers**, then **Add Provider**.
1. In **ID**, enter a unique identifier. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_clickhouse_database_1`).<br>This is how you or other Search
   Members reference your Dataset Provider when adding [Datasets](#dataset) to it.
1. In **Description**, you can briefly describe your Dataset Provider, so that other Search Members know its purpose.
1. Set **Dataset Provider Type** to **ClickHouse**.
1. Enter the following:
   - **Username**: Your ClickHouse username, for authentication.
   - **Password**: Your ClickHouse password, for authentication.
   - **Endpoint**: Your ClickHouse server or Cloud service URL.
1. Select **Save**.

Now, you're ready to add a [ClickHouse Dataset](#dataset).

## Add a ClickHouse Dataset {#dataset}

With a [ClickHouse Dataset Provider](#provider) ready, you can start adding ClickHouse Datasets:

1. Log in to Cribl Search as an Admin or Editor [Search Member](cloud-managing#search-member-roles).
1. Select **Data**, then **Datasets**, then **Add Dataset**.
1. In **ID**, enter a unique identifier. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `clickhouse_dataset_1`).<br>You and other Search Members will
   reference this ID when [searching](build-a-search#syntax) your Dataset (`dataset="clickhouse_dataset_1"`).
1. In **Description**, you can briefly describe your Dataset, so that other Search Members know its purpose.
1. In **Select a Provider**, pick the ID of a [ClickHouse Dataset Provider](#provider).
1. Enter the following:
   - **Database name**: Case-sensitive name of the ClickHouse database. If not set, defaults to the
     [ClickHouse `default_database` setting](https://clickhouse.com/docs/en/operations/server-configuration-parameters/settings#default_database).
   - **Table name, view, or query**: The name of the table or view, or a query. May be a simple name (`logs`) or an SQL
     query (`select * from logs`). Mind that for queries, and for tables without sorting keys, results will be limited
     to 100,000.
   - **Timestamp field**: Optional, case-sensitive name of the column that holds the timestamp of the event to query.
1. Select **Save**.

## List the Contents of a ClickHouse Dataset {#list-contents}

You can quickly list all tables and views accessible through a [ClickHouse Dataset](#dataset), using the
[`.show objects`](show-objects) command. For example:

```kusto
.show objects(clickhouse_dataset_ID)
```

## Search a ClickHouse Dataset {#search}

After adding a [ClickHouse Dataset Provider](#provider) and [Dataset](#dataset), you're ready to
[start searching](build-a-search).

Cribl Search submits the query to ClickHouse. Results will start showing up after the ClickHouse SQL query completes.
How long it takes depends on how much data there is in your ClickHouse database. Searching columns that contain large
strings may result in a slower search.

## Supported ClickHouse Data Types {#data-types}

Cribl Search supports the following [ClickHouse data types](https://clickhouse.com/docs/en/sql-reference/data-types):

- Strings: `String`, `FixedString`
- Integer types: `UInt8`, `UInt16`, `UInt32`, `UInt64`, `UInt128`, `UInt256`, `Int8`, `Int16`, `Int32`, `Int64`,
  `Int128`, `Int256`
- Floating-point numbers: `Float32`, `Float64`, `Decimal`
- Boolean: `Bool`
- Dates: `Date`, `Date32`, `DateTime`, `DateTime64`
- `UUID`
- Low cardinality types: `Enum`, `LowCardinality`
- `SimpleAggregateFunction`
- `Nullable`
- IP addresses: `IPv4`, `IPv6`

The following ClickHouse data types are **not supported**:

- `JSON`
- `Array`
- `Map`
- `AggregateFunction`
- `Tuple`
- Nested data structures (`Nested`)
- Geo types: `Point`, `Ring`, `Polygon`, `MultiPolygon`
- Special data types (including `Expression`, `Set`, `Nothing` and `Interval`)

## Known ClickHouse Limitations {#limitations}

Searching ClickHouse comes with the following limitations:

- **Limited unsorted data:** When your [ClickHouse Dataset](#dataset) targets a query, or a table without sorting keys, the maximum number of
  results is 100,000.
- **ClickHouse Cloud startup lag:** If you're using [ClickHouse Cloud](https://clickhouse.com/cloud), and the service is not awake, the first search might be slow as ClickHouse wakes the service. This can take 20–30 seconds or more.
- **Structured data type parsing:** ClickHouse will error out if a parameter's value for structured data types like Bool, IPv4, IPv6, or UUID cannot be parsed. For example, using a value like `1.0.0` for an IPv4 column will fail with an error of the form: `Code: 675. DB::Exception: Cannot parse IPv4: value 1.0.0 cannot be parsed as IPv4 for query parameter.`
- **Wildcard/regex matching on non-string columns:** The `startswith` operator, and others that use `ilike` in ClickHouse SQL, will fail with an error for non-string columns such as `UUID` or `Int32`. For example, attempting to match `*2345*` on a numeric column will trigger an error of the form: `DB::Exception: Illegal type Int32 of argument of function ilike`. As a workaround, you can force a cast to `toString()` for regex and wildcard matches on non-string columns, to ensure that they work correctly.
- **Operators with incorrect formats:** Operators like `!=`, `==`, `<`, `>`, `<=`, `>=` will fail if the data format is incorrect. For example, comparing `1.2.3` with an IPv4 column will trigger an error if `1.2.3` is not a valid IP. Although casting to `toString()` is a potential workaround, it’s better to validate the format first, to benefit from ClickHouse's type advantages.
  

