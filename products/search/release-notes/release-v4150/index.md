# Cribl Search 4.15.0


Cribl Search 4.15.0 adds new encryption functions, unique filenames for search results sent by email, [`export`](export)
operator working with BYOS Lake Datasets, and more.

## New Features

### Notebooks Are Generally Available

Cribl Search [Notebooks](notebooks) are now generally available and no longer in Preview. They also get several new features. For example:

- A new default Notebook template includes sample data with no configuration needed, so you can get started quickly and
  create an editable copy for your own use.
- Search cells now feature a new **View Mode**, so you can display the results in the most convenient way (chart only,
  table only, or both).
- Note cells now support Markdown block quotes, code blocks, and tables.
- In a search cell, hover over the **Last run** info to see time-range and sampling information.

### Cryptographic Functions

With the new [`encrypt`](encrypt) and [`decrypt`](decrypt) functions, you can selectively encrypt and decrypt data using
[keys managed by a Stream Worker Group](https://docs.cribl.io/stream/securing-encryption-keys). This means that Cribl
Search now decrypt fields that were first
[encrypted in Cribl Stream](https://docs.cribl.io/stream/securing-data-encryption).

### Export Operator Supports BYOS Lake Datasets

The Cribl Search [`export`](export) operator can now write to Lake Datasets in external
[Storage Locations](https://docs.cribl.io/lake/byos/) (BYOS).

### Unique Filenames for Search Results Sent by Email

If your email Notifications include search results as an attached CSV or JSON file, each file now has a unique name that
includes the ID of the related search, a timestamp, and a random string.

### Concurrency Limits Accept `%` Values

You can now set the following [Usage Group limits](usage-groups#usage-limits) as a percentage of the Leader’s CPU
capacity:

- **Concurrent ad hoc search limit**
- **Concurrent scheduled search limit**
- **Overall concurrent search limit**

### Dashboards Enhancements

We made Dashboards easier to use. For example, now you can:

- Auto-apply your Dashboard Inputs changes, without having to confirm them. To turn this on, select **Apply Input
  Changes Automatically** from the Dashboard’s three-dots Actions menu.
- Hide Chart labels, using the saved space to make the graphic larger. To do this for a Column, Donut, Funnel,
  Horizontal Bar, or Line Chart, select **X-Axis** > **Position** > **None**. For a Single Value Chart, just leave the
  **Label** field empty.

See also the new doc page on Dashboard Inputs: [Add Inputs to Your Cribl Search Dashboard](dashboards).

### Temporary Log Level Settings

Use a configurable TTL (time-to-live) setting to temporarily change log levels for up to 24 hours and debug without overwhelming your logs or incurring unnecessary storage costs. The log level automatically reverts to the permanent setting after the TTL setting expires.

## Corrections

| ID                                                         | Description                                                                                                                                                                                                                                                                                                                          |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <div style="width: 100px">CRIBL-ID</div>SEARCH-10703</div> | Notifications can now reference individual fields of the results set (using `{{resultSet[0].field}}`) regardless of the **Include search results** option. You need that option only to attach the full available sample of the results, or, in case of PagerDuty and Slack, reference that full set inline (using `{{resultSet}}`). |
| SEARCH-11154                                               | Cribl Search now enforces a maximum per-field payload size to protect search stability and API responsiveness; this limit applies to fields like `_raw`. Oversized fields are truncated during results rendering. The limit is not configurable and applies to all searches, with a default of `2` MB per field.                     |
| SEARCH-11290                                               | Using [`mv-expand`](mv-expand) in a Lakehouse query now returns the same number of results as without Lakehouse.                                                                                                                                                                                                                     |
