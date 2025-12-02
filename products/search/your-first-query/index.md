# Write Your First Query


Write simple queries in Kusto Query Language, using operators such as [`limit`](limit), [`project`](project), [`where`](where),
[`count`](operators-count), [`order`](order), and others.

---

Cribl Search is based on Kusto Query Language (KQL), which lets you delve into your data to discover patterns, identify
anomalies and outliers, and create statistical models.

>To generate queries from natural language, try the [Cribl Copilot KQL assistant](copilot-kql). 
>
{.box .info}

### Example Scenario

Imagine you're a security operations analyst working for a company that relies heavily on cloud infrastructure. You want
to analyze a Dataset containing virtual private cloud (VPC) flow logs for insight into network traffic patterns,
potential security threats, and resource utilization in different regions.

### What You’ll Learn

You'll write simple queries in KQL to analyze a sample Dataset of VPC flow logs. You'll learn how to:

- Understand the structure of a Cribl Search query.
- Return a specific number of rows by using the [`limit`](limit) operator.
- Extract information from the `_raw` field using the [`extract`](extract-operator) operator and Parser.
- Filter data by using the [`where`](where) operator.
- Calculate available events by using the [`count`](operators-count) operator.

### Your Learning Goals

By the end of this tutorial, you'll be able to write a query with the most commonly used operators in KQL to analyze VPC
flow logs, understand network traffic patterns, and identify potential security threats within your cloud
infrastructure.

## The Structure of a Cribl Search Query {#structure}

Organizations across various sectors process vast amounts of data and aim to extract valuable insight from it. In the
VPC flow logs example, you have a Dataset with network traffic data from your organization's cloud infrastructure. This
tutorial will guide you through the fundamental structure of the Kusto Query Language to analyze and interpret the
Dataset.

### What's a Cribl Search Query?

A Cribl Search query is a read-only request expressed in plain text that processes data and returns results in a tabular
or graphical format. It consists of one or more query statements. The predominant type of query statement is a tabular
expression statement.

### What's a Tabular Statement?

In a tabular expression statement, both input and output consist of tables or tabular Datasets.

Tabular statements hold zero or more operators, each beginning with tabular input and yielding tabular output. Operators
are connected by a pipe (|), allowing data to flow from one operator to the next for filtering or manipulation. This
sequential piping of information makes the query's operator order crucial.

Think of it like a funnel, where you start with a full data table. The data is filtered, rearranged, or summarized as it
passes through each operator. At the end of the funnel, you obtain a refined output.


![Data filtered through a funnel](search-data-funnel.png)
{scale="100%" border="false"}

Here's an example query:

```kusto {runSearch=true}
dataset="cribl_internal_logs" method=*
| where response_time > 1 and method != 'PATCH'
| summarize cnt=count() by method
| where cnt > 42
```

This query contains a single tabular expression statement that references a Dataset called "cribl_internal_logs" and
filters the data based on the method column. The data rows are filtered by the response_time column's value and exclude
rows with the method "PATCH". The query then summarizes the count of rows for each method and filters the results to
only display methods with a count greater than 42.


![Results of a query with a tabular expression statement](search-tabular-query.png)
{scale="100%"}

## Return a Specific Number of Rows by Using the `limit` Operator {#rows}

You can use a Cribl Search query to explore Datasets and gain insights. In this scenario, you have an unfamiliar Dataset
of VPC flow logs called "cribl_search_sample" that ships out of the box with Cribl Search. You want to explore what you
can learn from this data.

In this task, you'll examine the structure of the data to understand the kinds of questions you can ask about these
network flows.

### Write Your First Query

This query returns a small subset of the data to help you familiarize yourself with the columns and types of data in the
table.

The [`limit`](limit) operator is perfect for this task, as it returns a specific number of arbitrary rows.

1. Copy this query to your clipboard:
   ```kusto {runSearch=true}
   dataset="cribl_search_sample" | limit 100
   ```
2. Paste the query into the search box.<br> Tip: You can [autoformat](build-a-search#autoformat-query-text) the query
   text.
3. Notice that the query begins with a reference to the data table, `cribl_search_sample`. This data is piped into the
   first and only operator, which then selects 100 arbitrary rows.
4. Run the query by selecting **Search** or pressing **Enter**.
5. Review the results. The sample Dataset includes various data columns such as `_raw`, `source`, `host`, `dataSource`,
   and `datatype`.


![Results of a query using the `limit` operator](search-limit-query.png)
{scale="100%"}

## Extract Information from the `_raw` Field by Using the `extract` Operator and Parser {#extract}

Notice in the previous example that the useful information in the `_raw` field is unparsed. To analyze the data
effectively, you need to extract and parse the information into separate fields.

### Write a Query to Parse the `_raw` Field

To parse the data, you can use the [`extract`](extract-operator) operator along with the appropriate [Parser](parsers) (in this
case, **AWS VPC Flow Logs**).

1. Copy this query to your clipboard:
   ```kusto {runSearch=true}
   dataset="cribl_search_sample" | limit 1000 | extract parser='AWS VPC Flow Logs'
   ```
2. Paste the query into the search box and select **Search** or press **Enter**.
3. Check that your parsed results are similar to the following example. The actual data in the rows might differ because
   the rows are selected arbitrarily. The parsed results should now contain separate fields for each data point in the
   `_raw` field.


![Results of a query using the extract operator](search-parse-query.png)
{scale="100%"}

Now that the data is parsed and organized into separate fields, you can perform further analysis and create more
advanced queries to gain insight into the VPC flow logs.

## Filter Data by Using the `where` Operator {#filter}

In the previous sections, you learned how to extract and parse information from the VPC flow logs Dataset. Now let's
explore how to filter the data using the [`where`](where) operator to answer specific questions about the VPC flow logs.

### Use the `where` Operator

All the operators you've used so far have returned selected columns. Now let's take a look at specific rows of the data.

The [`where`](where) operator filters results that satisfy a certain condition. In this first example, you'll compare an
integer column to a minimum value by using the numerical operator greater-than (`>`). Specifically, you want to see only 
flows that have a certain number of packets. So you'll look at rows of data where the number of packets is greater than
a given threshold.

Run the following query:

```kusto {runSearch=true}
dataset="cribl_search_sample"
| extract parser='AWS VPC Flow Logs'
| where packets > 5
| project _time, srcaddr, dstaddr, packets
| take 10
```

The `extract parser` line is superfluous, because Cribl Search applies the parser automatically, but it's included for illustration.

Notice that all rows returned have packet values greater than 5.


![Results of a query using the where operator to return values](search-filter-query-where.png)
{scale="100%"}

Similarly, you can filter for flow events that have a start time that’s more recent than a certain time. For example,
run the following query, where `earliest=-30m` means last 30 minutes:

```kusto {runSearch=true}
dataset="cribl_search_sample" earliest=-30m
| extract parser='AWS VPC Flow Logs'
| where packets > 5
| summarize count() by srcaddr, dstaddr
```


![Results of a query using the where operator to filter for start times](search-filter-query-start-time.png)
{scale="100%"}

#### Filter by Using a String Value

Let's narrow down the flows to those that were accepted. Run the following query, which uses a second [`where`](where)
operator with the string value "ACCEPT":

```kusto {runSearch=true}
dataset="cribl_search_sample" bytes > 100 and action = "ACCEPT"
| limit 1000
```

Notice that all records returned from this query have the action "ACCEPT" and bytes greater than 100.


![Results of a query using the where operator to filter for a string value](search-filter-query-accepted.png)
{scale="100%"}

### Filter by Using the `has` Operator

You can search for specific strings in the flow log data by using the [`has`](has) operator. The [`has`](has) operator
is a case-insensitive search that matches a full term.

For example, let's filter the VPC flow logs by a specific interface ID:

```kusto {runSearch=true}
dataset="cribl_search_sample"
| extract parser='AWS VPC Flow Logs'
| where packets > 5
| where interface_id has "eni-07e5d76940fbaa5c6"
```


![Results of a query using the has operator](search-filter-query-has.png)
{scale="100%"}

### Filter on Time Range Values

Let's look more closely at the flows within a specific time range. Because time ranges are bounded by two extremes, it's
most efficient to limit your query to events within these two times.

To construct a date range, use `earliest` and `latest` at the top level, with the following syntax:
`[+|-]#TimeUnit[@SnapToTimeUnit]`

Let's incorporate the time range into a kind of query you've already seen. Run the following query:

```kusto {runSearch=true}
dataset="cribl_search_sample" earliest=-35m latest=-5m
| limit 1000
| extract parser='AWS VPC Flow Logs'
| where bytes > 100
```


![Results of a query filtering on a time range](search-filter-query-time-range.png)
{scale="100%"}

In this query, you filter the events in the Dataset by a specific time range, from 35 minutes ago to 5 minutes ago, using the
`earliest` and `latest` operators. You then extract and parse the data using the `AWS VPC Flow Logs` Parser. To focus on
flows with more than 100 bytes and action “ACCEPT”, use the [`where`](where) operator with the respective conditions.

Finally, you use the [`timestats`](timestats) operator to aggregate the data using a daily time span (`span=1d`) and
group the results by `srcaddr`.

The query will return results that show the aggregated statistics of VPC flows with action "ACCEPT" and bytes greater
than 100 for each `srcaddr`, separated by day, during the specified time period.

## Calculate Available Events by Using the `count` Operator {#calculate}

Now that you’ve refined your query using helpful operators, such as the [`limit`](limit) operator that returns a
specified number of events, suppose you want to determine the total number of events available within a specific
Dataset.

In this case, you can use the [`count`](operators-count) operator. For example, say you want to count VPC flow events
where bytes are greater than 100 after extracting the logs using the AWS VPC Flow Logs parser.

To do this, run this query:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| limit 1000
| where bytes > 100
```


![Results of a query using the count operator](search-count-query.png)
{scale="100%"}

In this example, you first use the [`extract`](extract-operator) operator to parse the data in the `cribl_search_sample`
Dataset using the `AWS VPC Flow Logs` Parser. Next, filter the extracted VPC flow logs for flows with bytes greater
than 100. Finally, use the [`count`](operators-count) operator to calculate the total number of events that meet this
criterion.

The query will return a single record containing the count of VPC flow events with bytes greater than 100.

## Summary

Throughout this tutorial, you’ve explored various aspects of Cribl Search queries and their application to the analysis
of VPC flow logs. Here's a recap of what you've learned:

1. **Return a specific number of rows by using the [`limit`](limit) operator**

   You learned how to get a sample of rows from the Dataset using the [`limit`](limit) operator, which provided quick
   insight into the structure of the data.

2. **Extract information from the `_raw` field by using the extract operator and Parser**

   You extracted and parsed information from the `_raw` field by applying the [`extract`](extract-operator) operator
   along with the `AWS VPC Flow Logs` Parser.

3. **Filter data by using the [`where`](where) operator**

   You explored how to filter the data using the [`where`](where) operator to answer specific questions about the VPC
   flow logs, such as filtering based on certain conditions, string values, or time ranges.

4. **Calculate the number of events by using the `count` operator**

   You learned how to use the [`count`](operators-count) operator to find the total number of input events in a Dataset
   that satisfy specific conditions, allowing you to get a quick overview of the data that met your filtering criteria.

By mastering these operators and techniques, you can effectively analyze and gain insights from the VPC flow logs
Dataset. Armed with this knowledge, you can perform more advanced data analysis, create more complex queries to answer
specific questions, and better understand the behavior and trends within your network flows.

