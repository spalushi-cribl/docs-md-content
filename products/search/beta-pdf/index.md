# beta_pdf



The `beta_pdf` function returns the probability density beta function.

The beta distribution is commonly used to study variation in the percentage of something across samples, such as the fraction of the day people spend watching television.

## Syntax

    `beta_pdf( X, Alpha, Beta )`

### Arguments

* **X**: A value at which to evaluate the function.
* **Alpha**: A parameter of the distribution.
* **Beta**: A parameter of the distribution.

## Returns

The [probability beta density function](https://en.wikipedia.org/wiki/Beta_distribution#Probability_density_function).

Note:
* If any argument is nonnumeric, beta_pdf() returns null value.
* If x ≤ 0 or 1 ≤ x, beta_pdf() returns NaN value.
* If alpha ≤ 0 or beta ≤ 0, beta_pdf() returns the NaN value.

## Examples {#examples}

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="access_combined"
| limit 100
| summarize combinations = count() by host, clientip
| extend probBetaDensity = beta_pdf( combinations, 10, 20 )
```
