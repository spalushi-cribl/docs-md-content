# Cribl Search 4.8


Starting with Cribl Search 4.8, you can connect to [ClickHouse](https://clickhouse.com/docs/en/intro) databases, automatically scale the number of search executors, normalize event structures via a new `foldkeys` operator, use literal regex in your queries, and more.

## ClickHouse Integration

You can now search ClickHouse databases, by adding ClickHouse [Dataset Providers](set-up-clickhouse#provider) and [Datasets](set-up-clickhouse#dataset).

## Automatic Scaling of Executors

You can now automatically scale the number of executors allocated to your search, by using the [`set`](set) statement option: `max_executors="auto"`.

## Support for Regex Literals

You can now use ECMA-style regular expressions in your queries, including in key compare expressions, `in`-lists, and the `cribl` operator. 

## New Operator: `foldkeys`

You can now easily handle field names that contain separator characters, normalizing them with the new [`foldkeys`](foldkeys) operator.

## New Function: `gettype`

You can use the new [`gettype`](gettype) scalar function to determine the data type of a field. 

## Vertical and Horizontal Bar Charts

You can use a new Horizontal Bar visualization type. The original (vertical) Bar type is now renamed Column, and it remains the default visualization. 

## More Control over Chart Settings

On visualizations, a new **Save chart settings** toggle enables you to choose whether to persist any formatting customizations to any subsequent queries performed on the page. In this toggle's default off position, the visualization will reset to its own defaults upon refresh. 

## Easier Viewing and Searching of Search Results

With new dedicated buttons, you’ll find it easier to [view the results](search-results#view) of your past searches and [query their result sets](search/common-examples#search-the-results).