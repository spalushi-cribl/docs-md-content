# Statistical Functions


# ![](page/agg-icon.svg) Statistical Functions

Statistical functions are used together with the [`summarize`](summarize), [`eventstats`](eventstats), and
[`timestats`](timestats) operators to aggregate your data. Cribl Search supports the following statistical functions:


| Name                       | Description                                                                                                   |
| -------------------------- | ------------------------------------------------------------------------------------------------------------- |
| [`avg`](avg)               | Calculates the average across the group.                                                                      |
| [`avgif`](avgif)           | Calculates the average across the group where a predicate evaluates to `true`.                                |
| [`count`](count)           | Counts events per summarization group.                                                                        |
| [`countif`](countif)       | Counts events based on a predicate.                                             |
| [`dcount`](dcount)         | Calculates an estimate of the number of distinct values.                                                      |
| [`dcountif`](dcountif)     | Calculates an estimate of the number of distinct values for those rows where a predicate evaluates to `true`. |
| [`max`](max)               | Finds the maximum value across the group.                                                                     |
| [`maxif`](maxif)           | Finds the maximum value for which a predicate evaluates to `true`.                                            |
| [`min`](min)               | Finds the minimum value across the group.                                                                     |
| [`minif`](minif)           | Finds the minimum value which a predicate evaluates to `true`.                                                |
| [`percentile`](percentile) | Returns an estimate for the specified nearest-rank percentile of the population defined.                      |
| [`stdev`](stdev)           | Calculates the standard deviation of an expression across the group.                                          |
| [`stdevif`](stdevif)       | Calculates the standard deviation of an expression which a predicate evaluates to `true`.                     |
| [`stdevp`](stdevp)         | Calculates the standard deviation of an expression across the group, considering the group as a population.   |
| [`sum`](sum)               | Calculates the sum of an expression across the group.                                                         |
| [`sumif`](sumif)           | Calculates the sum of an expression for which a predicate evaluates to `true`.                                |
| [`variance`](variance)     | Calculates the variance of an expression.                                                                     |
| [`varianceif`](varianceif) | Calculates the variance of an expression for which a predicate evaluates to `true`.                           |
| [`variancep`](variancep)   | Calculates the variance of an expression across the group, considering the group as a population.             |

