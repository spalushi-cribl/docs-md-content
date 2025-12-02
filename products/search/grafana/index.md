# Use Cribl Search with Grafana


Visualize your search results in Grafana, using the official Cribl Search plugin.

---

In addition to the [Charts](charting) and [Dashboards](dashboards) included in Cribl Search, you might want to present
your search results using Grafana's visualization tools. For that, use the official
[**Cribl Search plugin for Grafana**](https://grafana.com/grafana/plugins/criblcloud-search-datasource/).


![Cribl Search plugin for Grafana](grafana_adhoc.png)
{scale="100%"}

Once the plugin is [configured](#connect), you can:

- [Run](#run) any Cribl Search query directly in Grafana.
- [Feed](#schedule) your Grafana dashboards with up-to-date data, using [scheduled searches](scheduled-searches).

## Connect Grafana to Cribl Search {#connect}

To start using Cribl Search with Grafana, you need Organization administrator permissions in Grafana, and Owner or Admin
permissions in Cribl.Cloud.

1. In Grafana, [install the Cribl Search plugin](#install).
1. In Cribl.Cloud, [get a Cribl.Cloud credential](#cred).
1. In Grafana, [add a Cribl Search data source](#source).

### Install the Cribl Search Plugin in Grafana {#install}

To install the Cribl Search plugin in Grafana:

1. [Log in](https://grafana.com/auth/sign-in) to Grafana as an Organization administrator.
1. Go to the [Cribl Search plugin page](https://grafana.com/grafana/plugins/criblcloud-search-datasource), and select
   **Install**.

### Create a Cribl.Cloud Credential for Grafana {#cred}

To let Grafana pull data from Cribl Search, [create an API Credential in Cribl.Cloud](/api/#criblcloud):

1. Log in to [Cribl.Cloud](https://manage.cribl.cloud/), and select an Organization for which you're an Owner or Admin.
1. Go to **Account** > **Organization** > **API Management**.
1. Select **Add Credential**, and follow the steps to [create an API Credential](/api/#criblcloud).
1. Once you created the credential, keep the following at hand:
   - Client ID.
   - Client Secret.
   - The URL of your Cribl.Cloud Organization (for example, `https://<workspaceName>-<organizationId>.cribl.cloud`).

### Add a Cribl Search Data Source in Grafana {#source}

With the Cribl Search plugin [installed](#install), and your Cribl.Cloud [credential](#cred) ready, add a Cribl Search
data source in Grafana:

1. [Log in](https://grafana.com/auth/sign-in) to Grafana as an Organization administrator.
1. In Grafana, follow [Grafana's instructions](https://grafana.com/docs/grafana/latest/datasources/#add-a-data-source)
   for adding a new data source. Select the
   [Cribl Search plugin](https://grafana.com/grafana/plugins/criblcloud-search-datasource) as the data source type.
1. On the Cribl Search plugin page:

   - In **Cribl Organization URL**, enter the URL of your Cribl.Cloud Organization (for example,
     `https://<workspaceName>-<organizationId>.cribl.cloud`).
   - In **Cribl Client ID**, enter the Client ID of the [Cribl.Cloud credential](#cred).
   - In **Cribl Client Secret**, enter the Client Secret of the Cribl.Cloud credential.

1. Select **Save & test**.


![Cribl Search plugin configuration](grafana_plugin.png)
{scale="90%"}

## Run a Cribl Search Query in Grafana {#run}

Once you [set up](#connect) the Cribl Search plugin, you can run any Cribl Search query right there in Grafana:

1. In Grafana, go to your [Cribl Search data source](#source).
1. In **Query Type**, select **adhoc**.
1. In **Query**, enter your Cribl Search [query](your-first-query).
   
   > Your query can include [Grafana variables](https://grafana.com/docs/grafana/latest/dashboards/variables/).
   {.box .success}
1. At the top right, adjust the time range.
1. Select **Run query**.

## Schedule Searches to Load Grafana Dashboards Faster {#schedule}

To render your Grafana dashboards faster, keep them up to date by automatically pulling the latest results of
[scheduled searches](scheduled-searches) from Cribl Search.

First, get the ID of the scheduled search that you want to visualize in Grafana:

1. From the Cribl Search sidebar, select [**Saved Searches**](search-overview#saved).
1. Open an existing saved search, or select **Add Search** to save a new one.
1. Set up your scheduled search, by following this section: [Schedule a Search](scheduled-searches#schedule-a-search).
1. At the top, copy the scheduled search **ID** (for example, `cribl_search_finished_1h`).

With the scheduled search ID ready, go to Grafana:

1. In Grafana, go to your [Cribl Search data source](#source).
1. In **Query Type**, select **saved**.
1. In **Saved Search**, enter the ID of your Cribl Search scheduled search.
1. Select **Run query**.

Now, Grafana will automatically fetch the latest results of your scheduled search. When you open Grafana, the dashboard
dependent on your search will load immediately, since your Cribl Search results are already there.


![Pulling scheduled search results in Grafana](grafana_scheduled.png)
{scale="100%"}

## Get Grafana Alerts Based on Cribl Search Results {#alerts}

You can create [Grafana alert rules](https://grafana.com/docs/grafana/latest/alerting/) based on results produced by
Cribl Search. For example, you can get Grafana alerts when the number of your Cribl Search results crosses a certain
threshold, or when a result set contains a specific value.

1. Make sure your Grafana instance is [connected](#connect) to Cribl Search.
1. When [configuring an alert rule](https://grafana.com/docs/grafana/latest/alerting/alerting-rules/) in Grafana, select
   a [Cribl Search data source](#source), and add your [ad-hoc](#run) or [scheduled](#schedule) Cribl Search query.

For more information on setting up alerts, see
[Grafana's documentation](https://grafana.com/docs/grafana/latest/alerting/).
