# dcount


# ![](page/agg-icon.svg) dcount

The `dcount` aggregation function calculates an estimate of the number of distinct values that are taken by an expression in the summary group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

> The `dcount()` aggregation function is primarily useful for estimating the cardinality of huge sets. It trades performance for accuracy, and might return a result that varies between executions. The order of inputs might have an effect on its output.
>
{.box .info}

## Syntax

    `dcount( Expression[, Accuracy] )`

### Arguments

* **Expression**: A scalar expression whose distinct values are to be counted. Wildcards are not supported for field names.
* **Accuracy**: Optional. An integer that defines the requested [estimation accuracy](#accuracy). If unspecified, the default value is `1`.

## Results

Returns an estimate of the number of distinct values of **Expression** in the group.

## Example

This example summarizes the estimated cardinality of destination ports by destination address:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| summarize distinctCountPorts=dcount(dstport) by dstaddr
```



## Estimation Accuracy {#accuracy}

This function uses a variant of the [HyperLogLog (HLL) algorithm](https://en.wikipedia.org/wiki/HyperLogLog), which does a stochastic estimation of set cardinality. The algorithm provides a knob that can be used to balance accuracy and execution time per memory size:

Accuracy|Error (%)|Entry count
-----|-----|-----
0|1.6|212
1|0.8|214
2|0.4|216
3|0.28|217
4|0.2|218

> The **Entry count** column is the number of 1-byte counters in the HLL implementation.
>
{.box .info}

The algorithm includes some provisions for doing a perfect count (zero error), if the set cardinality is small enough:
* When the accuracy level is `1` – 1,000 values are returned
* When the accuracy level is `2` – 8,000 values are returned

The error bound is probabilistic, not a theoretical bound. The value is the standard deviation of error distribution (the sigma), and 99.7% of the estimations will have a relative error of under 3 x sigma.

The following image shows the probability distribution function of the relative estimation error, in percentages, for all supported accuracy settings:

![Search page](hll-error-distribution.52e953255d.png)

