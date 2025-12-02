# Cribl Search 4.13.0


Cribl Search 4.13.0 includes significant performance improvements, new capabilities, and important bug fixes.

## Important Changes

These changes to Cribl Search might require you to check or modify existing queries, or other configuration, especially
on saved searches.

### Scheduled Searches: Times Are Now Relative to Scheduled Time

On [scheduled searches](scheduled-searches), `now`, `earliest`, and `latest` timing is now relative to the time the
search was scheduled to be executed, rather than the actual execution time.

For example, if you schedule a search for 1:00, but an unplanned outage causes the search to run at 1:05, the value of
`now` will be 1:00, not 1:05. Note that this increases the effective time range accuracy of all existing scheduled
searches.

### Lakehouse Data Retention Times Are Now Based on Ingest Time

Within each [Lakehouse](/lake/lakehouse), the data's **Retention period** is now measured based on `ingestion_time` (the
clock time at which events were actually ingested into the Lakehouse), rather than based on `event_time` (each event's
`_time` field value). This enables some new use cases, and makes Lakehouse behavior consistent with Cribl Lake behavior.

### Lakehouse Attached to an Empty Dataset Includes More Events

When you attach a Lakehouse to an **empty** Lake Dataset, the Lakehouse will now mirror all of the Dataset's data. This
provides several advantages:

- All data sent to the Lake Dataset will be ingested into the Lakehouse, regardless of the `event_time` per event. So,
  you can now send older data to Lakehouses.
- However, regardless of `event_time`, data will age out of the Lakehouse 30 days after the data's **ingest** time.
- Cribl Search will now **always** use faster Lakehouse search when executing queries against such mirrored Datasets –
  unless you explicitly override this behavior by prepending the `set lakehouse="off"` command.

### End of Support Notice for AWS SDK v2

AWS will end support for their
[AWS SDK for JavaScript v2](https://aws.amazon.com/blogs/developer/announcing-end-of-support-for-aws-sdk-for-javascript-v2/)
on September 8, 2025. This SDK is used by Cribl Search when searching S3, Amazon Security Lake, and AWS API datasets. To ensure
uninterrupted operation and compatibility, we will update our SDK to v3 in the August 2025 Cribl release and completely
remove the v2 SDK in September 2025.



## New Features

This release adds the following new capabilities.

### New Larger Lakehouses Support Higher Ingest Rates

Cribl Lakehouse now supports two new Dataset [sizes](/lake/lakehouse#lakehouse-sizes):

| Size    | Ingest Rate     |
| ------- | --------------- |
| 3XLarge | Up to 14 TB/day |
| 6XLarge | Up to 28 TB/day |

The new tiers are available in the US and Germany, and are not visible in the Cribl Lake UI at this point. To start
using them, contact [Cribl Support](/support/).

### Prevent Data Gaps by Skipping Event-Time Filtering

On Amazon S3, Azure Blob Storage, and Google Cloud Storage Datasets, you can enter a new **Skip event time** **filter**
option. This will cause the time picker to ignore event timestamps, and filter only based on the partitioning scheme's
time boundaries, to retrieve data that might otherwise be hidden.

### Read Archived Amazon S3 Objects

Cribl Search can now read temporarily restored S3 objects. This enables you to reach objects in storage classes that
Cribl Search doesn’t support directly (such as S3 Glacier Deep Archive).
([Related known issue](https://docs.cribl.io/results/?query=9925&searchOption=known-issues&product=search)).

## Corrections

This release includes numerous fixes to various areas of Cribl Search, most notably:

| ID                                                                                                                                                             | Description                                                                                                                                                                                         |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <div style="width: 100px">SEARCH-10317</div>[Known issue](https://docs.cribl.io/results/?query=&searchOption=known-issues&product=search&tickets=SEARCH-10317) | Selecting a Dashboard panel with an [interaction](editing-dashboards#add-interactions) setting of **Run a new search** now properly carries over the original panel's time range to the new search. |
| SEARCH-8328                                                                                                                                                    | It’s no longer possible to unintentionally overwrite an existing [Notification](notifications) by duplicating a Notification ID.                                                                    |
| SEARCH-10078                                                                                                                                                   | Pack resources can now be deleted only from within their Packs.                                                                                                                                     |
