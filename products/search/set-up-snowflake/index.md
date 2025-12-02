# Connect Cribl Search to Snowflake


Configure Cribl Search to query your Snowflake warehouse.

---

[Snowflake](https://www.snowflake.com/) is a cloud platform that offers data warehousing as a service.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to [search](search-overview) a Snowflake warehouse.

## Add a Snowflake Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add a Snowflake
Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.

Next, configure the resulting **New Dataset Provider** modal as follows:

1. In **ID**, enter a unique identifier for the Dataset Provider. This is how you'll reference it when assigning
   Datasets to it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. Optionally, add **Description** to quickly describe the Dataset Provider.
1. Set **Dataset Provider Type** to **Snowflake**.
1. Enter the following:
   - **Account identifier**: Case-sensitive Snowflake
     [account identifier](https://docs.snowflake.com/en/user-guide/admin-account-identifier#using-an-account-name-as-an-identifier),
     in the format `<org_name>-<account_name>`. For example: `myorg-account123`.
   - **Username**: Case-sensitive Snowflake username.
   - **Private key**: The contents of your Snowflake
     [private key](https://docs.snowflake.com/en/user-guide/key-pair-auth#generate-the-private-key) (PEM) file. To
     upload a file, select the upload button at this field's upper right.
   - **Private key passphrase**: An optional
     [passphrase](https://docs.snowflake.com/en/user-guide/key-pair-auth#generate-the-private-key) used to protect your
     Snowflake private key.
   - **Executors limit**: Maximum number of concurrent Cribl Search [executors](search-details#executors) dispatched for
     processing Snowflake data partitions.
   - **Endpoint**: Optional URL to Snowflake REST API. If empty, defaults to
     `https://<org_name>-<account_name>.snowflakecomputing.com`.
1. Select **Save** when finished.

## Add a Snowflake Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.

Next, configure the resulting **New Dataset** modal as follows:

1. In **ID**, enter a unique identifier for the Dataset. You'll use this to specify the Dataset in a query's
   [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. Optionally, add **Description** to quickly describe the Dataset.
1. Set **Dataset Provider** to the ID of a [Snowflake Dataset Provider](#provider).
1. Enter the following:
   - **Warehouse**: Case-sensitive name of the Snowflake
     [warehouse](https://docs.snowflake.com/en/user-guide/warehouses). If not set, defaults to the `DEFAULT_WAREHOUSE`
     property for the Snowflake user.
   - **Database name**: Case-sensitive name of the Snowflake
     [database](https://docs.snowflake.com/en/guides-overview-db). If not set, defaults to the database value in the
     `DEFAULT_NAMESPACE` for the Snowflake user.
   - **Schema name**: Case-sensitive name of the Snowflake schema that contains the tables you plan to query. If not
     set, defaults to the schema value in the `DEFAULT_NAMESPACE` for the Snowflake user.
   - **Table or query**: The name of the table or view, or a query. May be a simple name (`logs`) or an SQL query
     (`select * from logs`).
   - **Timestamp field**: Optional, case-sensitive name of the column that holds the timestamp of the event to query.
   - **Role**: Optional Snowflake
     [role](https://docs.snowflake.com/en/user-guide/security-access-control-overview#roles) to use when running the SQL
     query. We recommend using a
     [read-only role](https://docs.snowflake.com/en/user-guide/security-access-control-configure#creating-custom-read-only-roles).
1. Select **Save** when finished.

## List the Contents of a Snowflake Dataset

You can quickly list all [tables and views](https://docs.snowflake.com/en/guides-overview-db) accessible through a
[Snowflake Dataset](#dataset), by using the [`.show objects`](show-objects) command. For example:

```kusto
.show objects(snowflake_dataset_ID)
```

## Search a Snowflake Dataset

Now that you have a Snowflake [Dataset Provider](#provider) and [Dataset](#dataset), you're ready to
[start searching](build-a-search).

Cribl Search submits the query to Snowflake. Results will start showing up after the Snowflake SQL query completes. How
long it takes depends on how much data there is in your Snowflake warehouse. Searching columns that contain large
strings may result in a slower search.

## Snowflake: Supported SQL Data Types

When querying Snowflake, Cribl Search supports the following SQL data types:

- `NUMBER`, `DECIMAL`, `DEC`, `NUMERIC`, `INT`, `INTEGER`, `BIGINT`, `SMALLINT`, `TINYINT`, `BYTEINT`, `FLOAT`,
  `FLOAT4`, `FLOAT8`, `DOUBLE`, `DOUBLE PRECISION`, `REAL`
- `VARCHAR`, `CHAR`, `CHARACTER`, `STRING`, `TEXT`
- `BOOLEAN`
- `DATE`, `DATETIME`, `TIME`, `TIMESTAMP`, `TIMESTAMP_LTZ`, `TIMESTAMP_NTZ`, `TIMESTAMP_TZ`
- `VARIANT`, `OBJECT`

