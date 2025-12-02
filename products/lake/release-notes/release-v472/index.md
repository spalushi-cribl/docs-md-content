# Cribl Lake 4.7.2


## New Features

### Cribl Lake Roles

Starting with v.4.7.2, Cribl Lake takes full advantage of the Cribl [access management](/stream/members) system.

You can now control access to Cribl Lake by granting Members and Teams product-level Permissions.
Read Only Permission allows a user to view all Datasets within Cribl Lake.
An Editor can create and update Datasets as well, while an Admin has full control over Datasets, including deleting them.

See [Cribl Lake Permissions](lake-permissions) for more information.

### Parquet Support

Cribl Lake now supports an additional data format, Parquet, besides the earlier JSON.
Parquet is a columnar storage format ideal for big data analytics.
You can select a storage format for each Dataset you create.

### Data Retention

Default retention period for new Cribl Lake Datasets is now extended to 1 year.

## Corrections

Metric events in the `cribl_metrics` Dataset now appear in the correct format with metric value, along with additional dimensions.

Organization Admins can now configure Cribl Lake Datasets.

