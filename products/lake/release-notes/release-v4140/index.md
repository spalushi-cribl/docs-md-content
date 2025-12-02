# Cribl Lake 4.14.0


Cribl Lake 4.14.0 includes new data storage and ingest options, faster default response to queries on Lakehouse Datasets, and bug fixes.

## Important Changes

These changes to Cribl Lake might change the behavior of existing [Cribl Search](/search/) queries, especially on saved searches.

### Lakehouse Datasets Default to Lakehouse Query Execution

When you use Cribl Search to query a Cribl Lake Dataset that’s assigned to a [Lakehouse](lakehouse), queries now default to Lakehouse execution. This returns results faster and more efficiently, but limits the results to the past 30 days, even if the underlying Dataset has a longer retention period. To search back beyond 30 days, precede your query with `set lakehouse="off";` to override the new default.

## New Features

This release adds the following new capabilities:

### Storage Locations (Bring Your Own Storage) – Amazon S3

Storage Locations, new in this release, enable you to create Cribl Lake Datasets on Amazon S3 buckets that you manage. This way, you can maintain direct ownership of your data for compliance purposes, while taking advantage of Cribl’s streamlined provisioning, access control, and analytics. In this release, Datasets created outside Cribl Lake will not be visible or accessible to Cribl Stream or Search.

### Direct Access (HTTP)

Cribl Lake [Direct Access](direct-access) adds a new HTTP option, enabling you to ingest data directly into defined Lake Datasets from a wide range of HTTP senders. This option bypasses the need for Cribl Stream routing. There are specific configuration settings for Splunk HEC and Elasticsearch Bulk API senders.

### Cribl Terraform Provider (Preview)

The [Cribl Terraform provider](https://registry.terraform.io/providers/criblio/criblio/latest/docs) is now available in Preview from the Terraform Registry. Use this `criblio` provider to create Cribl Lake resources as part of your infrastructure-as-code workflows.

### New Cribl.Cloud Regions

We are excited to now support creating Cribl.Cloud Organizations in Switzerland and Singapore AWS Regions (`eu-central-2`, `ap-southeast-1`).

## Corrections

This release contains the following bug fixes:

| ID | Description |
| -- | ----------- |
LAKE-843 | You can now edit the description of a DDSS (Direct Access Splunk Self Storage) Source once it has been created.
LAKE-919 | Bulk-deleting multiple Datasets at once (with appropriate Permissions) now completely deletes the whole set.
