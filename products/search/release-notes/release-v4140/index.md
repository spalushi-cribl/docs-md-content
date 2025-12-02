# Cribl Search 4.14.0


Cribl Search 4.14.0 includes significant performance improvements, new capabilities, and important bug fixes.

## Important Changes

These changes to Cribl Search might require you to check or modify existing queries, or other configuration, especially
on saved searches.

### Lakehouse Datasets Default to Lakehouse Query Execution

When you use Cribl Search to query a Cribl Lake Dataset that’s assigned to a [Lakehouse](/lake/lakehouse/), queries now
default to Lakehouse execution. This returns results faster and more efficiently, but limits the results to the past 30
days, even if the underlying Lake Dataset has a longer retention period. To override the new default and search back
beyond 30 days, precede your query with `set lakehouse="off";` (see the [docs](set#lakehouse) for details).

### Stricter Data Sending Permissions

Search Editors can no longer send search results to custom URLs. However, they can still send results to custom-named
Stream Worker Groups. For details, see the [Permissions](send#permissions) section of the `send` operator doc (available
starting Sep 17).

## New Features

This release adds the following new capabilities.

### Notebooks (Preview) for Data Investigation Storytelling

**Notebooks** enable data analysts to collaborate on complex, multi-stage investigations right within Cribl Search. In
14.4.0, we’re releasing Notebooks as a Preview feature. Here’s what you can do with it:

- **Iterate**: Run multiple searches next to one another, for faster, more efficient investigations.
- **Annotate**: Add complex notes using Markdown.
- **Share**: Grant editing or read-only access to your Notebook.

See the [Notebooks documentation](notebooks) for details.

### Dashboard Interactions Support More Tokens

When you add interactions to Dashboard visualizations, you can now use new token types to help users drill down into underlying values:

- `$environment.baseUrl$` (static)
- `$environment.type$` (static)
- `$environment.workspace$` (static)
- `$field.name$` (dynamic, the name of the field clicked)
- `$field.value$` (dynamic, the value of the field clicked)
- `$rowData.<fieldname>$` (dynamic, extracts individual fields from returned events)

For details, see [Add Interactions to Your Cribl Search Dashboard](dashboard-interactions).

### Cribl Search User API Credentials

Organization Owners and Organization Admins can now create [API Credentials](/cribl-as-code/authentication/#cloud-auth) that assign Cribl Search [User Permissions](cloud-managing#search-member-roles) for individual Workspaces.

### Cribl Terraform Provider (Preview)

The [Cribl Terraform provider](https://registry.terraform.io/providers/criblio/criblio/latest/docs) is now available in Preview from the Terraform Registry. Use the criblio provider to manage Cribl as part of your infrastructure-as-code workflows and make large deployments repeatable and reliable.

### New Cribl.Cloud Regions

We are excited to now support creating Cribl.Cloud Organizations in Switzerland and Singapore AWS Regions (`eu-central-2`, `ap-southeast-1`).

## Corrections

This release includes numerous fixes to various areas of Cribl Search, most notably:

| Issue                                       | Description                                                                                                                                                                                                              |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <div style="width: 100px">CRIBL-30786</div> | [`percentile`](percentile), [`median`](median), and [`medianif`](medianif) functions now return consistent results, whether you’re searching a Lakehouse or not.                                                         |
| SEARCH-3389                                 | Searches executed via the Cribl Search API now use the same default time range values as UI searches. `earliest` defaults to `-1h`, and `latest` to `now`.                                                               |
| SEARCH-5899                                 | When exporting results to Cribl Lake (`export to lake`), the search logs now correctly show the number of bytes written. Before, the `bytesOut` and `totalBytesOut` fields would incorrectly show `0` in some scenarios. |
| SEARCH-9084                                 | Exporting results to Cribl Lake now correctly returns final search logs upon completion.                                                                                                                                 |
| SEARCH-9844 | Searching large (>64 MB) Splunk journal files is now more reliable and scalable. |
| SEARCH-10396                                | Searches that execute immediately will no longer flicker as **Queued** in the UI.                                                                                                                                        |
| SEARCH-10461 | The number of executors no longer exceeds the [`max_executors`](set#max_executors) value. |
| SEARCH-10553                                | On the Cribl Search landing page, the **Available Datasets** and **History** sections now display a clearer wording for the last run time (**XX minutes ago** rather than **in XX minutes**).                            |
| SEARCH-10569                                | We fixed an issue where having a large number of Datasets made the query box work slower.                                                                                                                                |