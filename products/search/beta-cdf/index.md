# beta_cdf



The `beta_cdf` function returns the standard [cumulative beta distribution function](https://en.wikipedia.org/wiki/Beta_distribution#Cumulative_distribution_function).

The beta distribution is commonly used to study variation in the percentage of something across samples, such as the fraction of the day people spend watching television.

## Syntax

    `beta_cdf( X, Alpha, Beta )`

### Arguments

* **X**: A value at which to evaluate the function.
* **Alpha**: A parameter of the distribution.
* **Beta**: A parameter of the distribution.

## Returns

The cumulative beta distribution function.

Note:
* If any argument is nonnumeric, beta_cdf() returns null value.
* If x < 0 or x > 1, beta_cdf() returns NaN value.
* If alpha ≤ 0 or alpha > 10000, beta_cdf() returns the NaN value.
* If beta ≤ 0 or beta > 10000, beta_cdf() returns the NaN value.

## Examples {#examples}

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="access_combined"
| limit 100
| summarize combinations = count() by host, clientip
| extend betaDistrib = beta_cdf( combinations, 10, 20 )
```
