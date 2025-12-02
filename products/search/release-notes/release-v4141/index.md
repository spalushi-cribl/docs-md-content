# Cribl Search 4.14.1


Cribl Search 4.14.1 includes multiple enhancements to Notebooks (a Preview feature), a Workspace view of credits consumption, a new IAM Admin Permission, and bug fixes.

## New Features

This release adds the following new capabilities.

### New Notebook Cells from Returned Fields

Search [Notebooks (Preview)](notebooks) now support creating new cells by selecting returned fields in the field summary and event details drawers.

### Notebook Cell Display Options

Within [Notebooks](notebooks), you can now move cells using drag handles, rather than the earlier up/down arrow buttons. Above the drag handle, a new Collapse/Expand toggle enables you to control each cell’s vertical display depth. 

### Notebook Edit Locks

You can now lock and re-enable [Notebook](notebooks#access) editing, for use cases where you need to preserve an investigation’s results.

### Notebook Multi-User Save Notification

Notebook Editors will now be alerted to conflicting edits and prompted to reload.

### Notebook Summaries Enhanced

Notebook [summaries](notebooks#summarize), where enabled, now provide a Regenerate button, improved error handling and messaging, and several UX improvements.

### Notebook Markdown Typeahead Removed

Within Notebook [markdown](notebooks#notes) notes, we’ve disabled the distracting typeahead behavior.

### Customize Notebook Display Width

The Notebook Actions (![](actions-dropdown.light.svg)) menu now provides a **Wide Layout**/**Default Layout** toggle.

### IAM Admin Permission in Cribl.Cloud

The IAM Admin is a new Organization-level Permission that can manage the Organization's SSO configuration and can invite, update, and remove Members, without having access to data engineering functions. This reduces the risk of unauthorized data access or modification.

### Expanded Workspace Landing Page

Each [Workspace](workspaces) landing page is now a dashboard, providing:

- Buttons to quickly access all Cribl products.
- A **Search Queries** bar graph showing hourly credits consumption.
- Buttons for quick actions, such as creating a Notebook or managing and sharing Datasets. 
- Health status and throughput of Stream Worker Groups and Edge Fleets.
- Recent actions in your Workspace, and Cribl resources.

![Workspace landing page](workspace-landing-pg.png)
{border="true" scale="80%"}

### Deprecated Documentation Gone

We’ve removed the page that described the (already deprecated) Dataset Acceleration feature. One less page to read!

## Corrections

This release includes the following bug fixes:

ID | Description
-- | -----------
SEARCH-11030 | In [Lakehouse](/lake/lakehouse/) searches, using an aggregate function's result in a `by` clause now longer throws a `MULTIPLE_EXPRESSIONS_FOR_ALIAS` database error 
SEARCH-10569 | Corrected Search query box’s lagging response in Organizations with large numbers of Datasets.
SEARCH-10958 | Corrected visualization inaccuracies when aggregation operators precede numbers represented as strings.
SEARCH-11075 | Corrected parsing functions’ failure to filter on nested fields when piped directly to a [`where`](where) operator.