# top-hitters



The `top-hitters` operator counts distinct value combinations and returns the most frequent combinations in descending order. It provides valuable insights into the most popular and impactful data combinations within your input Dataset.

This operator is conceptually equivalent to using the [`summarize`](summarize) operator with `count` and `top`:

    `Scope | summarize C=count() by ValueExpression | top NumberOfValues by C desc`

## Syntax

    `Scope | top-hitters NumberOfValues of ValueExpression [ by SummingExpression ]`

### Arguments

* **Scope**: The events to search.
* **NumberOfValues**: The number of distinct values of **ValueExpression**.
* **ValueExpression**: An expression or list of fields that operate over the **Scope** whose distinct values are returned. Supports single expressions like `x / 42` or `abs(status)` as well as more complex expressions such as `top-hitters 10 of method, status [by SummingExpression]`.
* **SummingExpression**: If specified, a numeric expression over the **Scope** whose sum per distinct value of **ValueExpression** establishes which values to emit. If not specified, the count of each distinct value of **ValueExpression** is used instead.

## Examples

* Return the top 3 distinct `status` values.

    ```kusto {runSearch=true}
    dataset="cribl_search_sample" status="*"
    | limit 1000
    | top-hitters 3 of status
    ```

* Sum `bytes` and returns the top 2 `host` values.

    ```kusto {runSearch=true}
    dataset="cribl_search_sample" status="*" host="*"
    | limit 1000
    | top-hitters 3 of host by bytes
    ```

* Sum `bytes`, and return the top 10 distinct combinations of `clientip` and `useragent` values based on the `bytes` field.

    ```kusto {runSearch=true}
    dataset="cribl_search_sample" clientip="*" useragent="*"
    | limit 1000
    | top-hitters 5 of clientip, useragent by bytes
    ```



