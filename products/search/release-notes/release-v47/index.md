# Cribl Search 4.7


Starting with Cribl Search 4.7, you can search Snowflake warehouses and Prometheus metrics, attach search results to
email notifications, share Datasets with multiple users at once, and more.

### Snowflake Integration

You can now connect to [Snowflake](https://www.snowflake.com), by setting up Snowflake
[Data Providers](set-up-snowflake#provider) and [Datasets](set-up-snowflake#dataset).

### Prometheus Integration

You can now search your [Prometheus](https://prometheus.io/) metrics, by setting up Prometheus
[Data Providers](set-up-prometheus#provider) and [Datasets](set-up-snowflake#dataset).

### Email Notifications Can Include Results in CSV or JSON

[Email Notifications](email-notifications) can now include attachments with search results sets in CSV or JSON.

### Sharing with Teams of Users

Search Admins can now use Teams to [share Datasets](sharing#share-datasets-and-dataset-providers), Dataset Providers, and
[search results](sharing#share-search-results) with multiple users at once.

### TTL for Dashboards

You can now set the time-to-live (TTL) for [Dashboards](dashboards), to define how long Dashboard data should be cached.

### Generic HTTP APIs: Dynamic Query Parameters

You can now dynamically query different HTTP API endpoints without having to change the Dataset Provider configuration,
by using [dynamic query parameters](set-up-generic-http-api#parameters).

### Generic HTTP APIs: Snapshots

You can now create [Snapshots](set-up-generic-http-api#snapshots) of your Generic HTTP API Datasets, and store them in
[Cribl Lake](/lake/). This lets you see trends in how your data changed over time, and ensures consistent search
performance.

### New Global Navigation (Opt-In)

Building on the last release, in which we introduced the new Cribl.Cloud experience with
[Workspaces](/stream/workspaces), we’ve now added a global navigation system. Now when you opt in to the new Cribl.Cloud
user interface, you'll get a streamlined global navigation experience. When you open a Cribl product in a Workspace, a
product navigation pane on the left helps you move around within that product. Click Products to switch to a different
product or go back to Cribl.Cloud home.
