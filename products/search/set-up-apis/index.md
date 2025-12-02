# Connect Cribl Search to an API


Explore data coming from an API, and create Snapshots for better performance and observability.

---

## API Dataset Provider Types

To quickly connect to your API, use a dedicated Dataset Provider type:

- [AWS API](set-up-aws)
- [Azure API](set-up-azure-api)
- [Google Cloud Platform API](set-up-google-cloud-platform)
- [Google Workspace API](set-up-google-workspace-api)
- [Microsoft Graph API](set-up-microsoft-graph-api)
- [Okta API](set-up-okta)
- [Tailscale API](set-up-tailscale)
- [Zoom API](set-up-zoom)

To connect to any other HTTP API, create a [Generic HTTP API](set-up-generic-http-api) Dataset Provider.

## API Snapshots {#snapshots}

When searching an API Dataset, what you get is the current state of the queried system. But what if you want to see how
that state changes over time? You can do this by taking Dataset **Snapshots**.

To create Snapshots, Cribl Search runs [scheduled searches](scheduled-searches) on your API Dataset, and then keeps the
results of those searches in [Cribl Lake](/lake/). This allows you to:

- See trends in how your data changes over time.
- Ensure consistent search performance by avoiding API throttling of the queried endpoints.

### Enable Snapshots

You can enable Snapshots for any API Dataset:

1. In Cribl Search, go to **Data**, then **Datasets**.
1. Open an API Dataset.
1. Select **Snapshots**, and toggle **Enable Snapshots** to **Yes**.
1. Select a [**Cribl Lake Dataset**](/lake/datasets) where you want to store the Snapshots. To quickly
   [add](/lake/datasets#create-a-new-dataset) a new Lake Dataset, select **Open Lake**.
1. In **Schedule to run**, configure the time schedule for creating Snapshots. This is how often Cribl Search will run
   [scheduled searches](scheduled-searches) on your Dataset. Use the drop-downs, or enter a
   [cron](https://en.wikipedia.org/wiki/Cron) expression. You can double-check your configuration by looking at the
   **Estimated schedule** below.
1. Select **Save**.

Now, Cribl Search will systematically query your Dataset and keep the results in [Cribl Lake](/lake/). You can use those
results like any other Dataset: [searching](search-cribl-lake) them, running [scheduled searches](scheduled-searches),
setting up [Notifications](notifications), or creating [Dashboards](dashboards).
