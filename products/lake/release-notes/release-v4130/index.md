# Cribl Lake 4.13.0


Cribl Lake 4.13.0 includes significant performance improvements and new capabilities.

## Important Changes

These changes to Cribl Lake might affect your Lakehouse data retention and might change the behavior of existing
[Cribl Search](/search/) queries, especially on saved searches.

### Lakehouse Data Retention Times Are Now Based on Ingest Time

Within each Lakehouse, the data's **Retention period** is now measured based on `ingestion_time` (the clock time at
which events were ingested into the Lakehouse), rather than based on `event_time` (each event's `_time` field value).
This enables some new use cases, and makes Lakehouse behavior consistent with Cribl Lake behavior.

### Lakehouse Attached to an Empty Dataset Includes More Events

When you attach a Lakehouse to any **empty** Lake Dataset, the Lakehouse will now mirror all of the Dataset's data, for
the retention period of the Lakehouse. This provides several advantages:

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

**What you need to do:** Plan to upgrade your Cribl deployment to the latest version by September 2025 to align with
this change.

## New Features

This release adds the following new capabilities.

### New Larger Lakehouses Support Higher Ingest Rates

Cribl Lakehouse now supports two new Dataset [sizes](lakehouse#lakehouse-sizes):

| Size    | Ingest Rate     |
| ------- | --------------- |
| 3XLarge | Up to 14 TB/day |
| 6XLarge | Up to 28 TB/day |

The new tiers are available in the US and Germany, and are not visible in the Cribl Lake UI at this point. To start
using them, contact [Cribl Support](/support/).

### Direct Access Graduates from Preview

The [Direct Access](direct-access) option for ingesting Splunk Cloud DDSS data has shed its Preview badging and
disclaimer. It's here to stay.
