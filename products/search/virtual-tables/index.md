# Virtual Tables


Use virtual tables to dynamically generate system information useful for troubleshooting, performance analysis, and
testing.

---

## Available Virtual Tables

| Name                                            | Description                                                                                                                         |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| [`$vt_datasets`](vt_datasets)                   | Lists available [Datasets](basic-concepts#datasets). Useful for finding Datasets of a specific type or with specific name patterns. |
| [`$vt_dataset_providers`](vt_dataset_providers) | Lists available [Dataset Providers](basic-concepts#dataset-providers). Useful for a quick overview of your Dataset Providers.       |
| [`$vt_dummy`](vt_dummy)                         | Generates dummy data. Useful for testing your queries.                                                                              |
| [`$vt_jobs`](vt_jobs)                           | Lists previously run searches. Useful for troubleshooting or performance analysis.                                             |
| [`$vt_list`](vt_list)                           | Lists available virtual tables. Useful for looking up the names and descriptions of other virtual tables.                           |
| [`$vt_lookups`](vt_lookups)                     | Lists available [lookups](lookups), or gets the contents of a specific lookup. Useful for searching through your lookup files.      |
| [`$vt_results`](vt_results)                     | Loads the results of one or more previous searches, so you can [reuse](search-results#reuse) them.                                 |


