# Connect Cribl Search to Tailscale API


Configure Cribl Search to query a Tailscale API endpoint.

---

[Tailscale](https://tailscale.com/) VPN makes your devices and applications accessible from anywhere in the world,
securely and easily.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to [search](search-overview) the [acls](https://github.com/tailscale/tailscale/blob/main/api.md#policy-file) and
[devices](https://github.com/tailscale/tailscale/blob/main/api.md#device) endpoints in your Tailscale account.

## Add a Tailscale API Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add a Tailscale API
Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.

Set the following configurations in the **New Dataset Provider** modal:

1. **ID** is a unique identifier for the Dataset Provider. This is how you'll reference it when assigning Datasets to
   it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. **Description** is optional.
1. Set **Dataset Provider Type** to **Tailscale API**.
1. Select **Add Configuration** to specify your Tailscale account. In the **Account Configuration** table, enter:
   - **Account Name**: Tailscale account name.
   - **Client ID**: Tailscale Client ID.
   - **Client Secret**: Tailscale Client Secret.
1. Select **Save** when finished.

For details on obtaining your Tailscale credentials, see
[Authentication](https://github.com/tailscale/tailscale/blob/main/api.md#authentication/).

## Add a Tailscale API Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.

Set the following configurations in the **New Dataset** modal:

1. **ID** is an identifier unique for both Cribl Search and [Cribl Lake](/lake/datasets). You'll use this to specify the
   Dataset in a query's [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. **Description** is optional.
1. Set **Dataset Provider** to the **ID** of a [Tailscale Dataset Provider](#provider).
1. Select **Add endpoint** to select the endpoints for your Dataset.
1. **Enabled endpoints**: Select an endpoint from the drop-down menu. Your options are:
   - [acls](https://github.com/tailscale/tailscale/blob/main/api.md#policy-file)
   - [devices](https://github.com/tailscale/tailscale/blob/main/api.md#device)
1. In **Processing**, you can apply rules for breaking data into discrete events. For more information, see
   [Datatypes](datatypes).
1. In **Snapshots**, you can set up [API Snapshots](set-up-apis#snapshots).
1. Select **Save** when finished.

## Search Tailscale API

Now that you have a Dataset Provider and Dataset, you're ready to start [searching](search-overview).

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.

