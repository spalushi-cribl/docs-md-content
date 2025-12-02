# Cribl Search 4.4.4


## New Features

Starting with Cribl Search 4.4.4, you can configure Splunk SmartStore faster than before, reuse your search results, and declutter your charts.
Read on for details.

### Configure Splunk SmartStore Faster

In [Dataset Providers](basic-concepts#dataset-providers) that support Splunk SmartStore, you can now select SmartStore as the partitioning scheme
without having to specify further configuration. This applies to the following providers:
* [Amazon S3](set-up-s3)
* [Azure Blob Storage](set-up-azure-blob)
* [Cribl Edge](set-up-edge)
* [Google Cloud Storage](set-up-google-cloud-storage)

### Reuse Your Search Results

Your queries can now [reuse](search-results#reuse) the results of previous searches (with no need to rerun them), using the new [virtual table](virtual-tables): `$vt_results`.

### Visualizations

* You can now choose to display only the larger (top N) values of a Dataset in area, bar, donut, funnel, and line charts.
* Search results are now sorted in descending time order by default, presenting the latest information at the top.