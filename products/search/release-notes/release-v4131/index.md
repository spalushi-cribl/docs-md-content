# Cribl Search 4.13.1


Cribl Search 4.13.1 includes significant performance improvements, new features, and important bug fixes.

## Important Changes

These changes to Cribl Search might require you to check or modify existing queries, or other configuration, especially
on saved searches.

### AWS SDK Update from Sunsetting v2 to v3

AWS will end support for their
[AWS SDK for JavaScript v2](https://aws.amazon.com/blogs/developer/announcing-end-of-support-for-aws-sdk-for-javascript-v2/)
on September 8, 2025. Cribl Search uses this SDK when querying S3, Amazon Security Lake, and AWS API Datasets. To ensure
uninterrupted operation and compatibility, we have updated our SDK to v3 in this release, and we will completely remove
the v2 SDK in our September 2025 release. Based on AWS’ public communications and our testing, Cribl expects this update
to have no impact on Cribl Search users.

### Removed `.drop all metadata` and `.generate metadata` Commands

The deprecated `.drop all metadata` and `.generate metadata` commands are no longer available. To speed up searches on designated Datasets, consider assigning them to [Lakehouses](/lake/lakehouse/) in Cribl Lake.

### Removed `$vt_object_list` and `$vt_object_list_summary` Virtual Tables

The deprecated `$vt_object_list` and `$vt_object_list_summary` virtual tables are no longer available, as part of the same shift to [Lakehouses](/lake/lakehouse/) for faster searches.

### Removed `set` Statement's `scan_mode` Option

The [`set`](set) statement no longer supports its deprecated `scan_mode` option, as part of the same shift to [Lakehouses](/lake/lakehouse/) for faster searches.

## New Features

This release adds the following new capabilities.

###   Pack Lookups, Macros, and Saved Searches Can Be Moved

You can now move saved searches, macros, and lookups between Cribl Search [Packs](packs) and the product's global
context.

### Cloned Pack Resources Are Now Saved to Your Current UI Location

If you clone a Dashboard from within its Pack, the clone is saved as part of the Pack. If you clone the same Dashboard
from the global **Dashboards** page, the clone is saved as a global resource.

## Corrections

This release includes numerous fixes to various areas of Cribl Search, most notably:

| ID                                           | Description                                                                                                     |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| <div style="width: 100px">SEARCH-10486</div> | The results table no longer crashes when the returned events contain no `_time` fields.                         |
| SEARCH-10572                                 | Lakehouses now correctly handle queries that pass regex special characters to the [`has`](has) operator.        |
| SEARCH-8055                                  | Trailing dashes no longer cause trouble when you’re highlighting an event’s text to include it in the query.    |
| SEARCH-7548                                  | When adding new members to a [Usage Group](usage-groups), you can now search users by email with no issue.      |
| SEARCH-10221                                 | The saved searches page can now filter large numbers of entries faster.                                         |
| SEARCH-10339                                 | We fixed an issue where certain searches into very large Datasets could result in out-of-memory (OOM) failures. |
| SEARCH-10038                                 | We fixed an issue where certain searches could crash when querying multiple Datasets at once.                   |
| SEARCH-10414                                 | We fixed an issue where certain searches could intermittently fail with an EPIPE error.                         |
