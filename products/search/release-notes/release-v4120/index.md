# Cribl Search 4.12.0


With version 4.12.0, you can share Search Pack resources more widely, authenticate with HTTP API endpoints in new ways, use new kinds of inputs in Dashboard visualizations, and more.

## Important Changes

These changes to Cribl Search might require you to check or modify existing queries, or other configuration, especially
on saved searches.

### Deprecation Notice: Dataset Acceleration

Cribl is deprecating [Dataset Acceleration](acceleration) (a Preview feature) in preparation for a different solution.
This feature will be removed in a future release. Please continue to report issues through normal Cribl [support channels](/support/), but assistance for this deprecated feature might be limited.

### Case-Insensitive Comparisons with `cribl` Operator

Comparison expressions with the [`cribl`](cribl#comparisonoperator) operator are now fully case-insensitive, as intended. Previously,  the `=`, `==`, and `!=` comparison operators were case-sensitive. For existing queries built around those operators, this change might return larger result sets than before. To enforce case-sensitive searches, use the [`where`](where#comparison-operators) operator and case-specific [string comparison operators](string-operators).

## New Features

This release includes the following enhancements.

### Packs Are Out of the Preview Stage

Cribl Search [Packs](packs) are now generally available and no longer in Preview. They also get several new features. For example:

* Search Admins and Editors can quickly make Pack contents visible and usable from anywhere in the Workspace. See: [Allow Access to a Pack](packs#access).

* They can also now reference global saved searches, Macros, and Lookups from within a Pack context.

### Generic HTTP API Dataset Provider: Expanded Authentication Options

[Generic HTTP API](set-up-generic-http-api#authentication) Dataset Providers support three new authentication options:  Basic Auth, OAuth2, and sending credentials in a POST request to an endpoint that responds with an access token.



### Query `$vt_results` for Failed or Canceled Jobs

A new [`set` option](set#allow_incomplete_results) enables querying results from the [`$vt_results`](vt_results) virtual table even where the original search jobs failed or were canceled. A corresponding new `jobStatus` property indicates why the search did not complete.

### Dashboard Interactions Support Links

Dashboard Interactions now provide a **Link** option, which supports passing a token to an external URL.

### Dashboard Markdown Panels Support Tokens

Dashboard markdown panels now support token substitution. You can use this feature to (for example) display different images based on values along a range.

### Configurable Coordinator Memory Allocation

In each [Usage Group](usage-groups#usage-limits), you can now configure the search coordinator process' heap memory limit. Set a slightly higher value to resolve out-of-memory errors due to high Dataset cardinality or other causes.

### Improved Lakehouse Search Metrics

With [Lakehouse](/lake/lakehouse/) searches, the Search Details modal now shows accurate metrics on events in and bytes in.

### Detailed Billing Data Visibility in Cribl.Cloud

We've made more improvements to your Cribl.Cloud billing and usage portal. Now,
you can view all of these statistics in labeled, intuitive tabs:

- Your remaining credits, consumed credits, and average monthly consumption.
- Cumulative consumed credits per month across all products.
- Monthly data usage across all products, including infrastructure.
- Per-product consumption and credit cost in an easy-to-understand table format.

A detailed view lets you drill down a level deeper and see total, monthly, and
average consumption and usage for each product. Finally, a separate tab just for
invoices provides a one-stop shop to view finalized invoices that you can
download and export, as well as draft invoices to see where you currently stand.

## UI/UX Improvements

This release includes the following improvements to Cribl Search UI/UX.

### Events Tab Retains Column Selection

The Search Home [Events](search-overview#events) tab now preserves your column selection when you add fields, remove fields, or switch between Event and Table views. Your selection remains intact while you stay on the page.

### Improved JSON Editing Option

Using the JSON editor (for example, when [editing a Dashboard](editing-dashboards#json)) is now easier and more intuitive. New tabs enable you to toggle between JSON and graphical mode.

### Clearer Syntax Highlighting

Within queries, syntax highlighting now follows ANTLR Kusto grammar, to better distinguish operators, functions, values, and other identifiers.

## Corrections {#corrections}

This release includes numerous fixes to various areas of Cribl Search, most notably:

|Ticket|Description|
|--- |--- |
| <div style="width: 100px"> SEARCH-9808 <br> [Known Issue](known-issues#SEARCH-9808)|In Lakehouse searches, the [`dcountif`](dcountif) aggregation function now returns accurate numbers of results.|
|SEARCH-7344|We removed the optional **Strict Kusto Mode**, which previously prevented the cribl operator from being implicit. The change doesn’t affect the behavior of any of your searches.|
|SEARCH-8746|The Chart view now always shows the correct number of total results.|
|SEARCH-9247|No Cribl Search coordinator time is billed for Lakehouse-satisfied searches.|
|SEARCH-9760|[Lakehouses](/lake/lakehouse) now support indexing data fields whose names conflict with Cribl-generated fields. Fields from your data will be prefixed with `data_` to distinguish them from internal fields.|
|SEARCH-8746|Chart view now correctly displays total events.|
|SEARCH-8640|The [cribl](cribl) operator is now consistently case-sensitive, with and without [Lakehouse](/lake/lakehouse/) caching.|
|SEARCH-9061|To reduce security risks, authorization headers no longer appear on every event.|
|SEARCH-9875|Very small Markdown visualization panels can now be edited, as intended.|
|SEARCH-9645|Within [Search Limits](limits#list), the **Field summary field limit** option is renamed to **Field summary breadth limit**. It reads better.|

See also corrections in [Cribl Lake Release Notes](https://docs.cribl.io/lake/release-notes/release-v4120#corrections).
