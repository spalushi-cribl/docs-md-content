# Lake Datasets


Organize different types of data stored in Cribl Lake, using Lake Datasets.

---

Cribl Lake comes with ready-to-use default Lake Datasets and lets you create
your own Datasets.

You can have up to 200 Lake Datasets per Workspace. If you need more, reach out to [Cribl Support](/support/).

The data is stored as gzip-compressed JSON files.

## Lake Datasets Retention {#retention}

Every Lake Dataset stores data for a defined retention period.

For [built-in Datasets](#built-in-datasets), the retention period is fixed to 10-30 days,
depending on the Dataset.

For your own Datasets, you can [configure the retention period](managing-datasets) depending on your needs.

### How Lake Datasets Retention Is Calculated

The retention period of a Lake Dataset is based on the date on which data was uploaded and saved,
not on the dates of individual stored events.

This distinction is important if you are uploading older events in a batch.
Retention will then be calculated based on the date of the upload.

For example, if on the 1st Aug 2024 you upload a batch of data
including events dated at 1st June 2024, and set the retention period to 1 year,
the events will be deleted after 1st Aug 2025 (based on upload date),
not in June 2025.

## Built-in Lake Datasets

The following Lake Datasets are available by default in Cribl Lake:

| Lake Dataset | Contains | Retention Period (Days) | Notes |
| ------- | ----------- | ----------------------- | ----- |
| `default_logs` | Logs from multiple sources. | 30 ||
| `default_metrics` | Metrics from multiple sources. | 15 ||
| `default_spans` | Distributed trace spans from multiple sources. | 10 ||
| `default_events` | Events from sources such as Kubernetes or a third-party API. | 30 ||
| `cribl_metrics` | Metrics from the Cloud Leader. Data is stored for 30 days free of charge. | 30 | Cannot be targeted by a Destination. |
| `cribl_logs` | Logs from the Cloud Leader. Data is stored for 30 days free of charge. | 30 | Cannot be targeted by a Destination. |

### Audit and Access Logs

Cribl Lake automatically collects audit and access logs, and metrics from the Cloud Leader Node
(a node that manages the whole Cribl deployment).
This data is stored in the `cribl_logs` and `cribl_metrics` Lake Datasets.

Because these Lake Datasets are internal, you can't use them as a target in the [Cribl Lake Destination](/stream/destinations-cribl-lake/).

You can't edit or delete built-in Lake Datasets.
