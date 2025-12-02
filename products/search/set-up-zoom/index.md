# Connect Cribl Search to Zoom API


Configure Cribl Search to query a Zoom API endpoint.

---

[Zoom](https://zoom.us/) enables users to communicate via video, audio, phone, and chat.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to [search](search-overview) the [user](https://developers.zoom.us/docs/api/rest/reference/zoom-api/methods/#tag/Users),
[group](https://developers.zoom.us/docs/api/rest/reference/zoom-api/methods/#tag/Groups), and [meeting](https://developers.zoom.us/docs/api/rest/reference/zoom-api/methods/#tag/Meetings) endpoints in your Zoom
account.

## Add a Zoom API Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add a Zoom API
Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.

Set the following configurations in the **New Dataset Provider** modal:

1. **ID** is a unique identifier for the Dataset Provider. This is how you'll reference it when assigning Datasets to
   it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. **Description** is optional.
1. Set **Dataset Provider Type** to **Zoom API**.
1. Select **Add Configuration** to specify your Zoom account. In the **Account Configuration** table, enter:
   - **Account Name**: Zoom account name.
   - **Account ID**: Unique ID associated with your Zoom account.
   - **Client ID**: Zoom Client ID.
   - **Client Secret**: Zoom Client Secret.
1. Select **Save** when finished.

For details on obtaining your Zoom credentials, see
[Server-to-Server OAUTH](https://developers.zoom.us/docs/zoom-rooms/s2s-oauth/).

## Add a Zoom API Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.

Set the following configurations in the **New Dataset** modal:

1. **ID** is an identifier unique for both Cribl Search and [Cribl Lake](/lake/datasets). You'll use this to specify the
   Dataset in a query's [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. **Description** is optional.
1. Set **Dataset Provider** to the **ID** of a [Zoom Dataset Provider](#provider).
1. Select **Add endpoint** to select the endpoints for your Dataset.
1. **Enabled endpoints**: Select an endpoint from the drop-down menu. Your options are:
   - users
   - groups
   - meetings
   - pastMeetings
1. In **Processing**, you can apply rules for breaking data into discrete events. For more information, see
   [Datatypes](datatypes).
1. In **Snapshots**, you can set up [API Snapshots](set-up-apis#snapshots).
1. Select **Save** when finished.

## Search Zoom API

Now that you have a Dataset Provider and Dataset, you're ready to start [searching](search-zoom).

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.
