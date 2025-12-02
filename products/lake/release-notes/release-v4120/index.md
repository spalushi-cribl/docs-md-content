# Cribl Lake 4.12.0


## New Features

This release includes the following enhancements:

### Direct Archiving of Splunk Journal Files

A new Splunk Cloud Self Storage (DDSS) [Direct Access](direct-access) integration enables you to archive Splunk journal files directly to Cribl Lake, without routing through Cribl Stream.

### Lakehouse Flexible Retention Window

For Datasets newly [assigned to a Lakehouse](lakehouse#link-datasets-to-a-lakehouse), the retention window now begins at the time events are ingested into the Lakehouse, rather than being determined by timestamps on the events. This enables you to search historical data with Lakehouse acceleration, for purposes like incident investigation.

### Lakehouse Throughput and Capacity Indicators

The [Lakehouses](lakehouse#monitor-lakehouse-usage) page has new columns showing maximum throughput and trailing 24-hour ingestion rate.

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

## Corrections {#corrections}

This release includes the following fixes:

| ID | Description |
| -- | ------------|
| <div style="width: 100px"> LAKE-772 | Lakehouse now correctly handles Lake datasets using the Parquet data format. Events returned from Search queries now show all fields as expected. |

See also corrections in [Cribl Search Release Notes](https://docs.cribl.io/search/release-notes/release-v4120#corrections).
