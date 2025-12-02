# beta_inv


The `beta_inv` function returns the inverse of the beta cumulative probability beta density function.

The beta distribution can be used in project planning to model probable completion times given an expected completion time and variability.

## Syntax

    `beta_inv( Probability, Alpha, Beta )`

### Arguments

* **Probability**: A probability associated with the beta distribution.
* **Alpha**: A parameter of the distribution.
* **Beta**: A parameter of the distribution.

## Returns

The inverse of the beta cumulative probability density function [beta_cdf](beta-cdf).

Note:
* If any argument is nonnumeric, **beta_inv** returns null value.
* If alpha ≤ 0 or beta ≤ 0, **beta_inv** returns the null value.
* If probability ≤ 0 or probability > 1, **beta_inv** returns the NaN value.
* Given a value for probability, **beta_inv** seeks that value x such that beta_cdf(x, alpha, beta) = probability.

## Examples {#examples}

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="access_combined"
| limit 100
| summarize combinations = count() by host, clientip
| extend invBetaDensity = beta_inv( combinations, 10, 20 )
```
