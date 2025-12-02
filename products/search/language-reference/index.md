# Language Reference


A comprehensive reference for the Cribl Search implementation of KQL.

---

Cribl Search is based on [Kusto Query Language](https://learn.microsoft.com/en-us/kusto/query/?view=microsoft-fabric)
(KQL), which lets you delve into your data to discover patterns, identify anomalies and outliers, and create statistical
models.

While the Cribl implementation of KQL mostly follows the original, there are [certain KQL differences](kql-cribl) (for example, we provide the implicit [`cribl`](cribl) operator). To get the details right, follow the language usage guidelines presented in these
sections:

- [Operators](#operators)
- [Functions](#functions)
- [Statements](#statements)
- [Commands](#commands)
- [Virtual Tables](#virtual-tables)

> You can also browse and link to all KQL operators and functions in our [Index of Operators and Functions](language-index).
{.box .success}

## Operators {#operators}

An operator in Cribl Search is a query component that processes data, performing actions such as filtering, counting, or
transforming events. Operators can use [functions](#functions), and are delimited by the pipe character `|`.

For example, the [`limit`](limit) operator sets the maximum number of events returned from a search:

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | limit 100
```

To learn more, see: [Operators](operators).

## Functions {#functions}

A function in Cribl Search is a unit of logic that processes data based on arguments passed to it.

For example, the [`max`](max) function returns the maximum value of a field:

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | summarize LatestEvent=max(start)
```

To learn more, see: [Functions](functions).

## Statements {#statements}

A statement in Cribl Search is a special keyword that sets options ([`set`](set)) or assigns names to expressions
([`let`](let)).

For example, this statement sets the maximum number of events returned by the current search:

```kusto {runSearch=true}
set max_results_per_search=1000;
```

To learn more, see: [Statements](statements).

## Commands {#commands}

A command in Cribl Search is an instruction used to manage searches directly from the query box. Unlike functions and
operators, commands start with a period.

For example, the [`.cancel`](cancel) command can stop searches:

```kusto {runSearch=true}
.cancel running queries
```

To learn more, see: [Commands](commands).

## Virtual Tables {#virtual-tables}

A virtual table in Cribl Search is a dynamically generated dataset that provides system information useful for
troubleshooting, performance analysis, and testing.

For example, the [`$vt_dummy`](vt_dummy) virtual table generates sample data:

```kusto
dataset="$vt_dummy" event<10
```

To learn more, see: [Virtual Tables](virtual-tables).
