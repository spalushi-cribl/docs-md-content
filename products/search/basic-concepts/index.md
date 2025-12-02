# Basic Concepts


Learn fundamental Cribl Search concepts, terminology, and usage relationships, with links to further information.

---

## Datasets {#datasets}

Datasets are a way to organize and reference a set of data. Think of a Dataset as a definition that describes **what**
we need to query, or as a "container" of some data.

A Dataset might contain information about the path(s) that need to be queried for specific data at a specific source,
such as a filesystem, an S3 bucket, or an Edge Worker. For example, a Dataset named `myVPCFlowlogs` contains Amazon VPC
Flow logs, and is referenced as `dataset=myVPCFlowlogs` in a [query](build-a-search).

Data is broken into discrete events and searchable fields by assigning [Datatypes](datatypes) to a Dataset.

## Dataset Providers {#dataset-providers}



Dataset Providers enable Cribl Search to categorize and connect Datasets using specific interfaces to external systems.
Think of a Dataset Provider as a definition that describes **where** we need to send the query. A Dataset Provider can
contain connection information required to obtain the data, such as API keys or secrets.

## Searches

A search is a query request that Cribl Search uses to process data and return results. The request is a plain-text
string composed of multiple operators separated by pipes: `|`. For example, the search
`dataset=myVPCFlowlogs | limit 1000` will retrieve data from the Dataset `myVPCFlowlogs` and will return up to 1,000
results. For further details, see [Cribl Search Tour](search-overview).

## Operators and Functions

As data passes through a search, it is handled by [operators](operators) that process the data based on
[functions](functions). For example, in this search `dataset=myVPCFlowlogs | summarize count() by srcport` the
`summarize` operator will aggregate all events in the Dataset `myVPCFlowlogs`. The [`count()`](count) function will
count the number of events, and the `by` clause will group them by `srcport` values.

## Commands and Statements

[Commands](commands) are simple instructions that you can type right in the query box. They're designed primarily for
[Admins and Editors](cloud-managing#search-member-roless), who use them to [manage search jobs](commands#view-searches)
or learn more about the Organization's [Datasets](commands#list). Commands always start with a period (`.`) as you can
see in this example: [`.show running queries`](show-queries).

Statements help you build more readable and efficient searches. Use [`let`](let) statements to give names to expressions
or entire queries, and then reference those names in the context of the same search, which in turn allows you to
[`join`](join) Datasets. The [`set`](set) statement, on the other hand, lets you configure
[advanced search options](set#available-set-statement-options).

## Search Types {#aggregate}

The search type is based on the type of results returned, either raw or aggregate. If the query runs the
[`summarize`](summarize), [`eventstats`](eventstats), or [`timestats`](timestats) operators along with an aggregation
function, it returns tabular results that are [chartable](charting); otherwise, it returns raw results in a table.

[Cribl](cribl-functions) and [statistical](statistical-functions) are aggregation functions that are used in conjunction
with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators to aggregate your
data.

You can recognize aggregation functions and operators by this icon next to their titles: ![](page/agg-icon.svg).

## Cribl Lake and Lakehouses {#lake}

Cribl Lake is Cribl’s storage solution for your data, requiring little to no configuration on your part.
[Lake Datasets](/lake/datasets) work as Cribl Search Datasets out-of-the-box, so you can instantly
[start searching](search-cribl-lake) them. You can also assign Lake Datasets to [Lakehouses](/lake/lakehouse), for
dramatically faster search responses and more-predictable billing. For more information, see the
[Cribl Lake docs](/lake/).
