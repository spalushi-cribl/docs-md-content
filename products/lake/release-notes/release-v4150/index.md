# Cribl Lake 4.15.0


Cribl Lake 4.15.0 expands our “bring your own storage” feature (external Storage Locations) with a new KMS encryption option, a new data ingest option using the Cribl Search `export` operator, and more error-proof mapping of Datasets to buckets. We’ve also added an Enterprise option that helps you monitor each Dataset’s storage usage.

## New Features

### KMS Encryption Options on S3 Buckets

When you create an external [Storage Location](byos) on Amazon S3, you can now encrypt your bucket using either the default `SSE-S3` key management scheme, or an `SSE-KMS` alternative KMS scheme. Choosing `SSE-KMS` can provide additional security and auditability, through greater control over key policies, access, and rotation.

### Lakehouse Retention No Longer Limited to 30 Days

Lakehouse retention is no longer limited to 30 days. The retention period for a Lakehouse now matches the retention period of the linked Lake Dataset.

### Search Export Operator Embraces Storage Locations 

The Cribl Search [`export` operator](/search/export/#dataset) can now write to Lake Datasets in external Storage Locations. (It is no longer limited to Lake-native Datasets.) 

### Datasets Embrace Storage Locations

When you [create a Dataset](managing-datasets#create) after your Workspace has at least one external bucket configured, the **Storage Location** drop-down no longer defaults to `Cribl Lake`. `Cribl Lake` remains available, but to help prevent errors, you now must actively choose between it and a specific external location. 

### Dataset Size Indicators (Storage Insights)

With an Enterprise plan, the **Datasets** page and individual Datasets now show the **Size** of each configured Dataset. This supplements your billing data by breaking down the storage footprint of each Dataset – helping you to plan capacity and optimize storage costs.

### Temporary Log Level Settings

Use a configurable TTL (time-to-live) setting to temporarily change log levels for up to 24 hours and debug without overwhelming your logs or incurring unnecessary storage costs. The log level automatically reverts to the permanent setting after the TTL setting expires.

## Corrections

This release contains the following bug fixes:

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-ID</div>LAKE-1275</div> | Upon deleting data from an S3 Storage Location, the Cribl Lake UI now clears the "delete in progress" state, as intended. |