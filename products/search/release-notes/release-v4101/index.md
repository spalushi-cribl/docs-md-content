# Cribl Search 4.10.1


With version 4.10.1, Cribl Search offers more consistent evaluation of empty objects, expanded Datatypes options, improved Dashboards management, a gzip download option for lookup files, and several other functional and UX fixes.

## Important Changes

These changes to Cribl Search might require you to check or modify existing queries, or other configuration, especially on saved searches.

### Deprecation Notice: Dataset Acceleration

Cribl is deprecating [Dataset Acceleration](/search/acceleration) (a Preview feature) in preparation for a different solution. This feature will be removed in a future release. Please continue to report issues through normal Cribl [support channels](/support/support-contact), but assistance for this deprecated feature might be limited.

## New Features

This release includes the following new features:

### `isempty` and `isnotempty` Empty Object Evaluation

The [`isempty`](isempty) and [`isnotempty`](isnotempty) functions now evaluate an empty object (`{}`) as empty. This makes their behavior more consistent with integrated services. 

## Corrections

This release includes several fixes to various areas of Cribl Search, most notably:

| Reference | Description |
| --------- | ----------- |
| <div style="width: 100px"> CRIBL-30329 | After a session timeout, logging back into Cribl.Cloud now restores focus to the last-viewed product page. |
| SEARCH-9014 | Restored Datatypes link within Datasets. |
| SEARCH-8605 | The [`datetime`](datetime) Datatype now supports decimal values, as well as integers. |
| SEARCH-8542 | Determining the scalar value of a [`let`](let) query now works as expected. |
| SEARCH-8616 | Queries that include `project *, field, some=expression` now add to results, rather than replacing them. This new behavior is equivalent to `extend some=expression`. |
| SEARCH-8732 | Queries that include the [`find`](find) operator no longer return empty events. |
| SEARCH-8751 | Cribl Search now supports downloading lookup files in gzip (`.gz`) format. |
| SEARCH-7941 | Email Notifications now properly format embedded `_time` values, using UTC. |
| SEARCH-7991 | The **Objects discovered**, **scanned**, and **skipped** metrics now update as expected (below the query box) when a search completes. |
| SEARCH-9055 | Restored the ability to run ad hoc searches within the context of a Pack. |
| SEARCH-9074 | Adding a new Dashboard to a Pack no longer locks out the **Add** or **Exit** options. |
| SEARCH-8996 | Moving a Dashboard between Collections now preserves its ownership (**Created by**) metadata. |
| SEARCH-8381 | In Visualization details pop-overs, the **Created by** field now properly renders the owner's display name, rather than an ID hash. 
| SEARCH-4307 | When configuring a new visualization, the **Add to Dashboard** modal's default button is now **Add & Go to Dashboard**. |
| SEARCH-3326 | The **Saved Searches** page no longer displays a misleading **Add Search** button for users whose Permission does not allow them to create searches. |
| SEARCH-3328 | The **Search Home** page's **Actions** menu no longer displays a misleading **Save Search** option to users whose Permission does not allow them to execute it. |
| SEARCH-2695 | Configuring a Google Workspace API Dataset Provider now exposes a modal with improved controls. |
