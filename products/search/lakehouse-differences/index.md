# Lakehouse Search Differences


Identify Cribl Search behavior that differs when searching Lakehouses versus standard Cribl Lake Datasets.

---

When you run Cribl Search queries against a [Lakehouse](/lake/lakehouse/)-assigned Dataset, some behavior and results differ from a corresponding search executed against a [Cribl Lake Dataset](/lake/datasets/) without Lakehouse caching. This page identifies operators, functions, data types, and other details to keep in mind when searching Lakehouses.

You can disable using Lakehouse for a specific query by using the `set` statement
with the [`lakehouse`](set#lakehouse) option set to `off`.

## Unlimited Events with `sort` {#sort}

The [`sort`](/search/sort) operator’s `MaxNoOfOutputEvents` parameter defaults to unlimited with Lakehouse, rather than to `10000` events as in a regular search. However, you can set this limit parameter as desired.  

## Strict Boolean Comparisons {#strict}

Boolean comparisons in Lakehouse searches are strongly typed. Assume a query condition like `field1==field2`, in which one field is a boolean,   while the other is a boolean-compatible value (such as `"yes"`, `"y"`, `"t"`, `"true"`, or `"1"` for `true`; or `"no"`, `"f",` `"false"`, or `"0"` for `false`).

In a mixed comparison – for example, where `field1`'s value is `true` and `field2`'s value is `"yes"` – note that `field1==field2` will not match. (When searching the same data without Lakehouse caching, looser typing allows these values to match.)

## Statistical Aggregations with Booleans {#aggregation-null}

A [statistical aggregation](/search/statistical-functions) that aggregates mathematically on a boolean value will return a `null` in Lakehouse searches. (Running the same aggregation without Lakehouse caching will return a numeric value – either `1` for `true` or `0` for `false`.)

This `null` return value applies to the following functions when their `Expression` argument takes a boolean value: [`avg`](/search/avg), [`percentile`](/search/percentile), [`stdev`](/search/stdev), [`stdevp`](/search/stdevp), [`sum`](/search/sum), [`variance`](/search/variance), and [`variancep`](/search/variancep). (A boolean value in these functions' `Predicate` argument does not cause a `null` return value.) 

The `null` return value also occurs with a boolean value in the following functions' sole `Expression` argument: [`avgif`](/search/avgif),  [`stdevif`](/search/stdevif), [`sumif`](/search/sumif), and [`varianceif`](/search/varianceif).

## `dcount` Excludes `null` Values {#dcount-null}

When applying the [`dcount`](/search/dcount) aggregation function,  Lakehouse searches do not count `null` values toward the total, whereas searches without Lakehouse caching do.

This means that any `null` values in your data will cause the `dcount` value to be off by 1, compared to running the same query against the same data without Lakehouse caching (or in a different data store).

## Extra `null` Column with `timestats` {#timestats-null}

Applying the [`timestats`](/search/timestats) aggregation function to a search against a Lakehouse-assigned Dataset might return a `null` column when only empty values are found. Searches without Lakehouse caching do not return this column.

## Higher  `dedup` and `eventstats` Result Counts {#higher-count}

Applying the [`dedup`](/search/dedup) or [`eventstats`](/search/eventstats) operator to a search against a Lakehouse-assigned Dataset  can return a higher result count, compared to searching the same data without Lakehouse caching. This is because `dedup` and `eventstats` in Lakehouse mode can process unlimited events, compared to the 50,000-event limit applied to `dedup` and `eventstats` in searches without caching.


## Different `extract type=regex overwrite` Behavior {#overwrite}

When using the [`extract`](extract-operator) operator with `type=regex` and the `overwrite` option for existing event fields, the behavior is different with and without Lakehouse caching. Without Lakehouse, if `overwrite` is set to `false`, the extraction will convert any existing field values to an array. However, Lakehouse searches will omit any existing field values, regardless of whether `overwrite` is set to `true` or `false`. 

For example, if you extract `key src_ip` with value `10.2.3.3`, and the field already exists with value `10.1.2.2`, a non-Lakehouse search with `ow=false` will yield a field value of `["10.1.2.2", "10.2.3.3"]`. But a Lakehouse search will yield a field value of `10.2.3.3`.

To consistently preserve the field's original value, you can assign that value to a different variable earlier in the query.
