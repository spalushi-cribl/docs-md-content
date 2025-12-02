# Searching Amazon S3


Learn how to search your Amazon S3 data.

---

Cribl Search supports searching [Amazon S3](https://aws.amazon.com/s3/) buckets. You need a Dataset Provider and a
Dataset, see the setup guide for [Amazon S3](set-up-s3) and [Data Lake Amazon S3](set-up-data-lake-amazon-s3) for
details.

This document provides examples of searching objects from Amazon S3. To start, navigate to **Search Home**, where [searches](build-a-search) are run.

## Examples {#examples}

The following examples reference a built-in Dataset with the **ID** `cribl_search_sample`.

### Basic Search {#basic}

This search specifies the Dataset and limits the number of results.

```kusto {runSearch=true}
dataset="cribl_search_sample" source=*vpc*
| limit 100
```

![Basic search](search-s3-basic-example.png)
{scale="100%"}

### Built-in Parser {#parser}

Cribl Search comes with parsing libraries so you don't have to write them manually. Use the
[`extract`](extract-operator) operator to reference the built-in Parser for VPC Flow Logs.

```kusto {runSearch=true}
dataset="cribl_search_sample" source=*vpc*
| limit 100
| extract parser='AWS VPC Flow Logs'
```

Note the additional fields shown in the [field browser](search-overview#browser).

![Parser search](search-s3-parser-example.png)
{scale="100%"}

If you want to see the built-in parser's effects, first run the `extract` command below. Then add the `extract` command:

```kusto {runSearch=true}
dataset="$vt_dummy"
 | extend _raw = "2 602320997947 eni-5nxjlyb0xm0o96kgs 52.216.176.35 10.0.0.138 1311 3389 6 7 7784 1730263536 1730263542 ACCEPT OK"

// Run the above portion first to see the logs unparsed. Add the extract operator below to show the built-in parser in action.
 | extract parser='AWS VPC Flow Logs'
```

### Aggregate {#aggregate}

To aggregate, we'll use the [`summarize`](summarize) operator with the [count function](count) grouping by IP address
pairs.

```kusto {runSearch=true}
dataset="cribl_search_sample" source=*vpc*
| limit 1000
| extract parser='AWS VPC Flow Logs'
| summarize flowcount=count() by srcaddr, dstaddr
| sort by flowcount desc
```

![Aggregating search](search-s3-agg-example.png)
{scale="100%"}

### Aggregate over time {#time}

To aggregate over time, we'll use the [`timestats`](timestats) operator.

```kusto {runSearch=true}
dataset="cribl_search_sample" source=*vpc*
| limit 1000
| extract parser='AWS VPC Flow Logs'
| timestats span=1h bytessum=sum(bytes)
| extend mb=bytessum/1024/1024
| project-away bytessum
```

![Aggregating over time search](search-s3-agg-time-example.png)
{scale="100%"}

Cribl Search comes with rich [charting options](charting) out of the box, allowing you to adjust Charts as needed. The
above example is an Area Chart with a Y-axis minimum of 6,000.
