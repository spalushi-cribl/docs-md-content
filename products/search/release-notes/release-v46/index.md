# Cribl Search 4.6


Starting with Cribl Search 4.6, you can export search results to [Cribl Lake](/lake/), save fragments of your queries as
Macros, include search results in your email notifications, and more.

### Export to Cribl Lake

You can now use the [`export`](export) operator to send Cribl Search results to a [Cribl Lake](/lake/) Dataset.

### Macros

You can now create [Macros](macros), to quickly reuse query text across different searches.

Macros can be [shared](macros#sharing-Macros-with-other-members) with other Members of your Organization, to build a
library of useful functions and transformations.

### New Multistage Search Features

[`let`](let) statements got more powerful. Now, you can:

- Write [`let`](let) statements that reference one another.
- Append the results of a [`let`](let) statement to your main results, by using the new [`union`](union) operator.
- Use the results of a [`let`](let) statement when filtering your main results with the
  [`in`](in-cs)/[`!in`](not-in-cs)/[`in~`](in)/[`!in~`](not-in) operators.
- Use the results of a [`let`](let) statement when filtering your main results as a discrete value (for example,
  `where fieldName > let_search_value`).

### Window Functions

Cribl Search 4.6 introduces [window functions](window-functions), enabling powerful data analysis within your queries.
You can use the following functions:

- [`prev`](prev) and [`next`](next) to access previous and subsequent rows.
- [`row_number`](row_number), [`row_rank_dense`](row_rank_dense), and [`row_rank_min`](row_rank_min), for row ranking
  and numbering.
- [`row_cumsum`](row_cumsum) for aggregations like running totals.
- [`row_window_session`](row_window_session) for session analysis.

### Search Results in Email Notifications

[Email notifications](email-notifications) can now include HTML tables with a sample of the search results.

### Scopes of `set` Statements

[Options](set#available-set-statement-options) configured by [`set`](set) statements can now persist across multiple
searches. This means you can configure options for different [scopes](set#scope-of-set-statements), applying them either
to the current search only, or to all of your searches (`user:` scope), or to all users in the
[Usage Group](usage-groups) (`global:` scope, available to Admin [Search Members](cloud-managing#search-member-roles)).

You can also manage [`set`](set)-statement options by using the two new commands:

- [`.show options`](show-options), to see which options are currently set.
- [`.clear options`](clear-options), to disable options.

### Updated Sample Searches

All [Sample Searches](search-overview#sample-searches) now reference the `cribl_search_sample` Dataset, rather than the
`cribl_internal_logs` Dataset. Moving forward, most users won’t have access to the `cribl_internal_logs` Dataset by
administrator policy. The `cribl_search_sample` Dataset should always be available to all users, so these new sample
searches should always work for everyone.

### Cribl Copilot 

This release introduces Cribl Copilot, Cribl’s new AI assistant for Cribl.Cloud! Cribl Copilot helps you maximize efficiency without leaving Stream, Edge, Search, or Lake. To access Cribl Copilot, click the teal AI icon at the bottom right of any page. 



Note that the initial version of Cribl Copilot has the following limitations:

* To enable Cribl Copilot, a Cribl.Cloud Organization Owner or Admin must provide consent. This enables the assistant for all users in their Organization. Standard users can only access the assistant once their Organization Owner or Admin enables the feature.
* Organization Owners and Admins cannot withdraw consent from within the product. To disable Cribl Copilot, please contact Support. 
* Cribl Copilot leverages only two pieces of data when generating an answer: the documentation available on [docs.cribl.io](https://docs.cribl.io/), and whatever you type into the question box.
